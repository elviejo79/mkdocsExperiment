# Apache Web Server

I use apache for a long term as webserver, load balancer and more...
This page should be a maintenance page for me with some helpful configuration snippets, which I often use.

## Virtual Host

    <VirtualHost *:80>
        ServerName website.de
        ServerAdmin webmaster@website.de
        ....
        DocumentRoot /var/www
    </VirtualHost>

## Logging

    LogLevel warn
    CustomLog /var/log/apache2/my_service_access.log combined
    ErrorLog /var/log/apache2/my_service_error.log

## Reverse Proxy

The apache module `mod_proxy_http` has to be installed and enabled or some other `mod_proxy_*` modules if other protocols should be used.

    ProxyRequests Off   # only needed for apache 2.2 (default since apache 2.4)
    ProxyPreserveHost On
    ProxyPass "/"   "http://internal-service:8080/"
    ProxyReversePass "/"   "http://internal-service:8080/"

## SSL

    SSLEngine on
    SSLCertificateFile /etc/apache2/ssl/website.de.crt
    SSLCertificateKeyFile /etc/apache2/ssl/website.de.key
    SSLCACertificateFile /etc/apache2/ssl/website.de.ca-bundle

### Forwarding HTTP

The apache module `mod_rewrite` has to be installed and enabled.

    RewriteEngine On
    RewriteRule (.*) https://%{HTTP_HOST}%{REQUEST_URI}

{!docs/abbreviations.txt!}
