[← README로 돌아가기](../../README.md) | [주요 개념](Main.md)

# Docker Compose 실습 기록

Ubuntu VM의 `~/Codyssey/Projects/E1-1/compose`에서 진행함. 기존 `e1-web:1.1` 이미지를 사용함. SSH 실습은 [별도 문서](GitHub.md)에 기록함.

기존 실행 기록을 발췌하고 명령 바로 위에 설명 주석을 추가함. 출력은 보존하며, .env 방식의 실행 기록은 기존 명령별 env 전달 기록과 구분함.

## 1. Docker Compose 기초

Compose 버전과 사용할 이미지·기존 컨테이너를 확인한 뒤, 단일 `web` 서비스를 실행함. `config`로 해석된 설정을 확인하고, `ps`에서 `127.0.0.1:8082->80/tcp`와 실행 상태를 확인함.

```bash
# Docker Compose 버전 확인
:~$ docker compose version
Docker Compose version 2.40.3+ds1-0ubuntu1~24.04.1

# 사용할 로컬 이미지 확인
:~$ sudo docker images e1-web
[sudo] password for student:
        i Info →   U  In Use
IMAGE        ID             DISK USAGE   CONTENT SIZE   EXTRA
e1-web:1.0   be48e83b5367       93.2MB         26.3MB    U
e1-web:1.1   9466840bb879       93.2MB         26.3MB    U

# 기존 컨테이너 이름·이미지·포트 확인
:~$ sudo docker ps --format 'table {{.Names}}\t{{.Image}}\t{{.Ports}}'
NAMES         IMAGE        PORTS
e1-web-8081   e1-web:1.1   127.0.0.1:8081->80/tcp
e1-web-8080   e1-web:1.1   127.0.0.1:8080->80/tcp
e1-web        e1-web:1.0   80/tcp

# 사용할 로컬 이미지 확인
:~$ sudo docker images e1-web
        i Info →   U  In Use
IMAGE        ID             DISK USAGE   CONTENT SIZE   EXTRA
e1-web:1.0   be48e83b5367       93.2MB         26.3MB    U
e1-web:1.1   9466840bb879       93.2MB         26.3MB    U

# 실습 디렉토리 생성
:~$ mkdir -p Codyssey/Projects/E1-1/compose
# Compose 실습 디렉토리로 이동
:~$ cd Codyssey/Projects/E1-1/compose/

# Compose 설정 파일 편집 및 확인
:~/Codyssey/Projects/E1-1/compose$ nano docker-compose.yml 
:~/Codyssey/Projects/E1-1/compose$ cat docker-compose.yml
```
```yml
services:
  web:
    image: e1-web:1.1
    ports:
      - "127.0.0.1:8082:80"
```
```bash
# 변수 치환 후 Compose 설정 확인
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

# 서비스를 백그라운드로 생성·실행
:~/Codyssey/Projects/E1-1/compose$ sudo docker compose -p e1-bonus up -d
[+] Running 2/2
 ✔ Network e1-bonus_default  Created               0.1s
 ✔ Container e1-bonus-web-1  Started               0.2s

# 서비스 상태·포트 연결 확인
:~/Codyssey/Projects/E1-1/compose$ sudo docker compose -p e1-bonus ps
NAME             IMAGE        COMMAND                  SERVICE   CREATED          STATUS         PORTS
e1-bonus-web-1   e1-web:1.1   "/docker-entrypoint.…"   web       10 seconds ago   Up 9 seconds   127.0.0.1:8082->80/tcp

# web 서비스의 최근 로그 확인
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
# Compose 설정 파일 편집
:~/Codyssey/Projects/E1-1/compose$ nano docker-compose.yml

# 저장된 Compose 설정 확인
:~/Codyssey/Projects/E1-1/compose$ cat docker-compose.yml
services:
  web:
    image: e1-web:1.1
    ports:
      - "127.0.0.1:8082:80"

  client:
    image: alpine:3.20
    command: ["sleep", "infinity"]

# Compose 설정 검증 (정상 출력 생략)
:~/Codyssey/Projects/E1-1/compose$ sudo docker compose -p e1-bonus config --quiet

# 서비스를 백그라운드로 생성·실행
:~/Codyssey/Projects/E1-1/compose$ sudo docker compose -p e1-bonus up -d
[+] Running 4/4
 ✔ client Pulled                                         4.0s
   ✔ 3f26bc2dec0b Pull complete                          1.3s
   ✔ cee42a41056b Download complete                      0.0s
   ✔ 8b093306b817 Download complete                      0.0s
[+] Running 2/2
 ✔ Container e1-bonus-web-1     Running                  0.0s
 ✔ Container e1-bonus-client-1  Started                  0.6s

# 서비스 상태·포트 연결 확인
:~/Codyssey/Projects/E1-1/compose$ sudo docker compose -p e1-bonus ps
NAME                IMAGE         COMMAND                  SERVICE   CREATED          STATUS          PORTS
e1-bonus-client-1   alpine:3.20   "sleep infinity"         client    22 seconds ago   Up 21 seconds
e1-bonus-web-1      e1-web:1.1    "/docker-entrypoint.…"   web       3 hours ago      Up 3 hours      127.0.0.1:8082->80/tcp

# client에서 web 서비스로 HTTP 요청
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
```

