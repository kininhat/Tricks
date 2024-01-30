# nginx tricks

## 1. Tricks cơ bản

### 1.1 badmethod

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

### 1.2 block useragent

```bash
server {
....
#block useragent
if ($http_user_agent ~* (user_agent1|agent2)) { return 444; }
```

### 1.3 block client use proxy

```bash
if ( $http_x_proxy_id ) { return 444; }
if ( $http_x_forwarded_for ) { return 444; }
if ( $http_via ) { return 444; }
```

### 1.3 block referrer

```bash
if ($http_referer ~* ()) { return 444; }
```

### 1.4 block random uri

#### 1.4.1 Dạng cơ bản đánh random vào trang index

```bash
#Random uri: 
#GET //?120688150947115545DHP943642148411488171z 
#GET /?120688150947115545DHP943642148411488171z 
   if ($request  ~* (/\?) ) { return 444;  }
```

#### 1.4.2 Dạng cơ bản đánh random vào trang nhất định

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
