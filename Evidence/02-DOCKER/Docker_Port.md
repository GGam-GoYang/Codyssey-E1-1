[← README로 돌아가기](../../README.md)

# 포트 매핑 및 접속 테스트 로그

## 1. 포트 매핑 구성

같은 웹 이미지로 컨테이너 두 개를 실행하고, 서로 다른 호스트 포트에 연결함.

| 컨테이너 | 이미지 | VM 호스트 포트 | 컨테이너 포트 |
| --- | --- | --- | --- |
| `e1-web-8080` | `e1-web:1.1` | `8080` | `80` |
| `e1-web-8081` | `e1-web:1.1` | `8081` | `80` |

> 실습 과정에서 한글 출력 깨짐이 발생했으며, 자세한 원인 확인 및 해결 과정은 [트러블슈팅 사례 1](../99-TROUBLESHOOTING/TROUBLESHOOTING.md#사례-1)에 기술함.

## 2. 포트 매핑 전 포트 충돌 확인

ss 명령어와 docker ps 명령어를 통해 현재 사용 중인 포트를 확인함.
아래와 같이 8080, 8081 포트가 비어있는 것을 확인할 수 있음.

```bash
:~$ sudo ss -ltnp '( sport = :8080 or sport = :8081)'
State     Recv-Q    Send-Q       Local Address:Port        Peer Address:Port    Process


:~$ sudo docker ps --format 'table {{.Names}}\t{{.Ports}}'
NAMES            PORTS
e1-web           80/tcp
```

### 2-1. 포트 충돌 시 해결 방법

1. `ss`로 충돌한 포트의 점유 상태를 확인하고, `docker ps`로 해당 포트를 게시한 컨테이너를 확인함.
2. 기존 서비스를 유지해야 한다면 새 컨테이너의 호스트 포트를 사용하지 않는 다른 포트로 변경함. 컨테이너 내부 포트는 서비스가 대기하는 포트를 유지함.
3. 기존 컨테이너가 불필요하다면 이름을 확인한 뒤 해당 컨테이너만 중지함.
4. 포트 점유 상태를 다시 확인하고 새 컨테이너를 실행한 후 접속을 검증함.

## 3. 포트 매핑 컨테이너 실행

```bash
:~/Codyssey/Projects/E1-1/Docker/web$ sudo docker run -d --name e1-web-8080 -p 127.0.0.1:8080:80 e1-web:1.1
e17075132e8ee2301f17f735ddc46fa7a606574b81613c0ec4a3d09c11411855

:~/Codyssey/Projects/E1-1/Docker/web$ sudo docker run -d --name e1-web-8081 -p 127.0.0.1:8081:80 e1-web:1.1
f3e82867d8e3ccf1e78f7fbc619394abc05c1eca56c02d26ae58e91ffb01438b
```

## 3. 브라우저 접속 확인

PC에서는 SSH 터널을 통해 VM의 웹 서버에 접속함.

| 브라우저 접속 주소 | 연결된 VM 포트 |
| --- | --- |
| `http://127.0.0.1:18080/` | `8080` |
| `http://127.0.0.1:18081/` | `8081` |

### 첫 번째 접속

```text
http://127.0.0.1:18080/
```

  ## 사진 펼치기 
  
  ![image](Images/port_18080.png)

### 두 번째 접속

```text
http://127.0.0.1:18081/
```

  ## 사진 펼치기 
  
  ![image](Images/port_18081.png)
