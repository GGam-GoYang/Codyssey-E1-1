# Docker 설치 및 운영 로그

<details>
<summary><h2>1. Docker 버전·데몬 및 초기 상태 확인</h2></summary>

```text
~/Codyssey/Projects/E1-1/Docker$ docker --version # 도커 버전 확인
Docker version 29.1.3, build 29.1.3-0ubuntu3~24.04.2

~/Codyssey/Projects/E1-1/Docker$ sudo docker info # 도커 상세 정보 및 데몬과의 통신 확인
Client:
 Version:    29.1.3
 Context:    default
 Debug Mode: false
 Plugins:
  compose: Docker Compose (Docker Inc.)
    Version:  2.40.3+ds1-0ubuntu1~24.04.1
    Path:     /usr/libexec/docker/cli-plugins/docker-compose
  trust: Manage trust on Docker images (Docker Inc.)
    Version:  29.1.3
    Path:     /usr/libexec/docker/cli-plugins/docker-trust

Server:
 Containers: 0
  Running: 0
  Paused: 0
  Stopped: 0
 Images: 0
 Server Version: 29.1.3
 Storage Driver: overlayfs
  driver-type: io.containerd.snapshotter.v1
 Logging Driver: json-file
 Cgroup Driver: systemd
 Cgroup Version: 2
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local splunk syslog
 CDI spec directories:
  /etc/cdi
  /var/run/cdi
 Swarm: inactive
 Runtimes: io.containerd.runc.v2 runc
 Default Runtime: runc
 Init Binary: docker-init
 containerd version:
 runc version:
 init version:
 Security Options:
  apparmor
  seccomp
   Profile: builtin
  cgroupns
 Kernel Version: 6.8.0-134-generic
 Operating System: Ubuntu 24.04.4 LTS
 OSType: linux
 Architecture: aarch64
 CPUs: 2
 Total Memory: 1.913GiB
 Name: lima-codyssey
 ID: 5bf558b1-f0f9-4f80-94b9-43a67b4d5391
 Docker Root Dir: /var/lib/docker
 Debug Mode: false
 Experimental: false
 Insecure Registries:
  ::1/128
  127.0.0.0/8
 Live Restore Enabled: false
 Firewall Backend: iptables

~/Codyssey/Projects/E1-1/Docker$ sudo docker ps -a # 도커 컨테이너 전체 상태 확인
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```
</details>

<details>
<summary><h2>2. 이미지 다운로드·목록 확인</h2></summary>

```text
# hello-world의 latest 이미지를 pull
~/Codyssey/Projects/E1-1/Docker$ sudo docker pull hello-world:latest 
latest: Pulling from library/hello-world
58dee6a49ef1: Pull complete
c3bdf82c34d1: Download complete
Digest: sha256:c567b231f65614688711a75f31518a064258a51d442117a3e771a3a8fdba78ef
Status: Downloaded newer image for hello-world:latest
docker.io/library/hello-world:latest

# Ubuntu 24.04 릴리즈의 이미지를 pull
~/Codyssey/Projects/E1-1/Docker$ sudo docker pull ubuntu:24.04
24.04: Pulling from library/ubuntu
e2d2bb84d022: Pull complete
2238dc1b4376: Download complete
Digest: sha256:fe81560990fae86dc4743202b2cb4d3df98d0bcf949173cecef15ce1127ef852
Status: Downloaded newer image for ubuntu:24.04
docker.io/library/ubuntu:24.04

# 로컬에 설치된 이미지 목록 출력
~/Codyssey/Projects/E1-1/Docker$ sudo docker images
                                                                     i Info →   U  In Use
IMAGE                ID             DISK USAGE   CONTENT SIZE   EXTRA
hello-world:latest   c567b231f656       22.6kB         10.3kB
ubuntu:24.04         fe81560990fa        141MB         30.9MB
```
</details>

<details>
<summary><h2>3. hello-world 실행∙종료 상태∙로그 확인</h2></summary>

```text
# hello-world:latest로 'e1-hello'라는 이름의 컨테이너 생성∙실행
~/Codyssey/Projects/E1-1/Docker$ sudo docker run --name e1-hello hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (arm64v8)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/

# 실행 중인 컨테이너 목록 출력(hello-world의 경우 텍스트 출력 후 자동 종료되기에 미포함)
~/Codyssey/Projects/E1-1/Docker$ sudo docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

# 모든 상태의 컨테이너 목록 출력(종료되었던 hello-world 출력 확인)
~/Codyssey/Projects/E1-1/Docker$ sudo docker ps -a
CONTAINER ID   IMAGE                COMMAND    CREATED          STATUS                      PORTS     NAMES
03d5d9d30be7   hello-world:latest   "/hello"   22 seconds ago   Exited (0) 21 seconds ago             e1-hello

# e1-hello의 전체 로그 출력
~/Codyssey/Projects/E1-1/Docker$ sudo docker logs e1-hello 
Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (arm64v8)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```
</details>

<details>
<summary><h2>4. Ubuntu 내부 명령 및 메인 셸 종료 관찰</h2></summary>