### 실행 결과

- `web`과 `client`가 모두 실행 상태로 확인됨.
- `wget -qO- http://web:80/` 요청으로 HTML을 수신함.
- `client`의 `localhost`가 아닌 `web`으로 다른 서비스에 접근함.

## 3. Docker Compose 운영

접속 로그 확인 후 프로젝트를 종료·정리하고 남은 컨테이너를 확인함.

```bash
# web 서비스의 최근 로그 확인
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
# 프로젝트 컨테이너·기본 네트워크 정리
:~/Codyssey/Projects/E1-1/compose$ sudo docker compose -p e1-bonus down
[+] Running 3/3
 ✔ Container e1-bonus-client-1  Removed                 10.2s
 ✔ Container e1-bonus-web-1     Removed                  0.3s
 ✔ Network e1-bonus_default     Removed                  0.1s

# 종료된 것을 포함한 프로젝트 컨테이너 확인
:~/Codyssey/Projects/E1-1/compose$ sudo docker compose -p e1-bonus ps -a
NAME      IMAGE     COMMAND   SERVICE   CREATED   STATUS    PORTS
```

### 실행 결과

- `Wget`의 `GET /` 요청에 HTTP `200`이 기록됨.
- Chrome DevTools 관련 경로의 `404`는 해당 파일이 없다는 기록이며, 위 HTML 요청의 성공과 구분함.
- `down`으로 두 컨테이너와 `e1-bonus_default` 네트워크가 삭제됨.
- `ps -a`는 헤더만 출력되어 해당 프로젝트에 남은 컨테이너가 없음을 확인함.
- 다음 환경변수 실습에서 `up -d`로 다시 실행함.

## 4. 환경변수 활용 — .env 파일 사용

Compose 설정 파일과 같은 디렉토리의 `.env`를 편집하고 `--env-file .env`로 읽어 적용함. 호스트 접속 포트는 8083으로 유지하고 컨테이너 환경변수와 Nginx 내부 포트를 80에서 8080으로 변경함.

아래는 실제 실행 기록임. 복사 과정에서 명령·단어 중간에 삽입된 공백과 프롬프트 표기를 정리하고 `#` 설명을 추가함. `.env`와 템플릿의 편집 화면 대신, 적용된 Compose 설정·컨테이너 환경변수·Nginx 설정 출력으로 결과를 확인함.

### 4-1. .env·템플릿 편집 및 초기 설정 적용

