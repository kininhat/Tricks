# nginx tricks

1. [Tricks cơ bản](#nbtricks)

    1.1. [block badmethod](#block-badmethod)

    1.2. [block useragent](#block-useragent)

    1.3. [block client use proxy](#block-proxy)

    1.4. [block referrer](#block-referrer)

    1.5. [block random uri](#block-random-uri)

    1.5.1. [Dạng cơ bản đánh random vào trang index](#r-index)

    1.5.2. [Random vào link nhất định](#r-link-define)

## 1. Tricks cơ bản <a name="nbtricks"></a>

### 1.1 block badmethod <a name="block-badmethod"></a>

``` bassh
map $request_method $badmethod {
        default 1;
        ~(?i)(GET|HEAD|POST|PUT) 0;
}

server {
    ...
    if ($badmethod = 1) { return 444; }
    ...
}
```

### 1.2 block useragent <a name="block-useragent"></a>

```bash
server {
....
#block useragent
if ($http_user_agent ~* (user_agent1|agent2)) { return 444; }
```

### 1.3 block client use proxy <a name="block-proxy"></a>

```bash
if ( $http_x_proxy_id ) { return 444; }
if ( $http_x_forwarded_for ) { return 444; }
if ( $http_via ) { return 444; }
```

### 1.4 block referrer <a name="block-referrer"></a>

```bash
if ($http_referer ~* ()) { return 444; }
```

### 1.5 block random uri <a name="block-random-uri"></a>

#### 1.5.1 Dạng cơ bản đánh random vào trang index <a name="r-index"></a>

```bash
#Random uri: 
#GET //?120688150947115545DHP943642148411488171z 
#GET /?120688150947115545DHP943642148411488171z 
   if ($request  ~* (/\?) ) { return 444;  }
```

#### 1.5.2 Random vào link nhất định <a name="r-link-define"></a>

* Ví dụ, chỉ cho phép những kết nối dạng này truy cập:

* <https://abcd.com/doithecao>
* <https://abcd.com/doithecao?searchcode=09324835>
* <https://abcd.com/doithecao?page=2>

* Còn lại chặn không cho phép tới <https://abcd.com/doithecao>

* Thực hiện

```bash
location ^~ /doithecao {
        if ($request_uri = "/doithecao") {
        #       access_log /var/log/nginx/test.log test;
                proxy_pass  https://backend;
        }
        if ($request  ~* (\?(page|searchcode)=[0-9a-z-])) {
        #        access_log /var/log/nginx/test2.log; 
                proxy_pass  https://backend;

        }
        return 444;
}
```
