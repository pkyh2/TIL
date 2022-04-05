# NginX

## Nginx 기본 설정

### 4가지 영역으로 구성

1. Core 모듈
   코어 모듈은 대부분 환경 설정 파일의 최상단에 위치하며 한번만 사용할 수 있다. nginx의 기본적인 동작방식을 정의
2. http 블록
   웹 서버에 대한 동작을 설정하는 영역으로, server 블록과 location 블록의 root 블록이 있다.
3. server 블록
   하나의 웹사이트를 선언하는데 사용된다. 가상 호스팅의 개념
4. location 블록
   server 블록 내에서 특정 URL을 처리하는 방법을 정의
5. events 주로 네트워크 동작에 관련된 설정하는 영역으로, 이벤트 모듈을 사용한다.

#### NginX 설정 예제

```nginx
user nginx; ## NGINX 프로세스가 실행되는 권한, root 권한은 보안상 위험함

worker_processes 2; ## Default: 1, CPU 코어 하나에 최소한 한 개의 프로세스가 배정되도록 변경 권장

worker_priority 0; ## 값이 작을 수록 높은 우선순위를 갖는다. 커널 프로세스의 기본 우선순위인 -5 이하로는 설정하지 않도록 한다.

# 로그레벨 [ debug | info | notice | warn | error | crit ]
error_log /var/log/nginx/error.log error; ## 로그레벨을 warn -> error로 변경함
pid /var/run/nginx.pid;

events {
worker_connections 1024; ## Default: 1024, 현 서버는 RAM 8GB라 상향조정
multi_accept off; ## Default: off
}

http {
include /etc/nginx/mime.types;
default_type application/octet-stream;
    
log_format main '$remote_addr - $remote_user [$time_local] "$request" '
'$status $body_bytes_sent "$http_referer" '
'"$http_user_agent" "$http_x_forwarded_for"';
    
access_log /var/log/nginx/access.log main;
    
sendfile on;
#tcp_nopush on;
    
server_tokens off; ## 헤더에 NGINX 버전을 숨김 (보안상 설정 권장)
keepalive_timeout 65; ## 접속 시 커넥션 유지 시간
    
#gzip on;
    
include /etc/nginx/conf.d/*.conf;
}

출처: https://prohannah.tistory.com/136 [Hello, Hannah!]
```