```bash
# Compose 실습 디렉토리로 이동
:~$ cd Codyssey/Projects/E1-1/compose/
# .env 환경변수 파일 편집
:~/Codyssey/Projects/E1-1/compose$ nano .env
# Nginx 템플릿 디렉토리 생성
:~/Codyssey/Projects/E1-1/compose$ mkdir -p templates/
# Nginx 설정 템플릿 편집
:~/Codyssey/Projects/E1-1/compose$ nano templates/default.conf.template
# Compose 서비스 설정 편집
:~/Codyssey/Projects/E1-1/compose$ nano docker-compose.yml
# .env를 읽어 최종 Compose 설정 확인
:~/Codyssey/Projects/E1-1/compose$ sudo docker compose --env-file .env -p e1-bonus config
[sudo] password for student:
name: e1-bonus
services:
  client:
    command:
      - sleep
      - infinity
    image: alpine:3.20
    networks:
      default: null
  web:
    environment:
      APP_PORT: "80"
    image: e1-web:1.1
    networks:
      default: null
    ports:
      - mode: ingress
        host_ip: 127.0.0.1
        target: 80
        published: "8083"
        protocol: tcp
    volumes:
      - type: bind
        source: /home/student/Codyssey/Projects/E1-1/compose/templates
        target: /etc/nginx/templates
        read_only: true
        bind:
          create_host_path: true
networks:
  default:
    name: e1-bonus_default
# 설정 변경을 적용하여 백그라운드 실행
:~/Codyssey/Projects/E1-1/compose$ sudo docker compose --env-file .env -p e1-bonus up -d
[+] Running 2/2
 ✔ Container e1-bonus-client-1  Running                                    0.0s
 ✔ Container e1-bonus-web-1     Started                                    1.5s
# 서비스 상태와 포트 연결 확인
:~/Codyssey/Projects/E1-1/compose$ sudo docker compose --env-file .env -p e1-bonus ps
NAME                IMAGE         COMMAND                  SERVICE   CREATED           STATUS         PORTS
e1-bonus-client-1   alpine:3.20   "sleep infinity"         client    41 hours ago    Up 41 hours
e1-bonus-web-1      e1-web:1.1    "/docker-entrypoint.…"   web       6 seconds ago   Up 5 seconds   127.0.0.1:8083->80/tcp
```

### 4-2. 변경 전 내부 포트·응답 확인

```bash
# 컨테이너의 APP_PORT 값 확인
:~/Codyssey/Projects/E1-1/compose$ sudo docker compose --env-file .env -p e1-bonus exec web printenv APP_PORT
80
# 실제로 적용된 Nginx 설정 확인
:~/Codyssey/Projects/E1-1/compose$ sudo docker compose --env-file .env -p e1-bonus exec web cat /etc/nginx/conf.d/default.conf
server {
    listen 80;
    listen [::]:80;
    server_name localhost;

    location / {
        root /usr/share/nginx/html;
        index index.html index.htm;
    }

    error_page 500 502 503 504 /50x.html;
    location = /50x.html {
        root /usr/share/nginx/html;
    }
}
# 호스트 접속 주소에서 HTML 응답 확인
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

```bash
```

### 4-3. .env 수정 및 재적용

```bash
# .env 환경변수 파일 편집
:~/Codyssey/Projects/E1-1/compose$ nano .env
# 설정 유효성 검사 (정상이면 출력 없음)
:~/Codyssey/Projects/E1-1/compose$ sudo docker compose --env-file .env -p e1-bonus config --quiet
# 설정 변경을 적용하여 백그라운드 실행
:~/Codyssey/Projects/E1-1/compose$ sudo docker compose --env-file .env -p e1-bonus up -d
[+] Running 2/2
 ✔ Container e1-bonus-web-1     Started                                      0.6s
 ✔ Container e1-bonus-client-1  Running                                      0.0s
