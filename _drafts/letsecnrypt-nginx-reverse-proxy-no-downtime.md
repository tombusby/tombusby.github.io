---
title: "Let's Encrypt Auto-Renewal for Nginx Reverse Proxies"
layout: post
tag:
- letsencrypt
- privacy
- nginx
- proxy
- ssl
blog: true
---

##Motivation

_If you are familiar with Nginx as a reverse proxy and have already used Let's Encrypt, skip this section._

####Nginx

A very useful feature of [nginx](https://www.nginx.com) is that you can host multiple services on the same host and the same IP. For example, you could have a [Node.js](https://nodejs.org) application running on `localhost:8080`, a [Jenkins CI](https://jenkins.io/) service running on `localhost:8088` and any other combination of web services written in various languages.

These local ports are not exposed to the pubic internet. Instead, nginx listens on port 80/443 and proxies the relevent service based on what domain name is being requested. For example, a request to `jenkins.example.com` would be proxied to `localhost:8080` in our earlier example.

In order to secure our connection with the client, HTTPS connections can be set up in the nginx configuration. The local services can safely serve plaintext HTTP since all connections are local.

####Let's Encrypt

[Let's Encrypt](https://letsencrypt.org) is a new certificate authority which aims to provide free, automated SSL certificates. All the major browsers have now added Let's Encrypt to their default list of trusted providers. This means that site operators who wanted to serve HTTPS but didn't want to pay for a certificate now can.

This is actually very exciting for ordinary internet users' privacy and security!

####Certbot

Let's Encrypt provide an automated CLI tool to obtain certificates called [Certbot](https://github.com/certbot/certbot). Certbot proves to the Let's Encrypt certificate authority that you own the domain by simply listning on port 80/443 and having the certificate authority make a request.

If you're trying to acquire a certificate for `newdomain.example.com` then Let's Encrypt will just resolve that domain name and make a request. If they receive the response they're expecting then this proves that whoever is running certbot does control that domain.

There are currently three modes you can use to obtain your certificate:

1. `Apache` - Certbot is able to plug-in directly to the [Apache](https://httpd.apache.org/) web server and allow Let's Encrypt to make their request without any down time for what's normally hosted there. This is an ideal solution for Apache users.
2. `Webroot` - If you're not hosting your site with Apache, but have write access to the directory which serves as the web-root for the service, Certbot can drop some files in here and they'll be served up when Let's Encrypt comes knocking just as normal. Again, no interruption in service.
3. `Certonly` - Certbot will directly listen on the required port. This obviously requires some down-time for whatever service is normally listening on that port.

Unfortunately, a lot of the time when we're using nginx as a reverse proxy, it appears that we need to use Certonly. For example, if you're proxying a service that's running in a [Docker](https://www.docker.com/) container, then you don't have access to the webroot.

To make things worse, every time you need to renew the certificate for that one service, you need to stop nginx (severing the link to all other services too) so that Certbot can listen on those ports.

Fortunately, I'm going to show you a way around this. There is an easy way to set up your services behind an nginx reverse proxy and still get the benefits of automated certificate renewal.

## Provisioning a server

For this example I'm starting out by spinning up a fresh [Ubuntu](http://www.ubuntu.com/) instance on [Amazon EC2](https://aws.amazon.com) (t2.micro). To begin with I've allowed all incoming traffic via its security group. I'll be including all installation commands so this tutorial can be followed exactly.

## Setting up our service

For this example I'm going to use a [Rancher](http://rancher.com/) server as my example service. I'm doing this precisely because it runs in the Docker container, so I can't use Certbot's Apache mode, and I don't have access to the service's web root directory. 






###Shell highlight test
{% highlight shell %}
sudo apt-get install docker -y $TEST
{% endhighlight %}

###Nginx highlight test
{% highlight nginx %}
server {
    listen 443;

    ssl on;
    ssl_certificate /etc/ssl/your_domain_name.pem;
    ssl_certificate_key /etc/ssl/your_domain_name.key;

    server_name your.domain.com;
    access_log /var/log/nginx/nginx.vhost.access.log;
    error_log /var/log/nginx/nginx.vhost.error.log;

    location / {
        root /home/www/public_html/your.domain.com/public/;
        index index.html;
    }
}
{% endhighlight %}
