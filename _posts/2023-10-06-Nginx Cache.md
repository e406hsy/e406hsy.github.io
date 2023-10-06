---
layout: post
title:  "Nginx - 캐시 사용하기"
createdDate:   2023-10-06T18:06:00+09:00
date:   2023-10-06T18:06:00+09:00
pagination: enabled
author: SoonYong Hong
categories: Nginx
tags: Nginx Cache
---

# Nginx - 캐시 사용하기

캐싱된 페이지 노출하기 위한 nginx 캐싱 적용 방법

# 상세

nginx의 [http proxy 모듈](https://nginx.org/en/docs/http/ngx_http_proxy_module.html)에는 프록시된 서버의 캐시를 저장하여 제공하는 기능이 존재한다. 주요 directive의 사용방법 공유

## proxy\_cache\_path

### 캐시 기본 설정

```
Syntax:	proxy_cache_path path [levels=levels] [use_temp_path=on|off] keys_zone=name:size [inactive=time] [max_size=size] [min_free=size] [manager_files=number] [manager_sleep=time] [manager_threshold=time] [loader_files=number] [loader_sleep=time] [loader_threshold=time] [purger=on|off] [purger_files=number] [purger_sleep=time] [purger_threshold=time];
Default:	—
Context:	http
```

프록시 캐시를 저장할 directory path와 해당 path에 저장되는 캐시에 관련된 설정

#### 주요 값 설명

| key | 설명 | 참고 | 예시 |
| --- | --- | --- | --- |
| path | 캐시가 저장될 directory path | directory가 없다면 생성한다. <br> 해당 directory의 소유자가 아니면 chown을 실행하기 때문에 권한이 있어야 한다. | /etc/nginx/cache |
| levels | 캐시 directory depth (1 \~ 3, `:`로 구분), directory이름 길이 (1 \~2) |  | levels=1<br>levels=2<br>levels=1:1<br>levels=1:2<br>levels=2:1<br>levels=2:2<br>levels=1:1:1<br>levels=1:1:2<br>levels=1:2:1<br>levels=1:2:2<br>levels=2:1:1<br>levels=2:1:2<br>levels=2:2:1<br>levels=2:2:2 |
| use\_temp\_path | 임시 캐시 저장 여부 | proxy\_temp\_path directive로 저장위치를 설정할 수 있다. | use\_temp\_path=off |
| key\_zone | 모든 활성화된 키는 메모리 존에 저장된다. 메모리존의 이름과 크기를 설정한다. | 1MB에 8000개의 key 저장가능<br>메모리 존의 이름은 proxy\_cache directive에 사용된다. | key\_zone=name\_you\_want:1m |
| inactive | 캐시 미사용시 삭제 시간 | 기본값 10분 (`10m`) | inactive=5m |
| max\_size | 캐시 최대 크기 | @rows=2:캐시 최대크기를 초과하거나 파일시스템에 빈공간이 부족해지면 LRU 에 따라 캐시를 삭제한다 | max\_size=10m |
| min\_free | 파일시스템에 빈공간 | min\_free=1g |

### ex

proxy\_cache\_path <span style="color:#e11d21;">/etc/nginx/cache</span> levels=<span style="color:#207de5;">2</span> key\_zone=<span style="color:#fbca04;">main</span>:<span style="color:#009800;">1m</span> max\_size=<span style="color:#5319e7;">100m</span>;
<span style="color:#e11d21;">/etc/nginx/cache</span>에 <span style="color:#fbca04;">main</span>이라는 캐시존을 설정한다. 이 캐시존의 크기는 <span style="color:#009800;">1MB</span>이며 <span style="color:#207de5;">캐시 directory의 depth는 1, directory 이름의 길이는 2</span>이다. 캐시 최대 크기는 <span style="color:#5319e7;">100MB</span>이다.

## proxy\_cache

### 캐시 존 설정

```
Syntax:	proxy_cache zone | off;
Default:	
proxy_cache off;
Context:	http, server, location
```

프록시 캐시의 존을 설정한다. 이 존은 proxy\_cache\_path directive에 설정된 이름을 이용한다.

### example

`proxy_cache main;`
main이라는 이름의 캐시존을 이용한다.

## proxy\_cache\_bypass

### 캐시를 이용하지 않는 경우에 대한 설정

```
Syntax:	proxy_cache_bypass string ...;
Default:	—
Context:	http, server, location
```

특정 요청의 경우 캐시를 이용하지 않고 프록시된 서버에 요청하도록 설정할 수 있다.
string 값들중 한개라도 `0`이 아닌 값을 가지면 캐시를 이용하지 않는다.

### example

`prox_cache_bypass $http_cachebypass $cookie_nocache`
http 헤더 cachebypass 혹은 쿠키값 nocache에 0이 아닌 값이 설정되면 캐시를 이용하지 않는다.

## proxy\_no\_cache

### 캐시를 저장하지 않는 경우에 대한 설정

```
Syntax:	proxy_no_cache string ...;
Default:	—
Context:	http, server, location
```

특정 요청의 경우 캐시를 refresh하지 않도록 설정할 수 있다.
string 값들중 한개라도 `0`이 아닌 값을 가지면 캐시를 저장하지 않는다.

### example

`proxy_no_cache $http_cachebypass $cookie_nocache`
http 헤더 cachebypass 혹은 쿠키값 nocache에 0이 아닌 값이 설정되면 캐시를 저장하지 않는다.

## proxy\_cache\_background\_update

### 캐시를 백그라운드에서 업데이트하도록 설정

```
Syntax:	proxy_cache_background_update on | off;
Default:	
proxy_cache_background_update off;
Context:	http, server, location
```

업데이트 중에 만료된 캐시를 사용할수 있도록 설정`proxy_cache_use_stale updating;`이 필요하다.

## proxy\_cache\_key

### 캐시 키 설정

```
Syntax:	proxy_cache_key string;
Default:	
proxy_cache_key $scheme$proxy_host$request_uri;
Context:	http, server, location
```

캐시 키를 설정할 수 있다. 특정값에 따라 응답이 달라지는 경우, 키도 달라지도록 캐시키를 설정해서 각각의 응답을 캐싱하도록 하여야한다.

### example

`proxy_cache_key "$http_x_forwarded_proto$proxy_host$request_uri $cookie_HG_LOGIN";`
`http 헤더 X-Forwarded-Proto`, `요청 호스트`, `요청 path`, `HG_LOGIN 쿠키 값`에 따라 캐싱을 별도로 하도록 설정

## proxy\_cache\_use\_stale

### 만료된 캐시를 사용하도록 하는 설정

```
Syntax:	proxy_cache_use_stale error | timeout | invalid_header | updating | http_500 | http_502 | http_503 | http_504 | http_403 | http_404 | http_429 | off ;
Default:	
proxy_cache_use_stale off;
Context:	http, server, location
```

특정 경우에 만료된 캐시를 사용하도록 설정할 수 있다.

#### 주요 값 설명

| value | 설명 | 참고 |
| ----- | --- | --- |
| error | 서버와 커넥션 생성중, 요청 전달중, 응답헤더를 읽는 중 에러가 발생한 경우 |  |
| timeout | 서버와 커넥션 생성중, 요청 전달중, 응답헤더를 읽는 중 타임아웃이 발생한 경우 |  |
| invalid\_header | 서버가 빈 응답을 반환하거나 잘못된 응답을 반환한 경우 |  |
| updating | 캐시를 업데이트 하는 중 |  |
| http\_xxx | xxx응답 코드를 반환한 경우 | 403 404 429 500 502 503 504 만 지원한다. |

### example

`proxy_cache_use_stale error timeout invalid_header updating http_500 http_502 http_503 http_504 http_403 http_404 http_429;`

## proxy\_cache\_valid

### 응답코드에 따라 캐싱되는 시간을 설정한다.

```
Syntax:	proxy_cache_valid [code ...] time;
Default:	—
Context:	http, server, location
```

응답코드에 따라 캐싱되는 시간을 설정한다.
단 프록시된 서버에서 캐시관련 헤더를 설정하여 응답하면 해당 헤더의 설정을 따른다.
단 proxy\_ignore\_headers 디렉티브를 사용하여 우선순위를 무시할 수 있다.

#### 우선 순위 설명

1. `X-Accel-Expires`응답 헤더에 시간(단위: 초)이 설정되면 해당 헤더를 사용한다.
2. `Expires` 또는 `Cache-Control`응답 헤더 설정을 따른다.
3. `Set-Cookie`응답 헤더가 포함되면 캐싱하지 않는다.
4. `Vary`응답 헤더에 `*`이 설정되면 캐싱하지 않는다.
5. `Vary`응답 헤더에 `*`이 설정되지 않으면 `Vary`요청 헤더를 따른다.
6. proxy\_cache\_valid 설정을 따른다.

### example

```
# proxy_cache_valid 5m;는 proxy_cache_valid 200 301 302 5m;와 같다
proxy_ignore_headers Expires Cache-Control;
proxy_cache_valid 5m;
proxy_cache_valid 404      1m;
proxy_cache_valid 403      1h;
proxy_cache_valid any      1m;
```

## proxy\_cache\_purge

### 캐시를 퍼지하도록 하는 설정

```
Syntax:	proxy_cache_purge string ...;
Default:	—
Context:	http, server, location
```

특정 요청의 경우 캐시를 refresh하도록 설정할 수 있다.
string 값들중 한개라도 `0`이 아닌 값을 가지면 캐시를 저장하지 않는다.

### example

`proxy_cache_purge $http_cachePurge`
http 헤더 cachePurge 에 0이 아닌 값이 설정되면 캐시를 refresh한다.

## 전체 설정 예시

```
http {                                                  
    include mime.types;                                   
    default_type application/octet-stream;      
                     
    proxy_cache_path /etc/nginx/cache levels=1 keys_zone=main:1m max_size=100m;                                        
    proxy_temp_path /etc/nginx/tmp;                                                                 
    server {                              
        listen 80;    
        server_name www.hangame.com;
         
        access_log /var/log/nginx/access.log main;
        error_log /var/log/nginx/error.log debug;

        location = / {
            rewrite .* /index.html last;
        }
        location / {
            proxy_intercept_errors on; 
            proxy_set_header X-Forwarded-For $remote_addr;
            proxy_set_header X-Forwarded-Proto $scheme;
            proxy_set_header X-Forwarded-Host $host;
            proxy_set_header X-Forwarded-Port $server_port;
            proxy_pass http://localhost:8080;
            error_page 405 406 411 497 500 501 502 503 504 505 /errorhandler/nodisplay.html;
            error_page 400 404 /errorhandler/notfound.html;
            error_page 401 403 /errorhandler/noprivilege.html;
        }

        location = /index.html {
            # 캐시 main 적용
            proxy_cache main;
            # from_watchdog 요청 헤더가 적용된 경우 캐시 이용하지 않음
            proxy_cache_bypass $http_from_watchdog;
            # 에러발생시 캐시 반환
            proxy_cache_use_stale error timeout updating http_500 http_502 http_503 http_504 http_403 http_404 http_429;
            # http 200인 경우에 1분간 캐싱
            proxy_cache_valid 200 1m;
            proxy_ignore_headers Expires Cache-Control; 
            # HG_LOGIN 쿠키가 있으면 캐싱하지 않음
            proxy_no_cache $cookie_HG_LOGIN;
            
            proxy_intercept_errors on;            
            proxy_set_header X-Forwarded-For $remote_addr;
            proxy_set_header X-Forwarded-Proto $scheme;
            proxy_set_header X-Forwarded-Host $host;
            proxy_set_header X-Forwarded-Port $server_port;
            proxy_pass http://localhost:8080/index.html;
            error_page 405 406 411 497 500 501 502 503 504 505 /errorhandler/nodisplay.html;
            error_page 400 404 /errorhandler/notfound.html;
            error_page 401 403 /errorhandler/noprivilege.html;
        }

        location /errorhandler {
            root html;
        }
    }
}
```