:~/Codyssey/Projects/E1-1/compose$
```

### 4-4. 변경 후 내부 포트·응답 확인

```bash
# 컨테이너의 APP_PORT 값 확인
:~/Codyssey/Projects/E1-1/compose$ sudo docker compose --env-file .env -p e1-bonus exec web printenv APP_PORT
8080
# 실제로 적용된 Nginx 설정 확인
:~/Codyssey/Projects/E1-1/compose$ sudo docker compose --env-file .env -p e1-bonus exec web cat /etc/nginx/conf.d/default.conf
server {
    listen 8080;
    listen [::]:8080;
    server_name localhost;

    location / {
        root /usr/share/nginx/html;
        index index.html index.htm;
    }

    error_page 500 502 503 504 /50x.html;
    location = /50x.html {
        root /usr/share/nginx/html;
    }
}
# 서비스 상태와 포트 연결 확인
:~/Codyssey/Projects/E1-1/compose$ sudo docker compose --env-file .env -p e1-bonus ps
NAME                IMAGE         COMMAND                  SERVICE   CREATED                                           STATUS              PORTS
e1-bonus-client-1   alpine:3.20   "sleep infinity"         client    41 hours ago                                      Up 41 hours
e1-bonus-web-1      e1-web:1.1    "/docker-entrypoint.…"   web       About a minute ago   Up About a minute   80/tcp, 127.0.0.1:8083->8080/tcp
# 호스트 접속 주소에서 HTML 응답 확인
:~/Codyssey/Projects/E1-1/compose$ curl http://127.0.0.1:8083/
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
:~/Codyssey/Projects/E1-1/compose$
```

### 변경 전후 비교

| 항목 | 변경 전 | 변경 후 |
| --- | --- | --- |
| 컨테이너 APP_PORT | 80 | 8080 |
| Nginx listen | 80 | 8080 |
| 호스트 접속 포트 | 8083 | 8083 |
| 포트 연결 | 8083 → 80 | 8083 → 8080 |
| curl 응답 | 기존 HTML 수신 | 동일한 HTML 수신 |

웹페이지 코드와 호스트 접속 주소를 유지하면서 `.env` 수정 및 `up -d` 재적용으로 Nginx 내부 포트를 변경함. `ps`의 별도 `80/tcp` 표시는 이미지의 노출 포트 정보이며, 실제 게시 연결은 `127.0.0.1:8083->8080/tcp`로 확인됨.

`.env`는 Compose 설정의 변수 치환에 사용되며, 컨테이너에서 사용할 값은 `environment`로 전달함. 파일 수정만으로 실행 중인 서비스가 변경되지는 않으므로 `up -d`로 다시 적용함. 관련 개념은 [주요 개념](Main.md#4-환경변수-활용)을 참고.


<details>
<summary>기존 환경변수 실행 기록 — 명령별 env 전달 방식</summary>

아래는 기존 방식으로 수행한 기록이며 .env 방식의 실행 증거가 아님. `#` 설명 주석만 추가함.


Compose의 `${WEB_PORT:-8082}`를 사용하여 호스트 접속 포트를 변경함. 값이 없거나 비어 있으면 기본값 `8082`를 사용함. 이번 실습은 Compose 설정 치환이며, 컨테이너 내부 환경변수 주입이나 Nginx의 내부 리슨 포트 변경은 아님.

### 4-1. 호스트 포트 변경 전 확인

파일 수정 후 `WEB_PORT=8082`를 명시하여 실행하고 접속함. 이는 변수를 생략한 기본값 동작 시험과는 구분함.

```bash
# Compose 설정 파일 편집
:~/Codyssey/Projects/E1-1/compose$ nano docker-compose.yml

# 명령에 지정한 환경변수로 서비스 실행 (기존 방식)
:~/Codyssey/Projects/E1-1/compose$ sudo env WEB_PORT=8082 docker compose -p e1-bonus up -d
[+] Running 3/3
 ✔ Network e1-bonus_default     Created                  0.1s
 ✔ Container e1-bonus-web-1     Started                  0.4s
 ✔ Container e1-bonus-client-1  Started                  0.4s

# 서비스 상태·포트 연결 확인
:~/Codyssey/Projects/E1-1/compose$ sudo docker compose -p e1-bonus ps
NAME                IMAGE         COMMAND                  SERVICE   CREATED          STATUS          PORTS
e1-bonus-client-1   alpine:3.20   "sleep infinity"         client    13 seconds ago   Up 12 seconds
e1-bonus-web-1      e1-web:1.1    "/docker-entrypoint.…"   web       13 seconds ago   Up 12 seconds   127.0.0.1:8082->80/tcp

# 호스트 포트로 웹페이지 응답 확인
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
# 저장된 Compose 설정 확인
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

### 4-2. 호스트 포트 변경 및 접속 확인

같은 설정 파일에서 변수 값만 `8083`으로 변경하여 적용하고 접속함.

```bash
# 명령에 지정한 환경변수로 서비스 실행 (기존 방식)
:~/Codyssey/Projects/E1-1/compose$ sudo env WEB_PORT=8083 docker compose -p e1-bonus up -d
[+] Running 2/2
 ✔ Container e1-bonus-client-1  Running                  0.0s
 ✔ Container e1-bonus-web-1     Started                  0.4s

