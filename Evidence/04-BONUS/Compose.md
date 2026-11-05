[← README로 돌아가기](../../README.md) | [주요 개념](Main.md)

# Docker Compose 실습 기록

Ubuntu VM의 `~/Codyssey/Projects/E1-1/compose`에서 진행함. 기존 `e1-web:1.1` 이미지를 사용함. SSH 키 설정 실습은 제외함.

명령과 출력은 기존 레포 및 실습 로그를 보존함. 일부 긴 줄의 공백과 프롬프트 표기는 원문에 따른 것임.

## 1. Docker Compose 기초

Compose 버전과 사용할 이미지·기존 컨테이너를 확인한 뒤, 단일 `web` 서비스를 실행함. `config`로 해석된 설정을 확인하고, `ps`에서 `127.0.0.1:8082->80/tcp`와 실행 상태를 확인함.

```bash
:~$ docker compose version
Docker Compose version 2.40.3+ds1-0ubuntu1~24.04.1

:~$ sudo docker images e1-web
[sudo] password for student:
        i Info →   U  In Use
IMAGE        ID             DISK USAGE   CONTENT SIZE   EXTRA
e1-web:1.0   be48e83b5367       93.2MB         26.3MB    U
e1-web:1.1   9466840bb879       93.2MB         26.3MB    U

:~$ sudo docker ps --format 'table {{.Names}}\t{{.Image}}\t{{.Ports}}'
NAMES         IMAGE        PORTS
e1-web-8081   e1-web:1.1   127.0.0.1:8081->80/tcp
e1-web-8080   e1-web:1.1   127.0.0.1:8080->80/tcp
e1-web        e1-web:1.0   80/tcp

:~$ sudo docker images e1-web
        i Info →   U  In Use
IMAGE        ID             DISK USAGE   CONTENT SIZE   EXTRA
e1-web:1.0   be48e83b5367       93.2MB         26.3MB    U
e1-web:1.1   9466840bb879       93.2MB         26.3MB    U

:~$ mkdir -p Codyssey/Projects/E1-1/compose
:~$ cd Codyssey/Projects/E1-1/compose/

:~/Codyssey/Projects/E1-1/compose$ nano docker-compose.yml 
:~/Codyssey/Projects/E1-1/compose$ sudo docker compose -p e1-bonus config
name: e1-bonus
services:
  web:
    image: e1-web:1.1
    networks:
      default: null
    ports:
      - mode: ingress
        host_ip: 127.0.0.1
        target: 80
        published: "8082"
        protocol: tcp
networks:
  default:
    name: e1-bonus_default

:~/Codyssey/Projects/E1-1/compose$ sudo docker compose -p e1-bonus up -d
[+] Running 2/2
 ✔ Network e1-bonus_default  Created               0.1s
 ✔ Container e1-bonus-web-1  Started               0.2s

:~/Codyssey/Projects/E1-1/compose$ sudo docker compose -p e1-bonus ps
NAME             IMAGE        COMMAND                  SERVICE   CREATED          STATUS         PORTS
e1-bonus-web-1   e1-web:1.1   "/docker-entrypoint.…"   web       10 seconds ago   Up 9 seconds   127.0.0.1:8082->80/tcp

:~/Codyssey/Projects/E1-1/compose$ sudo docker compose -p e1-bonus logs --tail=30 web
```
```log
web-1  | /docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
web-1  | /docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
web-1  | /docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
web-1  | 10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
web-1  | 10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
web-1  | /docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
web-1  | /docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
web-1  | /docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
web-1  | /docker-entrypoint.sh: Configuration complete; ready for start up
web-1  | 2026/11/05 04:45:18 [notice] 1#1: using the "epoll" event method
web-1  | 2026/11/05 04:45:18 [notice] 1#1: nginx/1.31.8
web-1  | 2026/11/05 04:45:18 [notice] 1#1: built by gcc 15.2.0 (Alpine 15.2.0)
web-1  | 2026/11/05 04:45:18 [notice] 1#1: OS: Linux 6.8.0-134-generic
web-1  | 2026/11/05 04:45:18 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1024:524288
web-1  | 2026/11/05 04:45:18 [notice] 1#1: start worker processes
web-1  | 2026/11/05 04:45:18 [notice] 1#1: start worker process 30
web-1  | 2026/11/05 04:45:18 [notice] 1#1: start worker process 31
web-1  | 172.18.0.1 - - [05/Nov/2026:04:45:33 +0000] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/156.0.0.0 Safari/537.36" "-"
web-1  | 2026/11/05 04:45:33 [error] 30#30: *1 open() "/usr/share/nginx/html/.well-known/appspecific/com.chrome.devtools.json" failed (2: No such file or directory), client: 172.18.0.1, server: localhost, request: "GET /.well-known/appspecific/com.chrome.devtools.json HTTP/1.1", host: "127.0.0.1:18082"
web-1  | 172.18.0.1 - - [05/Nov/2026:04:45:33 +0000] "GET /.well-known/appspecific/com.chrome.devtools.json HTTP/1.1" 404 555 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/156.0.0.0 Safari/537.36" "-"
web-1  | 172.18.0.1 - - [05/Nov/2026:04:45:33 +0000] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/156.0.0.0 Safari/537.36" "-"
web-1  | 2026/11/05 04:45:33 [error] 30#30: *1 open() "/usr/share/nginx/html/.well-known/appspecific/com.chrome.devtools.json" failed (2: No such file or directory), client: 172.18.0.1, server: localhost, request: "GET /.well-known/appspecific/com.chrome.devtools.json HTTP/1.1", host: "127.0.0.1:18082"
web-1  | 172.18.0.1 - - [05/Nov/2026:04:45:33 +0000] "GET /.well-known/appspecific/com.chrome.devtools.json HTTP/1.1" 404 555 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/156.0.0.0 Safari/537.36" "-"
```

