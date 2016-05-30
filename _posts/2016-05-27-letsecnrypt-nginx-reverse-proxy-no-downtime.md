---
title: "Let's Encrypt Auto-Renewal for Nginx Reverse Proxies"
layout: post
date: 2016-05-27 11:03
tag:
- letsencrypt
- privacy
- nginx
- proxy
- ssl
blog: true
---

## Motivation

_If you are familiar with using Nginx as a reverse proxy and have already used Let's Encrypt, skip to "[Provisioning a Server](#provisioning-a-server)"._

#### Nginx

A very useful feature of [nginx](https://www.nginx.com) is that you can host multiple services on the same host and the same IP. For example, you could have a [Node.js](https://nodejs.org) application running on `localhost:8080`, a [Jenkins CI](https://jenkins.io) service running on `localhost:8088` and any other combination of web services written in various languages.

These local ports are not exposed to the pubic internet. Instead, nginx listens on port 80/443 and proxies the relevent service based on what domain name is being requested. For example, a request to `jenkins.example.com` would be proxied to `localhost:8080` in our earlier example.

In order to secure our connection with the client, HTTPS connections can be set up in the nginx configuration. The local services can safely serve plaintext HTTP since all connections are local.

#### Let's Encrypt

[Let's Encrypt](https://letsencrypt.org) is a new certificate authority which aims to provide free, automated SSL certificates. All the major browsers have now added Let's Encrypt to their default list of trusted providers. This means that site operators who wanted to serve HTTPS but didn't want to pay for a certificate now can.

This is actually very exciting for ordinary internet users' privacy and security!

#### Certbot

Let's Encrypt provide an automated CLI tool to obtain certificates called [Certbot](https://github.com/certbot/certbot). Certbot proves to the Let's Encrypt certificate authority that you own the domain by simply listning on port 80/443 and having the certificate authority make a request.

If you're trying to acquire a certificate for `newdomain.example.com` then Let's Encrypt will just resolve that domain name and make a request. If they receive the response they're expecting then this proves that whoever is running certbot does control that domain.

There are currently three modes you can use to obtain your certificate:

1. `Apache` - Certbot is able to plug-in directly to the [Apache](https://httpd.apache.org) web server and allow Let's Encrypt to make their request without any down time for what's normally hosted there. This is an ideal solution for Apache users.
2. `Webroot` - If you're not hosting your site with Apache, but have write access to the directory which serves as the web-root for the service, Certbot can drop some files in here and they'll be served up when Let's Encrypt comes knocking just as normal. Again, no interruption in service.
3. `Standalone` - Certbot will directly listen on the required port. This obviously requires some down-time for whatever service is normally listening on that port.

Unfortunately, a lot of the time when we're using nginx as a reverse proxy, it (at first) appears that we need to use Standalone. For example, if you're proxying a service that's running in a [Docker](https://www.docker.com) container, then you don't have access to the web-root.

To make things worse, every time you need to renew the certificate for that one service, you need to stop nginx (severing the link to all other services too) so that Certbot can listen on those ports.

Fortunately, I'm going to show you a way around this. There is an easy way to set up your services behind an nginx reverse proxy and still get the benefits of automated certificate renewal.

## Provisioning a server

For this example I'm starting out by spinning up a fresh [Ubuntu](http://www.ubuntu.com) instance on [Amazon EC2](https://aws.amazon.com) (t2.micro). I've assigned an Elastic IP and, to begin with, allowed all incoming traffic via its security group. I've also pointed `ssltest.busby.ninja` at the instance. I'll be including all installation commands so this tutorial can be followed exactly.

## Setting up the services

For this example I'm going to use a [Rancher](http://rancher.com) server as my example service. I'm doing this precisely because it runs in the Docker container, so I can't use Certbot's Apache mode, and I don't have access to the service's web root directory. 

The first thing we need to do after SSHing into the instance is install nginx and docker:

{% highlight shell %}
sudo apt-get update && sudo apt-get install nginx -y
wget -qO- https://get.docker.com/ | sh
# The next command is optional, it allows you to run docker without sudo
# Replace ubuntu with whatever your user is called
sudo usermod -aG docker ubuntu
{% endhighlight %}

After this, double-check that you see the default nginx page when you navigate to the domain. If this works correctly, then the next thing to do is start up our rancher server. Retrieving and running the server can be done with a single docker command:

{% highlight shell %}
# If you want to listen on another port, change only the first 8080
cid=$(sudo docker run -d --restart=always -p 8080:8080 rancher/server)
{% endhighlight %}

It may take a little while for rancher to be downloaded and run. Once the previous command has terminated, you can watch Rancher's progress as it boots up using the following command:

{% highlight shell %}
sudo docker logs -f $cid
{% endhighlight %}

When you see `Connection established` in the logs, it should be ready and listening. Assuming you didn't change it, you can test that by calling port 8080. In my case that means navigating to [http://ssltest.busby.ninja:8080](http://ssltest.busby.ninja:8080).

## Basic nginx configuration

Now that we have nginx and rancher running, we can configure it as a basic reverse proxy that supports HTTP (but not HTTPS yet). You'll want to use `sudo su -` to login as root, then navigate to `/etc/nginx/sites-available`.

Below are the two configuration files which you need to create in this directory. The first ensures that if you enter the server's IP into the browser, or nginx doesn't recognise the domain in the HTTP host, then it will refuse to serve any content. The second is the configuration for our reverse proxy to rancher.

<p style="margin-bottom: 0; font-weight: bold"><code>/etc/nginx/sites-available/default</code></p>
{% highlight nginx %}
server {
    deny  all;
}
{% endhighlight %}

<p style="margin-bottom: 0; font-weight: bold"><code>/etc/nginx/sites-available/rancher</code></p>
{% highlight nginx %}
upstream rancher {
    # change this if you used a port other than 8080
    server 127.0.0.1:8080 fail_timeout=0;
}

server {
    listen 80;
    listen [::]:80;

    server_name ssltest.busby.ninja; # replace this with your domain

    location / {
        # Some of these might not be required for other services
        # proxy_pass is the most important
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header X-Forwarded-Port $server_port;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_pass http://rancher;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_read_timeout 900s;
    }
}
{% endhighlight %}

Next you need to symlink these into `/etc/nginx/sites-enabled` and restart the server.

{% highlight shell %}
cd ../sites-enabled
ln -sf ../sites-available/default
ln -sf ../sites-available/rancher
sudo service nginx restart
{% endhighlight %}

If restarting nginx fails, then the logs are at `/var/log/nginx/error.log`. If it succeeds, you should now be able to navigate to your domain (in my case [http://ssltest.busby.ninja](http://ssltest.busby.ninja)) and see rancher load as if you'd accessed it directly. If you see the nginx default page again, try refershing, it may be the browser cache. You should also try accessing the server directly by IP (in my case [http://52.51.228.42](http://52.51.228.42)). If all has gone well you'll see a 403 Forbidden message.

## Setting up a web-root

As discussed in the introduction, we will be using the Certbot tool that Let's Encrypt provide.

* While we could use the standalone mode, this would involve shutting down nginx while it runs and disrupting service to rancher.
* We can't use the apache mode since we're using nginx and rancher.
* We don't have access to rancher's web-root so we also can't use the webroot mode...

...or can we.

This is one of the ways in which nginx is really very cool. It's possible for us to configure a separate web-root in our `/etc/nginx/sites-available/rancher` file. When a request is made, nginx firstly checks to see if that path exists as a static file in the web-root directory we specified. If it exists, nginx acts like a regular web server and serves it. If that path doesn't exist in the web-root then the entire request is proxied over to rancher as normal.

Firstly let's create the webroot (I'm assuming you're still root here):

{% highlight shell %}
mkdir -p /var/www/rancher-certbot-webroot
# Create file we'll use later to test 
echo "nginx is awesome!" > /var/www/rancher-certbot-webroot/test.html
# Set the ownership of the directory to be the user you'll run certbot as
chown -R ubuntu:ubuntu /var/www/rancher-certbot-webroot
{% endhighlight %}

Next we need to modify our nginx config:

<p style="margin-bottom: 0; font-weight: bold"><code>/etc/nginx/sites-available/rancher</code></p>
{% highlight nginx %}
upstream rancher {
    # change this if you used a port other than 8080
    server 127.0.0.1:8080 fail_timeout=0;
}

server {
    listen 80;
    listen [::]:80;

    server_name ssltest.busby.ninja; # replace this with your domain

    # Here we define the web-root
    root /var/www/rancher-certbot-webroot;

    location @proxy {
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header X-Forwarded-Port $server_port;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_pass http://rancher;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_read_timeout 900s;
    }

    location / {
        # This line looks for the request path in webroot
        # If it fails, it uses @proxy (defined above)
        try_files $uri @proxy;
    }
}
{% endhighlight %}

Restart nginx and access /test.html ([http://ssltest.busby.ninja/test.html](http://ssltest.busby.ninja/test.html) for me). Should should see a message correctly stating how awesome nginx is. You should also check that other paths are still redirecting to rancher. If you're a clean-freak like me you'll probably want to delete test.html after confirming that everything is working. You can also log out of root for now.

## Acquiring an SSL certificate

Next we'll need to get Certbot installed. Many of you might want to run it in its own user, and to get the latest version via github. I'm going to keep things simple and just directly download a stable version to the ubuntu user's home directory (and it also ensures hopefully the tutorial will still work in the medium-term future).

_**Important note**: If you're following along on a low power instance like the EC2 t2.micro that I'm using, the certbot install my cause errors. You just need to stop the rancher service while you do this segment since it's a bit of a memory hog. I realise that this defeats the purpose of using `webroot` instead of `standalone` but this is intended as a demonstration. On a more powerful machine, or with less demanding services, you won't have this issue. The commands to do this are below:_

{% highlight shell %}
# Look for rancher, the container ID is on the left
# You can use ps -a to see stopped containers
sudo docker ps
sudo docker stop <container id>
# To restart it later:
sudo docker start <container id>
{% endhighlight %}

In the latest version of ubuntu you can actually install certbot via apt-get:

{% highlight shell %}
sudo apt-get install letsencrypt -y
{% endhighlight %}

To make this tutorial a little more universal though, I won't do that. I'm going to get a stable version from [https://certbot.eff.org](https://certbot.eff.org).

{% highlight shell %}
# <Log out of root if necessary>
cd ~
wget https://dl.eff.org/certbot-auto
chmod a+x certbot-auto
# Run certbot once with no arguments
# This will install its dependencies
./certbot-auto
{% endhighlight %}

Now we get to the important part: actually requesting the certificate!

{% highlight shell %}
# Change the webroot and the domain to your own values if needed
./certbot-auto certonly --webroot \
    -w /var/www/rancher-certbot-webroot \
    -d ssltest.busby.ninja
{% endhighlight %}

You'll be prompted for an email, and to accept some terms and conditions. When you've done that, you'll see something like the following:

{% highlight text %}
IMPORTANT NOTES:
 - Congratulations! Your certificate and chain have been saved at
   /etc/letsencrypt/live/ssltest.busby.ninja/fullchain.pem. Your cert
   will expire on 2016-08-24. To obtain a new version of the
   certificate in the future, simply run Certbot again.
 - If you lose your account credentials, you can recover through
   e-mails sent to tom@busby.ninja.
 - Your account credentials have been saved in your Certbot
   configuration directory at /etc/letsencrypt. You should make a
   secure backup of this folder now. This configuration directory will
   also contain certificates and private keys obtained by Certbot so
   making regular backups of this folder is ideal.
 - If you like Certbot, please consider supporting our work by:

   Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
   Donating to EFF:                    https://eff.org/donate-le
{% endhighlight %}

## Configuring nginx to use the SSL certificate

_Users affected by the out-of-memory errors mentioned earlier will want to restart rancher now._

Again, it's time to modify the rancher nginx config:

<p style="margin-bottom: 0; font-weight: bold"><code>/etc/nginx/sites-available/rancher</code></p>
{% highlight nginx %}
upstream rancher {
    # change this if you used a port other than 8080
    server 127.0.0.1:8080 fail_timeout=0;
}

# This block 301 redirects HTTP requests to HTTPS
server {
    listen 80;
    listen [::]:80;

    server_name ssltest.busby.ninja; # replace this with your domain
    return 301 https://$server_name$request_uri;
}

server {
    listen 443 ssl;
    listen [::]:443;

    server_name ssltest.busby.ninja; # replace this with your domain

    root /var/www/rancher-certbot-webroot;

    # The public and private parts of the certificate are linked here
    ssl_certificate /etc/letsencrypt/live/ssltest.busby.ninja/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/ssltest.busby.ninja/privkey.pem;

    location @proxy {
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header X-Forwarded-Port $server_port;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_pass http://rancher;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_read_timeout 900s;
    }

    location / {
        try_files $uri @proxy;
    }
}
{% endhighlight %}

This is quite a basic, naive way of setting up SSL but I'll include instructions for increasing the security with extra configuration options in a future post.

Once you've restarted nginx then you should confirm by navigating to your domain name. You should be redirected to HTTPS and served up rancher with a nice green padlock showing in the address bar.

At this point, you're basically done. You have your reverse proxy set up with a valid SSL cert (at least until the certificate expires) and you can repeat this process to proxy as many other services as you like.

## Automated renewal and revoking certificates

In order to renew your certificates, you simply run the following:

{% highlight shell %}
# You can add --dry-run to test without changes
./certbot-auto renew
{% endhighlight %}

If you have any certificates that are within the renewal window (usually 30 days before expiry, Let's Encrypt certs typically expire after three months) then these will be automatically renewed. Given that we left the web-root infrastructure in place, this can be done without any interruption to the service.

It's a good idea to set up a [cron job](http://www.thegeekstuff.com/2009/06/15-practical-crontab-examples/) to run periodically. The consensus seems to be that you should run renew once or twice a day.

If, at this point, you've followed the tutorial and understood it then you should revoke your certificate before destroying your instance or test environment. This is accomplished with the following command:

{% highlight shell %}
#Change the path to reflect your domain name
./certbot-auto revoke --cert-path \
    /etc/letsencrypt/live/ssltest.busby.ninja/fullchain.pem
{% endhighlight %}

As a final note, I encourage you to have a read of [Let's Encrypt's own documentation](https://letsencrypt.org/getting-started/). There are things I haven't covered (such as acquiring one certificate for multiple domains) and the things they cover, they do so in more detail than me. If, like me, you find ngnix to be a very powerful, flexible piece of software then you can find their documentation [here](http://nginx.org/en/docs/). I haven't scratched the surface of what it's capable of.

Thank you to everyone who read this. (Constructive) feedback and corrections are always welcome.
