# Cpanel

I. [htaccess](#htaccess)

1. [Chặn useragent có random link ddos và chỉ  allow curl, wget, cron](#htaccess-drop-ua)

2. [Dùng geoip để chặn quốc tế](#htaccess-drop-qt)

    2.1. [Chỉ cho phép useragent và quốc gia nhất định truy cập](#htaccess-drop-qt-ua)

II. [Cpanel cmd](#cpanel-cmd)

1. [rebuild webserver](#rebuild-ws)

2. [restart litespeed/openlitespeed](#restart-ws)

III. [Cloudlinux cmd](#cloudlinux-cmd)

1. [restart cloudlinux](#restart-cl)
2. [Map từ cloudlinux user id sang username](#map-cl-id-to-user)
3. [Tìm user load cao trong cloudlinux](#cl-user-load)

## I. htaccess <a name="htaccess"></a>

### 1. Chặn useragent có random link ddos và chỉ  allow curl, wget, cron <a name="htaccess-drop-ua"></a>

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

### 2. Dùng geoip để chặn quốc tế  <a name="htaccess-drop-qt"></a>

#### 2.1. Chỉ cho phép useragent và quốc gia nhất định truy cập <a name="htaccess-drop-qt-ua"></a>

```bash
RewriteEngine On
RewriteCond %{HTTP_USER_AGENT} !^cron-job\.org [NC]
RewriteCond %{HTTP_USER_AGENT} !^console\.cron-job\.org [NC]
RewriteRule ^ - [E=ALLOW_USERAGENT:1]

RewriteCond %{ENV:ALLOW_USERAGENT} !^1$
RewriteCond %{ENV:GEOIP_COUNTRY_CODE} !^(US|VN)$
RewriteRule ^ - [F,L]
```

## II. Cpanel cmd <a name="cpanel-cmd"></a>

### 1. rebuild webserver <a name="rebuild-ws"></a>

```bash
/scripts/rebuildhttpdconf
/scripts/restartsrv_httpd
```

### 2. restart litespeed/openlitespeed <a name="restart-ws"></a>

```bash
systemctl restart lsws
/usr/local/lsws/bin/lswsctrl restart
```

## II.Cloudlinux cmd <a name="cloudlinux-cmd"></a>

### 1. restart cloudlinux  <a name="restart-cl"></a>

```bash
service lvestats restart
```

### 2. Map từ cloudlinux user id sang username <a name="map-cl-id-to-user"></a>

```bash
id -nu cloudlinux_user_id
```

### 3. Tìm user load cao trong cloudlinux <a name="cl-user-load"></a>

* Sort theo % cpu usage

```bash
lvetop -t -s cpu -l
```

* 1 số option khác

* -p to print per-process/per-thread statistics
* -n to print LVE ID istead of username
* -o to use formatted output (fmt=id,ep,pid,tid,cpu,mem,io)
* -d to show dynamic cpu usage instead of total cpu usage
* -c to calculate average cpu usage for <time> seconds (used with -d)
* -r to run under realtime priority for more accuracy (needs privileges)
* -s to sort LVEs in output (cpu, process, thread, mem, io)
* -t to run in the top-mode
* -w to show pids instead of command names
* -h to print this brief help message
* -l to print cpu usage in old format
* -m to print cpu usage in mhz