### 실행 결과

- 프로젝트 `e1-bonus`의 네트워크와 `web` 컨테이너가 생성됨.
- 실행 옵션을 YAML 파일로 관리하여 같은 구성을 반복 적용할 수 있음.

## 2. Docker Compose 멀티 컨테이너

`client` 서비스를 추가하고 두 서비스를 함께 실행함. `client`에서 서비스 이름 `web`과 컨테이너 내부 포트 `80`으로 HTTP 요청을 보냄.

```bash
:~/Codyssey/Projects/E1-1/compose$ nano docker-compose.yml
:~/Codyssey/Projects/E1-1/compose$ cat docker-compose.yml
services:
  web:
    image: e1-web:1.1
    ports:
      - "127.0.0.1:8082:80"

  client:
    image: alpine:3.20
    command: ["sleep", "infinity"]

:~/Codyssey/Projects/E1-1/compose$ sudo docker compose -p e1-bonus config --quiet
:~/Codyssey/Projects/E1-1/compose$ sudo docker compose -p e1-bonus up -d
[+] Running 4/4
 ✔ client Pulled                                         4.0s
   ✔ 3f26bc2dec0b Pull complete                          1.3s
   ✔ cee42a41056b Download complete                      0.0s
   ✔ 8b093306b817 Download complete                      0.0s
[+] Running 2/2
 ✔ Container e1-bonus-web-1     Running                  0.0s
 ✔ Container e1-bonus-client-1  Started                  0.6s

:~/Codyssey/Projects/E1-1/compose$ sudo docker compose -p e1-bonus ps
NAME                IMAGE         COMMAND                  SERVICE   CREATED          STATUS          PORTS
e1-bonus-client-1   alpine:3.20   "sleep infinity"         client    22 seconds ago   Up 21 seconds
e1-bonus-web-1      e1-web:1.1    "/docker-entrypoint.…"   web       3 hours ago      Up 3 hours      127.0.0.1:8082->80/tcp

:~/Codyssey/Projects/E1-1/compose$ sudo docker compose -p e1-bonus exec client wget -qO- http://web:80/
```
```html
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>Codyssey E1-1 Mission Page</title>
</head>
<body>
  <h1>안녕하세요.</h1>
  <h2>Dockerfile로 만든 이미지를 통해 제공하는 페이지입니다!</h2>
</body>
</html>
:~/Codyssey/Projects/E1-1/compose$
```

### 실행 결과

- `web`과 `client`가 모두 실행 상태로 확인됨.
- `wget -qO- http://web:80/` 요청으로 HTML을 수신함.
- `client`의 `localhost`가 아닌 `web`으로 다른 서비스에 접근함.
- 첫 요청은 대문자 `O` 대신 숫자 `0`을 입력하여 실패함. 옵션 수정 후 성공함. `-O -`는 응답을 표준 출력으로 표시하는 옵션임.

