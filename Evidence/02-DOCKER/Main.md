[← README로 돌아가기](../../README.md)

# Docker-Main

## 개요 : 본 문서는 Docker 사용 중 학습한 개념들을 아래와 같이 정리한 문서임.

## Docker 설치 및 운영

### 이미지·컨테이너·데몬

| 개념 | 의미 | 실습 예시 |
| --- | --- | --- |
| 이미지 | 컨테이너 생성에 사용하는 파일시스템과 실행 설정의 기반 | `ubuntu:24.04` |
| 컨테이너 | 이미지로 생성한 실행 환경. 프로세스가 종료되어도 삭제 전까지 컨테이너 상태와 파일시스템 변경분이 남음 | `e1-exit`, `e1-keep` |
| Docker CLI | 사용자가 명령을 입력하는 클라이언트 | `docker ps` |
| Docker 데몬 | 이미지·컨테이너 등을 관리하고 CLI 요청을 처리하는 서버 | `docker info`의 Server 정보 |

### 관리자 권한으로 Docker 명령 실행

- 명령어 : `sudo docker 명령`
- 실행 결과 : 해당 Docker 명령을 관리자 권한으로 실행
- `sudo`는 모든 환경에서 필수인 Docker 문법은 아니며, 이번 실습에서 사용한 실행 방식임.

```text
sudo docker ps
```

### Docker 버전 확인

- 명령어 : `docker --version`
- 실행 결과 : 설치된 Docker CLI 버전 출력
- CLI 버전 출력만으로 데몬 연결 성공까지 확인한 것은 아님.

```text
docker --version
```

### Docker 데몬 정보 확인

- 명령어 : `sudo docker info`
- 실행 결과 : 클라이언트·서버 정보와 컨테이너·이미지 개수 등의 정보 출력
- 이번 로그에서는 Server 정보가 출력되어 데몬과 통신한 것을 확인함.

```text
sudo docker info
```

### 이미지 다운로드 (pull)

- 명령어 : `sudo docker pull 이미지:태그`
- 실행 결과 : 지정한 이미지와 태그 다운로드
- `ubuntu`는 이미지 이름, `24.04`는 태그. 태그가 가리키는 이미지는 갱신될 수 있으며 digest는 이미지 콘텐츠 식별에 사용됨.

```text
sudo docker pull hello-world:latest
sudo docker pull ubuntu:24.04
```

### 이미지 목록 확인 (images)

- 명령어 : `sudo docker images`
- 실행 결과 : 로컬 이미지의 이름·태그·ID·용량 등 출력

```text
sudo docker images
```

### 새 컨테이너 생성 및 실행 (run)

- 명령어 : `sudo docker run [옵션] 이미지 [명령]`
- 실행 결과 : 이미지로 새 컨테이너를 생성하고 실행
- `--name`은 컨테이너 이름을 지정. 이미지 이름과 컨테이너 이름은 서로 다른 값이며, 기존 컨테이너 이름은 중복 사용할 수 없음.

```text
sudo docker run --name e1-hello hello-world:latest
```

### 실행 중·전체 컨테이너 목록 (ps)

- 명령어 : `sudo docker ps / sudo docker ps -a`
- 실행 결과 : 실행 중인 컨테이너 또는 종료된 컨테이너를 포함한 전체 목록 출력
- `-a`는 종료 상태도 포함. `Up`은 실행 중, `Exited`는 종료 상태를 의미함.

```text
sudo docker ps
sudo docker ps -a
```

### 대화형 컨테이너 실행

- 명령어 : `sudo docker run -it --name 이름 이미지 bash`
- 실행 결과 : 새 컨테이너의 메인 프로세스로 bash를 실행하고 대화형 터미널 연결
- `-i`는 표준 입력 유지, `-t`는 가상 터미널 할당.

```text
sudo docker run -it --name e1-exit ubuntu:24.04 bash
```

### Ubuntu 컨테이너 내부 명령

- 명령어 : `pwd / ls / cat / echo / exit`
- 실행 결과 : 현재 경로·목록·OS 정보·문구를 확인하고 셸 종료
- 이 예시는 컨테이너 내부에서 실행. e1-exit의 메인 bash에서 exit하면 컨테이너도 종료됨.

```text
pwd
ls
cat /etc/os-release
echo "Ubuntu container TEST"
exit
```

### 백그라운드 컨테이너 실행

- 명령어 : `sudo docker run -d --name 이름 이미지 명령`
- 실행 결과 : 컨테이너를 백그라운드로 실행하고 ID 출력
- `-d`는 백그라운드 실행, `bash -c`는 문자열을 셸 명령으로 실행. 셸의 `exec sleep infinity`는 Ubuntu 환경에서 bash를 sleep으로 교체하여 대기하게 함. `-d` 자체가 프로그램을 무한 실행시키는 것은 아님.

```text
sudo docker run -d --name e1-keep ubuntu:24.04 bash -c 'echo "My container started"; exec sleep infinity'
```

### 실행 중인 컨테이너에서 추가 명령 실행 (exec)

- 명령어 : `sudo docker exec -it 컨테이너 bash`
- 실행 결과 : 실행 중인 컨테이너에서 추가 bash 셸 실행
- 추가 셸에서 exit해도 메인 프로세스 sleep은 유지됨. Docker의 exec 명령과 bash 내부의 exec 명령은 구분해야 함.

```text
sudo docker exec -it e1-keep bash
```

### 로그 확인 (logs)