# 서비스 상태·포트 연결 확인
:~/Codyssey/Projects/E1-1/compose$ sudo docker compose -p e1-bonus ps
NAME                IMAGE         COMMAND                  SERVICE   CREATED         STATUS         PORTS
e1-bonus-client-1   alpine:3.20   "sleep infinity"         client    2 minutes ago   Up 2 minutes
e1-bonus-web-1      e1-web:1.1    "/docker-entrypoint.…"   web       8 seconds ago   Up 7 seconds   127.0.0.1:8083->80/tcp

# 호스트 포트로 웹페이지 응답 확인
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

### 호스트 포트 변경 전후 비교

| 항목 | 변경 전 | 변경 후 |
| --- | --- | --- |
| 변수 값 | `WEB_PORT=8082` | `WEB_PORT=8083` |
| 호스트 게시 주소 | `127.0.0.1:8082` | `127.0.0.1:8083` |
| 컨테이너 내부 포트 | `80` | `80` |
| 접속 결과 | HTML 수신 | 동일한 HTML 수신 |

웹페이지 코드나 이미지를 수정하지 않고 접속 설정을 변경함. `WEB_PORT`는 해당 명령의 Compose 프로세스에 전달되며 영구 저장되지 않음. 이후 `up` 실행 시 같은 값을 지정해야 8083 설정을 유지할 수 있음. `ps`는 현재 상태 조회이므로 변수 없이 실행해도 현재 게시된 포트를 확인할 수 있음.

### 4-3. Nginx 설정 템플릿 작성

앞선 WEB_PORT 실습은 호스트 게시 포트의 치환임. 이어서 APP_PORT를 컨테이너에 주입하고 Nginx가 실제 사용하는 내부 포트를 변경함. 템플릿은 읽기 전용으로 연결하고 이미지의 HTML은 유지함.

```bash
# Nginx 포트 치환용 템플릿 확인
:~/Codyssey/Projects/E1-1/compose$ cat templates/default.conf.template
server {
    listen ${APP_PORT};
    listen [::]:${APP_PORT};
    server_name localhost;

    location / {
        root /usr/share/nginx/html;
        index index.html index.htm;
    }

    error_page 500 502 503 504 /50x.html;
    location = /50x.html {
        root /usr/share/nginx/html;
    }
}
```

### 4-4. 내부 포트 변경 전 확인

`APP_PORT=80` 주입 후 Nginx 설정과 접속을 확인함. 이 단계에서는 포트 연결에 `WEB_PORT`의 기본값 80을 사용함.

```bash
# 컨테이너의 APP_PORT 값 확인
:~/Codyssey/Projects/E1-1/compose$ sudo docker compose -p e1-bonus exec web printenv APP_PORT
80

# 실제 적용된 Nginx 설정 확인
:~/Codyssey/Projects/E1-1/compose$ sudo docker compose -p e1-bonus exec web cat /etc/nginx/conf.d/default.conf
server {
    listen 80;
    listen [::]:80;
    server_name localhost;

    location / {
        root /usr/share/nginx/html;
        index index.html index.htm;
    }

    error_page 500 502 503 504 /50x.html;
    location = /50x.html {
        root /usr/share/nginx/html;
    }
}

# Nginx 설정 문법 검사
:~/Codyssey/Projects/E1-1/compose$ sudo docker compose -p e1-bonus exec web nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful

# 서비스 상태·포트 연결 확인
:~/Codyssey/Projects/E1-1/compose$ sudo docker compose -p e1-bonus ps
NAME                IMAGE         COMMAND                  SERVICE   CREATED          STATUS          PORTS
e1-bonus-client-1   alpine:3.20   "sleep infinity"         client    42 seconds ago   Up 41 seconds
e1-bonus-web-1      e1-web:1.1    "/docker-entrypoint.…"   web       42 seconds ago   Up 41 seconds   127.0.0.1:8083->80/tcp

# 호스트 포트로 웹페이지 응답 확인
:~/Codyssey/Projects/E1-1/compose$ curl http://127.0.0.1:8083/
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

### 4-5. 내부 포트 변경 및 접속 확인

`environment`와 `ports`의 변수를 `APP_PORT`로 맞춤. 값을 8080으로 변경하고 `up -d`로 적용한 뒤 설정과 접속을 확인함.

```bash
# Compose 설정 파일 편집
:~/Codyssey/Projects/E1-1/compose$ nano docker-compose.yml