## 3. Docker Compose 운영

접속 로그 확인 후 프로젝트를 종료·정리하고 남은 컨테이너를 확인함.

```bash
:~/Codyssey/Projects/E1-1/compose$ sudo docker compose -p e1-bonus logs --tail=20 web
```
```log
web-1  | 10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
web-1  | /docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
web-1  | /docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
web-1  | /docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
web-1  | /docker-entrypoint.sh: Configuration complete; ready for start up
web-1  | 2026/11/05 04:45:18 [notice] 1#1: using the "epoll" event method
web-1  | 2026/11/05 04:45:18 [notice] 1#1: nginx/1.31.8
web-1  | 2026/11/05 04:45:18 [notice] 1#1: built by gcc 15.2.0 (Alpine 15.2.0)
web-1  | 2026/11/05 04:45:18 [notice] 1#1: OS: Linux 6.8.0-134-generic
web-1  | 2026/11/05 04:45:18 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1024:524288
web-1  | 2026/11/05 04:45:18 [notice] 1#1: start worker processes
web-1  | 2026/11/05 04:45:18 [notice] 1#1: start worker process 30
web-1  | 2026/11/05 04:45:18 [notice] 1#1: start worker process 31
web-1  | 172.18.0.1 - - [05/Nov/2026:04:45:33 +0000] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/156.0.0.0 Safari/537.36" "-"
web-1  | 2026/11/05 04:45:33 [error] 30#30: *1 open() "/usr/share/nginx/html/.well-known/appspecific/com.chrome.devtools.json" failed (2: No such file or directory), client: 172.18.0.1, server: localhost, request: "GET /.well-known/appspecific/com.chrome.devtools.json HTTP/1.1", host: "127.0.0.1:18082"
web-1  | 172.18.0.1 - - [05/Nov/2026:04:45:33 +0000] "GET /.well-known/appspecific/com.chrome.devtools.json HTTP/1.1" 404 555 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/156.0.0.0 Safari/537.36" "-"
web-1  | 172.18.0.1 - - [05/Nov/2026:04:45:33 +0000] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/156.0.0.0 Safari/537.36" "-"
web-1  | 2026/11/05 04:45:33 [error] 30#30: *1 open() "/usr/share/nginx/html/.well-known/appspecific/com.chrome.devtools.json" failed (2: No such file or directory), client: 172.18.0.1, server: localhost, request: "GET /.well-known/appspecific/com.chrome.devtools.json HTTP/1.1", host: "127.0.0.1:18082"
web-1  | 172.18.0.1 - - [05/Nov/2026:04:45:33 +0000] "GET /.well-known/appspecific/com.chrome.devtools.json HTTP/1.1" 404 555 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/156.0.0.0 Safari/537.36" "-"
web-1  | 172.18.0.3 - - [05/Nov/2026:07:53:42 +0000] "GET / HTTP/1.1" 200 253 "-" "Wget" "-"
```
```bash
:~/Codyssey/Projects/E1-1/compose$ sudo docker compose -p e1-bonus down
[+] Running 3/3
 ✔ Container e1-bonus-client-1  Removed                 10.2s
 ✔ Container e1-bonus-web-1     Removed                  0.3s
 ✔ Network e1-bonus_default     Removed                  0.1s

:~/Codyssey/Projects/E1-1/compose$ sudo docker compose -p e1-bonus ps -a
NAME      IMAGE     COMMAND   SERVICE   CREATED   STATUS    PORTS
```

### 실행 결과

- `Wget`의 `GET /` 요청에 HTTP `200`이 기록됨.
- Chrome DevTools 관련 경로의 `404`는 해당 파일이 없다는 기록이며, 위 HTML 요청의 성공과 구분함.
- `down`으로 두 컨테이너와 `e1-bonus_default` 네트워크가 삭제됨.
- `ps -a`는 헤더만 출력되어 해당 프로젝트에 남은 컨테이너가 없음을 확인함.
- 다음 환경변수 실습에서 `up -d`로 다시 실행함.

## 4. 환경변수 활용

