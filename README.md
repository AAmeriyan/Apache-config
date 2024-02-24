# Apache Virtual Host Configuration Readme

## Introduction

This readme provides a quick guide on configuring Virtual Hosts in the Apache HTTP Server, tailored for a GitHub README.

## Basic Virtual Host

A basic Virtual Host configuration looks like this:

```apache
<VirtualHost sub.domain.com:80>
     ServerAdmin info@mail.com
    ServerName sub.domain.com
    Redirect permanent / https://sub.domain.com/
</VirtualHost>
```
```apache
<VirtualHost sub.domain.com:443>
    ServerAdmin info@mail.com
    ServerName sub.domain.com
    #SSL configuration
    SSLEngine On
    SSLCertificateFile /path/to/ssl/certificate.crt
    SSLCertificateKeyFile /path/to/ssl/key.key
    SSLCertificateChainFile  /path/to/ssl/bundle.crt
    #Load Balancing for several localhost Node.js server  
    <Proxy "balancer://mycluster">
        BalancerMember http://localhost:4000/
        BalancerMember http://localhost:4001/
        BalancerMember http://localhost:4002/
        BalancerMember http://localhost:4003/
        BalancerMember http://localhost:4004/
        BalancerMember http://localhost:4005/
        BalancerMember http://localhost:4006/
        BalancerMember http://localhost:4007/
        BalancerMember http://localhost:4008/
        BalancerMember http://localhost:4009/
        BalancerMember http://localhost:4010/
        ProxySet lbmethod=byrequests
    </Proxy>
    # ignore some routes to revsere proxy especialy when using wordpress beside node.js
    ProxyPreserveHost On
    <Location "/blog/*">
        ProxyPass "!"
    </Location>
    <Proxy *>
        Order allow,deny
        Allow from all
    </Proxy>

      #reserve proxy
    ProxyPass / balancer://mycluster/
    ProxyPassReverse / balancer://mycluster/
</VirtualHost>
#another sub.domain reverse proxy but simple not with cluster load balancer
<VirtualHost sub2.domain.com:443>
    ServerAdmin info@mail.com
    ServerName sub2.domain.com

    SSLEngine On
    SSLCertificateFile /path/to/ssl/certificate.crt
    SSLCertificateKeyFile /path/to/ssl/key.key
    SSLCertificateChainFile  /path/to/ssl/bundle.crt


    ProxyPass / http://xxx.xxx.xxx.xxx:9001/
    ProxyPassReverse / http://xxx.xxx.xxx.xxx:9001/
    ProxyTimeout 30
</VirtualHost>
```
