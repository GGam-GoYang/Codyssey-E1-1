[← README로 돌아가기](../../README.md)

```text
~$ docker compose version
Docker Compose version 2.40.3+ds1-0ubuntu1~24.04.1

~$ sudo docker images e1-web
[sudo] password for student:
        i Info →   U  In Use
IMAGE        ID             DISK USAGE   CONTENT SIZE   EXTRA
e1-web:1.0   be48e83b5367       93.2MB         26.3MB    U
e1-web:1.1   9466840bb879       93.2MB         26.3MB    U

~$ sudo docker ps --format 'table {{.Names}}\t{{.Image}}\t{{.Ports}}'
NAMES         IMAGE        PORTS
e1-web-8081   e1-web:1.1   127.0.0.1:8081->80/tcp
e1-web-8080   e1-web:1.1   127.0.0.1:8080->80/tcp
e1-web        e1-web:1.0   80/tcp

~$ sudo docker images e1-web
        i Info →   U  In Use
IMAGE        ID             DISK USAGE   CONTENT SIZE   EXTRA
e1-web:1.0   be48e83b5367       93.2MB         26.3MB    U
e1-web:1.1   9466840bb879       93.2MB         26.3MB    U

~$ mkdir -p Codyssey/Projects/E1-1/compose
~$ cd Codyssey/Projects/E1-1/compose/

~/Codyssey/Projects/E1-1/compose$ nano docker-compose.yml 
~/Codyssey/Projects/E1-1/compose$ sudo docker compose -p e1-bonus config
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

~/Codyssey/Projects/E1-1/compose$ sudo docker compose -p e1-bonus up -d
[+] Running 2/2
 ✔ Network e1-bonus_default  Created               0.1s
 ✔ Container e1-bonus-web-1  Started               0.2s

~/Codyssey/Projects/E1-1/compose$ sudo docker compose -p e1-bonus ps
NAME             IMAGE        COMMAND                  SERVICE   CREATED          STATUS         PORTS
e1-bonus-web-1   e1-web:1.1   "/docker-entrypoint.…"   web       10 seconds ago   Up 9 seconds   127.0.0.1:8082->80/tcp

~/Codyssey/Projects/E1-1/compose$ sudo docker compose -p e1-bonus logs --tail=30 web
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

~/Codyssey/Projects/E1-1/compose$
```