Compose의 `${WEB_PORT:-8082}`를 사용하여 호스트 접속 포트를 변경함. 값이 없거나 비어 있으면 기본값 `8082`를 사용함. 이번 실습은 Compose 설정 치환이며, 컨테이너 내부 환경변수 주입이나 Nginx의 내부 리슨 포트 변경은 아님.

### 4-1. 변경 전 — WEB_PORT=8082

파일 수정 후 `WEB_PORT=8082`를 명시하여 실행하고 접속함. 이는 변수를 생략한 기본값 동작 시험과는 구분함.

```bash
:~/Codyssey/Projects/E1-1/compose$ nano docker-compose.yml
:~/Codyssey/Projects/E1-1/compose$ sudo env WEB_PORT=8082 docker compose -p e1-bonus up -d
[+] Running 3/3
 ✔ Network e1-bonus_default     Created                  0.1s
 ✔ Container e1-bonus-web-1     Started                  0.4s
 ✔ Container e1-bonus-client-1  Started                  0.4s

:~/Codyssey/Projects/E1-1/compose$ sudo docker compose -p e1-bonus ps
NAME                IMAGE         COMMAND                  SERVICE   CREATED          STATUS          PORTS
e1-bonus-client-1   alpine:3.20   "sleep infinity"         client    13 seconds ago   Up 12 seconds
e1-bonus-web-1      e1-web:1.1    "/docker-entrypoint.…"   web       13 seconds ago   Up 12 seconds   127.0.0.1:8082->80/tcp

:~/Codyssey/Projects/E1-1/compose$ curl http://127.0.0.1:8082
```
```html
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>Codyssey E1-1 Mission Page</title>
</head>
<body>
  <h1>안녕하세요.</h1>
  <h2>Dockerfile로 만든 이미지를 통해 제공하는 페이지입니다!</h2>
</body>
</html>
```
```bash
:~/Codyssey/Projects/E1-1/compose$ cat docker-compose.yml
services:
  web:
    image: e1-web:1.1
    ports:
      - "127.0.0.1:${WEB_PORT:-8082}:80"

  client:
    image: alpine:3.20
    command: ["sleep", "infinity"]
```

### 4-2. 변경 후 — WEB_PORT=8083

같은 설정 파일에서 변수 값만 `8083`으로 변경하여 적용하고 접속함.

```bash
:~/Codyssey/Projects/E1-1/compose$ sudo env WEB_PORT=8083 docker compose -p e1-bonus up -d
[+] Running 2/2
 ✔ Container e1-bonus-client-1  Running                  0.0s
 ✔ Container e1-bonus-web-1     Started                  0.4s

:~/Codyssey/Projects/E1-1/compose$ sudo docker compose -p e1-bonus ps
NAME                IMAGE         COMMAND                  SERVICE   CREATED         STATUS         PORTS
e1-bonus-client-1   alpine:3.20   "sleep infinity"         client    2 minutes ago   Up 2 minutes
e1-bonus-web-1      e1-web:1.1    "/docker-entrypoint.…"   web       8 seconds ago   Up 7 seconds   127.0.0.1:8083->80/tcp

:~/Codyssey/Projects/E1-1/compose$ curl http://127.0.0.1:8083
```
```html
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>Codyssey E1-1 Mission Page</title>
</head>
<body>
  <h1>안녕하세요.</h1>
  <h2>Dockerfile로 만든 이미지를 통해 제공하는 페이지입니다!</h2>
</body>
</html>
```

### 변경 전후 비교

| 항목 | 변경 전 | 변경 후 |
| --- | --- | --- |
| 변수 값 | `WEB_PORT=8082` | `WEB_PORT=8083` |
| 호스트 게시 주소 | `127.0.0.1:8082` | `127.0.0.1:8083` |
| 컨테이너 내부 포트 | `80` | `80` |
| 접속 결과 | HTML 수신 | 동일한 HTML 수신 |

웹페이지 코드나 이미지를 수정하지 않고 접속 설정을 변경함. `WEB_PORT`는 해당 명령의 Compose 프로세스에 전달되며 영구 저장되지 않음. 이후 `up` 실행 시 같은 값을 지정해야 8083 설정을 유지할 수 있음. `ps`는 현재 상태 조회이므로 변수 없이 실행해도 현재 게시된 포트를 확인할 수 있음.
