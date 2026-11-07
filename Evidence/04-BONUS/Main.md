[← README로 돌아가기](../../README.md) | [Compose 실습 로그](Compose.md) | [GitHub 실습 로그](GitHub.md)

# 보너스 과제 주요 개념

## 1. Docker Compose 기초

- 컨테이너 실행 설정을 YAML 파일로 관리함.
- `services` 아래에 서비스별 이미지, 포트, 실행 명령 등을 정의함.
- 이번 실습에서는 `e1-web:1.1` 이미지의 웹 서버를 `web` 서비스로 실행함.
- `-p e1-bonus`는 Compose 프로젝트 이름을 지정함. 다른 Docker 명령의 포트 게시 옵션 `-p`와 구분함.

## 2. Docker Compose 멀티 컨테이너

- `web`과 보조 서비스 `client`를 같은 Compose 프로젝트에서 실행함.
- 별도 네트워크를 지정하지 않은 두 서비스가 기본 네트워크에 연결됨.
- 같은 네트워크에서 서비스 이름 `web`으로 상대 컨테이너에 접근함.
- 내부 요청은 `http://web:80/`을 사용하며 호스트에 게시한 `8082` 또는 `8083`과 구분함.
- `client`의 `sleep infinity`는 컨테이너의 주 프로세스를 유지하기 위한 설정임.

## 3. Docker Compose 운영

| 명령·옵션 | 의미 |
| --- | --- |
| `config --quiet` | 설정을 검증하며 정상 설정 출력은 생략함 |
| `up -d` | 설정을 적용하여 백그라운드로 서비스를 실행함 |
| `ps` | 프로젝트 컨테이너의 상태와 포트 등을 조회함 |
| `logs --tail=20 web` | web 서비스의 최근 로그 20줄을 조회함 |
| `exec client ...` | 실행 중인 client 컨테이너에서 명령을 실행함 |
| `down` | 프로젝트의 서비스 컨테이너와 기본 네트워크 등을 정리함 |
| `ps -a` | 종료된 것을 포함한 프로젝트 컨테이너 상태를 조회함 |

이번 `down`에는 이미지 삭제나 볼륨 삭제 옵션을 지정하지 않음.

## 4. 환경변수 활용

### 호스트 포트 변경

- `127.0.0.1:${WEB_PORT:-8082}:80`은 호스트 게시 포트만 변수로 설정함.
- `${WEB_PORT:-8082}`는 값이 없거나 비어 있으면 `8082`를 사용함.
- `sudo env WEB_PORT=8083 docker compose -p e1-bonus up -d`는 해당 실행에 변수 값을 전달함.
- 변수 값 변경으로 호스트 접속 포트를 8082에서 8083으로 변경하고 동일한 HTML 응답을 확인함.
- 컨테이너 내부 Nginx 포트는 80으로 유지됨. Compose 치환과 컨테이너의 `environment:` 설정은 서로 다름.
- 코드·이미지와 실행 설정을 분리하면 동일한 웹 콘텐츠를 다른 접속 설정으로 제공할 수 있음.

### 컨테이너 환경변수 주입과 실제 서버 설정

- `environment.APP_PORT`는 컨테이너 프로세스에 변수를 전달함.
- Nginx 이미지의 시작 스크립트가 `/etc/nginx/templates/default.conf.template`의 `${APP_PORT}`를 치환하여 `/etc/nginx/conf.d/default.conf`를 생성함.
- ports의 대상 포트도 같은 APP_PORT로 맞춰 서버의 리슨 포트와 연결이 일치하도록 함.
- read-only 마운트는 원본 템플릿 경로에 적용되며 생성된 Nginx 설정 파일은 컨테이너 내부에 기록됨.
- 명령어: `sudo env APP_PORT=8080 docker compose -p e1-bonus up -d`
- 실행 결과: 컨테이너 환경변수와 Nginx 내부 포트 8080 적용, 호스트 8083에서 동일한 HTML 수신.
- 변경 전후 기록은 [Compose 실습 로그](Compose.md)에 보관함.

### .env 파일로 설정값 저장

- `.env`에 `WEB_PORT=8083`, `APP_PORT=8080`처럼 값을 저장하여 다음 실행에도 재사용함.
- `sudo docker compose --env-file .env -p e1-bonus up -d`로 지정한 파일의 값을 읽어 설정을 적용함.
- `${WEB_PORT:-8082}`는 호스트 포트, `${APP_PORT:-80}`는 내부 포트에 사용함.
- `.env`는 Compose 치환용이며 컨테이너로 자동 주입되지 않음. `environment.APP_PORT`로 전달한 값을 Nginx 템플릿에서 사용함.
- 파일 수정만으로 실행 중인 서비스가 변경되지는 않으므로 `up -d`로 다시 적용함.
- Compose 프로세스의 같은 이름의 셸 환경변수는 `.env`보다 우선할 수 있으므로 `config`로 적용값을 확인함.
- [.env 실행 기록](Compose.md#4-환경변수-활용--env-파일-사용)에서 호스트 8083을 유지한 채 APP_PORT와 Nginx 내부 포트를 80에서 8080으로 변경하고 동일한 HTML 응답을 확인함.

## 5. GitHub SSH 키 설정

### HTTPS와 SSH 인증

- HTTPS와 SSH는 원격 저장소에 접근하는 전송·인증 방식임. HTTPS는 토큰·자격증명 도구 등을, SSH는 등록된 공개키와 대응하는 개인키를 사용함.
- 공개키는 GitHub에 등록하며 개인키와 키 암호는 공유하거나 레포에 올리지 않음.

### PowerShell에서 SSH 키 생성·등록·인증

- 개인 Windows 노트북의 PowerShell에서 SSH 관련 명령을 실행함.
- `ssh-keygen -t ed25519 -C "Codyssey-GitHub"`로 Ed25519 키를 생성함.
- `Get-Content "$HOME\.ssh\id_ed25519.pub" | Set-Clipboard`로 공개키를 복사하여 GitHub에 등록함.
- `ssh -T git@github.com`으로 GitHub 계정 인증을 확인함.

### 서버 호스트 키 확인

- 호스트 키는 접속할 서버의 신원을 확인하기 위한 키임. 첫 연결 시 서버 호스트 키를 확인하고 신뢰 여부를 선택함.

### 원격 주소 변경 및 푸시

| 명령어 | 실행 결과 |
| --- | --- |
| `git remote set-url origin git@github.com:GGam-GoYang/Codyssey-E1-1.git` | 현재 레포의 origin 주소를 SSH 방식으로 변경 |
| `git remote -v` | fetch·push 원격 주소 확인 |
| `git status -sb` | 현재 브랜치와 작업 상태 확인 |
| `git push origin HEAD` | 현재 브랜치를 origin으로 푸시 |

`Everything up-to-date`는 새로 전송할 커밋이 없다는 뜻임.

[SSH 실행 기록](GitHub.md)
