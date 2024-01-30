# Cpanel

## I. htaccess

### 1. chặn useragent có random link ddos và chỉ  allow curl, wget, cron

```bash
RewriteEngine On

# Allow User-Agents containing 'curl', 'cron', or 'wget'
RewriteCond %{HTTP_USER_AGENT} curl [NC,OR]
RewriteCond %{HTTP_USER_AGENT} cron [NC,OR]
RewriteCond %{HTTP_USER_AGENT} wget [NC]
RewriteRule ^ - [L]

# Block User-Agents that are a single continuous string (no spaces)
RewriteCond %{HTTP_USER_AGENT} ^[^\s]+$ [NC]
RewriteRule ^ - [F,L]

```

### 2. dùng geoip để chặn quốc tế

#### a. Chỉ cho phép useragent và quốc gia nhất định truy cập

```bash
RewriteEngine On
RewriteCond %{HTTP_USER_AGENT} !^cron-job\.org [NC]
RewriteCond %{HTTP_USER_AGENT} !^console\.cron-job\.org [NC]
RewriteRule ^ - [E=ALLOW_USERAGENT:1]

RewriteCond %{ENV:ALLOW_USERAGENT} !^1$
RewriteCond %{ENV:GEOIP_COUNTRY_CODE} !^(US|VN)$
RewriteRule ^ - [F,L]
```

## II. cpanel cmd

### 1. rebuild webserver

```bash
/scripts/rebuildhttpdconf
/scripts/restartsrv_httpd
```
