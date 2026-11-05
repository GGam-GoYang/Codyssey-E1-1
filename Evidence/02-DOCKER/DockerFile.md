# Dockerfile 이미지 빌드 및 실행 로그

## 1. 이미지 빌드 전 사전 준비

### 베이스 이미지 선택

- 베이스 이미지로 `nginx:alpine`을 사용함.
- Alpine Linux 기반으로 Nginx가 구성된 이미지임.
- 직접 작성한 HTML 파일을 제공하는 데 필요한 웹서버가 포함되어 있어 해당 이미지를 선택함.

### 커스텀 내용

```dockerfile
FROM nginx:alpine
COPY site/ /usr/share/nginx/html/
```

- `FROM`은 이미지 빌드의 기반이 되는 이미지를 지정함.
- `COPY`는 빌드 컨텍스트의 `site/` 내용을 이미지 내부의 `/usr/share/nginx/html/`에 복사함.
- 직접 작성한 `index.html`을 포함하여 Nginx 기본 페이지 대신 실습용 페이지를 제공하도록 구성함.

### 커스텀 이미지 제작 목적

- Nginx 웹 서버와 직접 작성한 웹페이지를 하나의 이미지에 포함하기 위함.
- 생성한 이미지로 컨테이너를 실행하여 작성한 웹페이지를 제공함.
- 호스트 파일 수정만으로는 기존 이미지에 복사한 내용이 변경되지 않으므로, 수정 내용을 이미지에 포함하려면 다시 빌드해야 함.

### 기존 컨테이너 확인 및 작업 폴더 준비

```text
~$ cd Codyssey/Projects/E1-1/Docker/

~/Codyssey/Projects/E1-1/Docker$ sudo docker ps -a
[sudo] password for student:
CONTAINER ID   IMAGE                COMMAND                   CREATED       STATUS                     PORTS     NAMES
931f9c9ccac5   ubuntu:24.04         "bash -c 'echo \"My c…"   2 hours ago   Exited (137) 2 hours ago             e1-keep
c4030efcc5f7   ubuntu:24.04         "bash"                    2 hours ago   Exited (0) 2 hours ago               e1-exit
03d5d9d30be7   hello-world:latest   "/hello"                  2 hours ago   Exited (0) 2 hours ago               e1-hello

~/Codyssey/Projects/E1-1/Docker$ mkdir -p web/site
~/Codyssey/Projects/E1-1/Docker$ cd web
```

## 2. Dockerfile∙웹페이지 작성 및 내용 확인

```text
~/Codyssey/Projects/E1-1/Docker/web$ nano site/index.html
```
```html
<title>Codyssey E1-1 Mission Page</title>
<body>
<h1>안녕하세요.</h1>
<h2>Dockerfile로 만든 이미지를 통해 제공하는 페이지입니다!</h2>
</body>
```
```text
~/Codyssey/Projects/E1-1/Docker/web$ nano Dockerfile
```
```text
FROM nginx:alpine
COPY site/ /usr/share/nginx/html/
```

> 위는 `e1-web:1.0`의 최초 빌드 당시 HTML 코드임.<br>
이후 브라우저 접속 시 한글 깨짐이 발생하여 HTML에 UTF-8 선언을 추가하고 `e1-web:1.1`로 빌드한 뒤 컨테이너를 재생성함.<br>상세 과정은 [트러블슈팅 사례 1](../99-TROUBLESHOOTING/TROUBLESHOOTING.md#사례-1) 참고.

## 3. 커스텀 이미지 빌드

```text
~/Codyssey/Projects/E1-1/Docker/web$ sudo docker build -t e1-web:1.0 .
DEPRECATED: The legacy builder is deprecated and will be removed in a future release.
            Install the buildx component to build images with BuildKit:
            https://docs.docker.com/go/buildx/

Sending build context to Docker daemon  3.584kB
Step 1/2 : FROM nginx:alpine
alpine: Pulling from library/nginx
cbe54b031f3e: Pulling fs layer
d3531b7a4f21: Pulling fs layer
1408044c7403: Pulling fs layer
7b9d02243238: Pulling fs layer
567a969e0221: Pulling fs layer
f523b340a6c0: Pulling fs layer
dbb1ef19121a: Pulling fs layer
c2fefbef5314: Pulling fs layer
c2fefbef5314: Download complete
1408044c7403: Download complete
7b9d02243238: Download complete
567a969e0221: Download complete
f523b340a6c0: Download complete
dbb1ef19121a: Download complete
d3531b7a4f21: Download complete
2d759c5f2f47: Download complete
d3531b7a4f21: Pull complete
db5a6d6bb46f: Download complete
cbe54b031f3e: Download complete
c2fefbef5314: Pull complete
1408044c7403: Pull complete
7b9d02243238: Pull complete
567a969e0221: Pull complete
f523b340a6c0: Pull complete
dbb1ef19121a: Pull complete
cbe54b031f3e: Pull complete
Digest: sha256:785835ea65db4bba6f021acc0f3be91b0fef76a53fe41f8588c6698d2aed0606
Status: Downloaded newer image for nginx:alpine
 ---> 785835ea65db
Step 2/2 : COPY site/ /usr/share/nginx/html/
 ---> be48e83b5367
Successfully built be48e83b5367
Successfully tagged e1-web:1.0
```

## 4. 빌드 이미지 확인 및 컨테이너 실행

```
~/Codyssey/Projects/E1-1/Docker/web$ sudo docker images e1-web
                                                                                                           i Info →   U  In Use
IMAGE        ID             DISK USAGE   CONTENT SIZE   EXTRA
e1-web:1.0   be48e83b5367       93.2MB         26.3MB
~/Codyssey/Projects/E1-1/Docker/web$ sudo docker run -d --name e1-web e1-web:1.0
0df9ea93d54d62d9b6175a32573a454779fdbbbbea29b4bb7ce07456ad9561b3
~/Codyssey/Projects/E1-1/Docker/web$ sudo docker ps
CONTAINER ID   IMAGE        COMMAND                  CREATED         STATUS         PORTS     NAMES
0df9ea93d54d   e1-web:1.0   "/docker-entrypoint.…"   6 seconds ago   Up 5 seconds   80/tcp    e1-web
```

## 5. NGINX 로그 확인
  
```
~/Codyssey/Projects/E1-1/Docker/web$ sudo docker logs e1-web
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2026/11/04 06:38:17 [notice] 1#1: using the "epoll" event method
2026/11/04 06:38:17 [notice] 1#1: nginx/1.31.8
2026/11/04 06:38:17 [notice] 1#1: built by gcc 15.2.0 (Alpine 15.2.0)
2026/11/04 06:38:17 [notice] 1#1: OS: Linux 6.8.0-134-generic
2026/11/04 06:38:17 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1024:524288
2026/11/04 06:38:17 [notice] 1#1: start worker processes
2026/11/04 06:38:17 [notice] 1#1: start worker process 30
2026/11/04 06:38:17 [notice] 1#1: start worker process 31
```

## 6. 컨테이너 내부 index.html 파일 확인
  
```
~/Codyssey/Projects/E1-1/Docker/web$ sudo docker exec e1-web cat /usr/share/nginx/html/index.html
<title>Codyssey E1-1 Mission Page</title>
<body>
<h1>안녕하세요.</h1>
<h2>Dockerfile로 만든 이미지를 통해 제공하는 페이지입니다!</h2>
</body>
```