# 저장된 Compose 설정 확인
:~/Codyssey/Projects/E1-1/compose$ cat docker-compose.yml
services:
  web:
    image: e1-web:1.1
    environment:
      APP_PORT: "${APP_PORT:-80}"
    ports:
      - "127.0.0.1:8083:${APP_PORT:-80}"
    volumes:
      - ./templates:/etc/nginx/templates:ro

  client:
    image: alpine:3.20
    command: ["sleep", "infinity"]

# Compose 설정 검증 (정상 출력 생략)
:~/Codyssey/Projects/E1-1/compose$ sudo env APP_PORT=8080 docker compose -p e1-bonus config --quiet

# 명령에 지정한 환경변수로 서비스 실행 (기존 방식)
:~/Codyssey/Projects/E1-1/compose$ sudo env APP_PORT=8080 docker compose -p e1-bonus up -d
[+] Running 2/2
 ✔ Container e1-bonus-web-1     Started                                    0.4s
 ✔ Container e1-bonus-client-1  Running                                    0.0s

# 컨테이너의 APP_PORT 값 확인
:~/Codyssey/Projects/E1-1/compose$ sudo docker compose -p e1-bonus exec web printenv APP_PORT
8080

# 실제 적용된 Nginx 설정 확인
:~/Codyssey/Projects/E1-1/compose$ sudo docker compose -p e1-bonus exec web cat /etc/nginx/conf.d/default.conf
server {
    listen 8080;
    listen [::]:8080;
    server_name localhost;

    location / {
        root /usr/share/nginx/html;
        index index.html index.htm;
    }

    error_page 500 502 503 504 /50x.html;
    location = /50x.html {
        root /usr/share/nginx/html;
    }
}

# Nginx 설정 문법 검사
:~/Codyssey/Projects/E1-1/compose$ sudo docker compose -p e1-bonus exec web nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful

# 서비스 상태·포트 연결 확인
:~/Codyssey/Projects/E1-1/compose$ sudo docker compose -p e1-bonus ps
NAME                IMAGE         COMMAND                  SERVICE   CREATED              STATUS              PORTS
e1-bonus-client-1   alpine:3.20   "sleep infinity"         client    4 minutes ago        Up 4 minutes
e1-bonus-web-1      e1-web:1.1    "/docker-entrypoint.…"   web       About a minute ago   Up About a minute   80/tcp, 127.0.0.1:8083->8080/tcp

# 호스트 포트로 웹페이지 응답 확인
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

### 내부 포트 변경 전후 비교

| 항목 | 변경 전 | 변경 후 |
| --- | --- | --- |
| 컨테이너 APP_PORT | 80 | 8080 |
| Nginx listen | 80 | 8080 |
| 호스트 포트 | 8083 | 8083 |
| 연결 | 8083 → 80 | 8083 → 8080 |
| nginx -t | 성공 | 성공 |
| curl 응답 | 기존 HTML | 동일한 HTML |

호스트 접속 포트와 HTML은 유지하고 Nginx 내부 포트만 변경됨. `ps`의 별도 `80/tcp` 표시는 이미지의 노출 포트 정보임.

환경변수 치환과 포트 연결의 의미는 [주요 개념](Main.md#4-환경변수-활용)을 참고.

</details>