- 명령어 : `sudo docker logs 컨테이너`
- 실행 결과 : 컨테이너 로그 출력
- 기본적으로 컨테이너 메인 프로세스 쪽의 표준 출력·오류를 확인하는 기능. exec로 연 대화형 터미널의 모든 입력과 출력을 모으는 기능은 아님.

```text
sudo docker logs e1-hello
sudo docker logs e1-keep
```

### 리소스 사용량 확인 (stats)

- 명령어 : `sudo docker stats --no-stream 컨테이너`
- 실행 결과 : CPU·메모리·네트워크 등의 사용량을 한 번 출력
- `--no-stream`은 연속 갱신하지 않고 한 번 출력 후 종료. 수치는 측정 시점마다 달라질 수 있음.

```text
sudo docker stats --no-stream e1-keep
```

### 컨테이너 중지 (stop)

- 명령어 : `sudo docker stop 컨테이너`
- 실행 결과 : 실행 중인 컨테이너 중지. 컨테이너 자체는 삭제하지 않음
- 일반적으로 종료 신호를 보내고, 제한 시간 내 종료되지 않으면 강제 종료함.

```text
sudo docker stop e1-keep
sudo docker ps -a
```

### 기존 컨테이너 재시작 (start)

- 명령어 : `sudo docker start 컨테이너`
- 실행 결과 : 정지된 기존 컨테이너를 다시 실행
- run은 새로 생성하고 실행하지만, start는 기존 컨테이너를 다시 실행함.

```text
sudo docker start e1-keep
sudo docker ps
```

### 메인 프로세스 종료와 추가 셸 종료의 차이

| 컨테이너 | 메인 프로세스 | 종료한 대상 | 실제 결과 |
| --- | --- | --- | --- |
| `e1-hello` | 안내 메시지 출력 프로그램 | 프로그램이 작업 후 종료 | `Exited (0)` |
| `e1-exit` | `bash` | 메인 셸에서 `exit` | `Exited (0)` |
| `e1-keep` | `sleep infinity` | `docker exec`로 추가한 bash에서 `exit` | `Up` 유지 |

### 여러 줄 입력과 작은따옴표

- 작은따옴표를 닫지 않고 Enter를 누르면 Bash가 추가 입력을 기다리며 보조 프롬프트 `>`를 표시할 수 있음.
- 다음 줄에서 따옴표를 닫으면 입력이 완성되어 명령이 실행될 수 있음.
- 이번 실습에서는 다음 줄에서 따옴표를 닫은 뒤 컨테이너 ID가 출력되었으므로 첫 run은 성공한 상태였음.
- 이때 다시 run하지 말고 `docker ps -a`로 생성 여부를 확인해야 함.
- 보조 프롬프트 `>`는 이 상황에서 출력 리다이렉션 연산자가 아니라 셸의 입력 대기 표시임.

### 종료 코드 해석

| 종료 코드 | 일반적인 의미 |
| --- | --- |
| `0` | 성공적으로 종료 |
| `1` | 일반적인 오류. 상세 의미는 프로그램마다 다름 |
| `126` | 셸에서 명령을 찾았지만 실행할 수 없음 |
| `127` | 셸에서 명령을 찾을 수 없음 |
| `128 + 신호 번호` | 통상 해당 신호에 의한 종료를 표현 |

### 자주 접하는 종료 관련 신호

| 신호 번호 | 신호 이름 | 의미·주요 발생 상황 | 종료 시 통상 코드 |
| --- | --- | --- | --- |
| `1` | `SIGHUP` | 터미널 연결 종료 등을 알림. 일부 프로그램은 설정 다시 읽기 용도로 처리함 | `129` |
| `2` | `SIGINT` | 실행 중단 요청. 터미널에서 보통 `Ctrl+C`로 발생 | `130` |
| `3` | `SIGQUIT` | 종료 및 코어 덤프 요청. 터미널에서 보통 `Ctrl+\`로 발생 | `131` |
| `9` | `SIGKILL` | 즉시 강제 종료. 프로그램이 잡아서 처리하거나 무시할 수 없음 | `137` |
| `11` | `SIGSEGV` | 잘못된 메모리 접근 등으로 발생 | `139` |
| `13` | `SIGPIPE` | 읽는 쪽이 없는 파이프 등에 쓰려고 할 때 발생 | `141` |
| `15` | `SIGTERM` | 정상적인 정리 작업 후 종료할 기회를 주는 종료 요청 | `143` |

### 이번 실습에서의 종료 코드 해석

| 표시 | 해석 | 이번 로그에서 확인한 범위 |
| --- | --- | --- |
| `Exited (0)` | 메인 프로세스가 성공 종료 코드 0을 반환 | hello-world와 메인 bash 종료 |
| `Exited (137)` | 통상 128 + 9로, SIGKILL에 의한 종료를 나타냄 | stop 이후 표시됨. 구체적인 원인은 미확인 |

### 호스트 OS와 컨테이너 공간 구분

- VM의 `docker info`에는 Ubuntu 24.04.4 LTS가 표시됨.
- 컨테이너의 `/etc/os-release`에는 Ubuntu 24.04.5 LTS가 표시됨.
- 컨테이너는 이미지의 사용자 공간 파일을 사용하면서 호스트 커널을 공유하므로, 두 배포판 표시가 반드시 같을 필요는 없음.
- README의 VM 환경 값을 컨테이너 버전으로 바꾸지 않음.