```text
# 설치된 ubuntu:24.04 이미지로 e1-exit의 이름의 컨테이너 생성 후 bash 실행.
~/Codyssey/Projects/E1-1/Docker$ sudo docker run -it --name e1-exit ubuntu:24.04 bash
root@c4030efcc5f7:/# pwd
/

root@c4030efcc5f7:/# ls
bin   dev  home  media  opt   root  sbin  sys  usr
boot  etc  lib   mnt    proc  run   srv   tmp  var

root@c4030efcc5f7:/# cat /etc/os-release
PRETTY_NAME="Ubuntu 24.04.5 LTS"
NAME="Ubuntu"
VERSION_ID="24.04"
VERSION="24.04.5 LTS (Noble Numbat)"
VERSION_CODENAME=noble
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=noble
LOGO=ubuntu-logo

root@c4030efcc5f7:/# echo "Ubuntu container TEST"
Ubuntu container TEST

# e1-exit 컨테이너의 bash 종료
root@c4030efcc5f7:/# exit
exit

# exit 명령어로 인해 실행 중인 컨테이너 목록에 e1-exit 없음
~/Codyssey/Projects/E1-1/Docker$ sudo docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

# e1-exit의 상태가 Exited 인걸 확인할 수 있음
~/Codyssey/Projects/E1-1/Docker$ sudo docker ps -a
CONTAINER ID   IMAGE                COMMAND    CREATED              STATUS                          PORTS     NAMES
c4030efcc5f7   ubuntu:24.04         "bash"     31 seconds ago       Exited (0) 5 seconds ago                  e1-exit
03d5d9d30be7   hello-world:latest   "/hello"   About a minute ago   Exited (0) About a minute ago             e1-hello
```
</details>

<details>
<summary><h2>5. 백그라운드 실행</h2></summary>

```text
# 우분투 이미지로 e1-keep 컨테이너 생성 후 백그라운드(-d) 실행 bash -c는 뒤의 '문자열'을 명령어로 인식하게 함. 
~/Codyssey/Projects/E1-1/Docker$ sudo docker run -d --name e1-keep ubuntu:24.04 bash -c 'echo "My container started"; exec sleep infinity'
# 컨테이너 ID 전달 받음
931f9c9ccac585bf3ad1d2a4c37f5419b307998cd9be4e51ed760750d351196e

# e1-keep가 Exited가 아닌 Up 상태인 걸 확인할 수 있음
~/Codyssey/Projects/E1-1/Docker$ sudo docker ps
CONTAINER ID   IMAGE          COMMAND                   CREATED          STATUS          PORTS     NAMES
931f9c9ccac5   ubuntu:24.04   "bash -c 'echo \"My c…"   17 seconds ago   Up 16 seconds             e1-keep
```
</details>

<details>
<summary><h2>6. exec 셸 종료 후 컨테이너 유지 확인</h2></summary>

```text
~/Codyssey/Projects/E1-1/Docker$ sudo docker exec -it e1-keep bash
root@931f9c9ccac5:/# pwd
/

root@931f9c9ccac5:/# ls
bin   dev  home  media  opt   root  sbin  sys  usr
boot  etc  lib   mnt    proc  run   srv   tmp  var

root@931f9c9ccac5:/# echo "shell was opened with docker exec"
shell was opened with docker exec

root@931f9c9ccac5:/# exit
exit

~/Codyssey/Projects/E1-1/Docker$ sudo docker ps
CONTAINER ID   IMAGE          COMMAND                   CREATED          STATUS          PORTS     NAMES
931f9c9ccac5   ubuntu:24.04   "bash -c 'echo \"My c…"   52 seconds ago   Up 51 seconds             e1-keep
```
</details>

<details>
<summary><h2>7. 로그∙리소스 확인</h2></summary>

```text
~/Codyssey/Projects/E1-1/Docker$ sudo docker logs e1-keep
My container started

~/Codyssey/Projects/E1-1/Docker$ sudo docker stats --no-stream e1-keep
CONTAINER ID   NAME      CPU %     MEM USAGE / LIMIT   MEM %     NET I/O         BLOCK I/O    PIDS
931f9c9ccac5   e1-keep   0.00%     416KiB / 1.913GiB   0.02%     3.49kB / 126B   0B / 4.1kB   1
```
</details>

<details>
<summary><h2>8. 중지·재시작 및 최종 상태 확인</h2></summary>

```text
~/Codyssey/Projects/E1-1/Docker$ sudo docker stop e1-keep
e1-keep

~/Codyssey/Projects/E1-1/Docker$ sudo docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

~/Codyssey/Projects/E1-1/Docker$ sudo docker ps -a
CONTAINER ID   IMAGE                COMMAND                   CREATED              STATUS                       PORTS     NAMES
931f9c9ccac5   ubuntu:24.04         "bash -c 'echo \"My c…"   About a minute ago   Exited (137) 5 seconds ago             e1-keep
c4030efcc5f7   ubuntu:24.04         "bash"                    2 minutes ago        Exited (0) 2 minutes ago               e1-exit
03d5d9d30be7   hello-world:latest   "/hello"                  3 minutes ago        Exited (0) 3 minutes ago               e1-hello

~/Codyssey/Projects/E1-1/Docker$ sudo docker start e1-keep
e1-keep

~/Codyssey/Projects/E1-1/Docker$ sudo docker ps
CONTAINER ID   IMAGE          COMMAND                   CREATED              STATUS         PORTS     NAMES
931f9c9ccac5   ubuntu:24.04   "bash -c 'echo \"My c…"   About a minute ago   Up 8 seconds             e1-keep

~/Codyssey/Projects/E1-1/Docker$ sudo docker stop e1-keep
e1-keep

~/Codyssey/Projects/E1-1/Docker$ sudo docker ps -a
CONTAINER ID   IMAGE                COMMAND                   CREATED         STATUS                       PORTS     NAMES
931f9c9ccac5   ubuntu:24.04         "bash -c 'echo \"My c…"   2 minutes ago   Exited (137) 9 seconds ago             e1-keep
c4030efcc5f7   ubuntu:24.04         "bash"                    3 minutes ago   Exited (0) 3 minutes ago               e1-exit
03d5d9d30be7   hello-world:latest   "/hello"                  4 minutes ago   Exited (0) 4 minutes ago               e1-hello

~/Codyssey/Projects/E1-1/Docker$
```
</details>
