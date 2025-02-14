---
title: "Squid Proxy Basic Auth Configuration"
date: 2025-03-10T10:00:00+01:00
description: "How to configure Squid proxy with basic authentication"
tags: ["howto", "squid", "proxy", "auth"]
categories: ["System Administration"]
draft: false
---

This quick and straightforward how-to is the one I would have loved to have found while I was setting up [Squid proxy](https://www.squid-cache.org/) for a production application.

Squid is a caching proxy for the Web supporting HTTP, HTTPS, FTP, and more. It reduces bandwidth and improves response times by caching and reusing frequently-requested web pages.

I wanted to restrict access to the proxy to only authenticated users.
Here's how I did it.

## Create the htpasswd file
The first step is to create the htpasswd file.
```
htpasswd -c /etc/squid/passwd username
```
This will prompt for a password.

## Make sure Squid can read the htpasswd file
Without this, Squid will not be able to read the file and fail silently when trying to authenticate.
```
chmod 644 /etc/squid/passwd
```

## Verify the authentication
Run the following command to verify the authentication against squid's basic_ncsa_auth tool:
```
echo "username password" | /usr/lib/squid/basic_ncsa_auth /etc/squid/passwd
```

## Squid configuration
Add the following to your Squid configuration file:
```
# /etc/squid/squid.conf
...
# Authentication settings
auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/passwd
auth_param basic realm proxy
auth_param basic credentialsttl 2 hours
auth_param basic casesensitive off

# Define ACL for authenticated users
acl authenticated_users proxy_auth REQUIRED

# Allow authenticated users to access the web
http_access allow authenticated_users

# And finally deny all other access to this proxy
http_access deny all
...
```
Ensure that `http_access allow authenticated_users` is the last allow rule in the file.

Verify the squid configuration is correct with:
```
squid -k parse
```

And restart Squid:
```
systemctl restart squid
```

## Check if it actually works
From your local machine (meaning not the machine that is running the Squid proxy), try to access a website through the proxy with authentication:
```
curl -x http://username:password@squid-proxy-host:3128 http://example.com
```

Make sure you try to use the proxy without authentication as well, to make sure we get an access denied error.
```
curl -x http://squid-proxy-host:3128 http://example.com
```

## Check the logs
Use the following command to check the logs:
```
tail -f /var/log/squid/access.log
```
You should see logs like this: 
```
1741610861.934  25047 10.135.50.227 TCP_TUNNEL/200 21623 CONNECT example.com:443 username HIER_DIRECT/5.226.179.10 -
1741610861.937  22061 10.135.50.227 TCP_TUNNEL/200 21530 CONNECT example.com:443 username HIER_DIRECT/5.226.179.10 -
1741610861.939   6654 10.135.50.227 TCP_TUNNEL/200 22589 CONNECT example.com:443 username HIER_DIRECT/5.226.179.10 -
1741610861.940  60714 10.135.50.224 TCP_TUNNEL/200 21579 CONNECT example.com:443 username HIER_DIRECT/5.226.179.10 -
```

`TCP_TUNNEL/200` indicates the requests are redirected correctly.

## Troubleshooting

If you get `TCP_DENIED/40*` in the logs, it means something is wrong with the authentication mechanism, for example:
```
1741608951.396      3 10.135.50.230 TCP_DENIED/407 4016 CONNECT example.com:443 username HIER_NONE/- text/html
```
In this case, the request was denied because the user was not authenticated to the proxy.
