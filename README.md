# Codyssey-E1-1
Codyssey E1-1 Personal Mission

## 본 문서는 코디세이 올인원 1기 입학 연수 1주차 미션을 위해 작성하는 기술 문서임.
### 개요 : 실행, 배포, 디버깅 등의 개발 과정을 내 컴퓨터 이외의 다른 환경에서도<br>범용적으로 진행할 수 있는 환경 구성이 목표.


## 실행 환경
| 항목 | 실제 값 |
| --- | --- |
| OS | ARM Ubuntu 24.04.4 LTS |
| 셸 | Bash |
| 터미널 | PuTTY 0.85 |
| Docker 버전 | 29.1.3 |
| Git 버전 | 2.43.0 |
| 저장 | `https://github.com/GGam-GoYang/Codyssey-E1-1.git` |
```text
$ lsb_release -a
No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 24.04.4 LTS
Release:        24.04
Codename:       noble

$ docker --version
Docker version 29.1.3, build 29.1.3-0ubuntu3~24.04.2

$ git --version
git version 2.43.0
```

## 프로젝트 구조

```text
Codyssey-E1-1/
├── README.md                       # 프로젝트 개요, 실행 환경 및 체크리스트
├── Dockerfile                      # 커스텀 웹 서버 이미지 빌드 설정
├── STUDY_LOG.md                     # 학습 기록용 문서
├── site/
│   ├── index.html                  # 이미지 빌드에 사용하는 웹페이지
│   └── index_bind.html             # 바인드 마운트 실습에서 수정한 내용의 사본
└── Evidence/
    ├── 00-INDEX/
    │   └── Evidence.md             # 실습 문서 및 로그 링크 모음
    ├── 01-TERMINAL/
    │   ├── Main.md                 # 터미널 및 권한 주요 개념
    │   ├── Terminal.md             # 터미널 조작 명령 및 실행 결과
    │   └── Permission_Change.md    # 권한 변경 및 전후 비교 기록
    ├── 02-DOCKER/
    │   ├── Main.md                 # Docker 주요 개념
    │   ├── Docker.md               # Docker 설치 및 기본 운영 기록
    │   ├── DockerFile.md           # Dockerfile 작성 및 이미지 빌드 기록
    │   ├── Docker_Port.md          # 포트 매핑 및 접속 확인 기록
    │   ├── Docker_Bind.md          # 바인드 마운트 및 파일 변경 반영 기록
    │   ├── Docker_Volume.md        # 컨테이너 삭제 전후 볼륨 데이터 유지 기록
    │   └── Images/
    │       ├── port_18080.png      # 첫 번째 포트 접속 화면
    │       ├── port_18081.png      # 두 번째 포트 접속 화면
    │       └── bind_mount.png      # 바인드 마운트 파일 변경 후 화면
    ├── 03-GIT/
    │   ├── Main.md                 # Git 주요 개념 정리용 문서
    │   ├── Git.md                  # Git 설정 로그 및 VSCode 연동 기록
    │   └── Images/
    │       ├── git_push.png        # VSCode 소스 제어 및 커밋 상태
    │       └── git_sync.png        # VSCode GitHub 계정 로그인 상태
    └── 99-TROUBLESHOOTING/
        └── TROUBLESHOOTING.md      # 문제, 원인, 해결 및 재검증 기록
```

### 구성 기준

| 구분 | 구성 기준 |
| --- | --- |
| 루트 | 프로젝트 안내 문서와 이미지 빌드에 필요한 Dockerfile 배치 |
| `site/` | 웹페이지 소스와 바인드 마운트 실습의 수정본 사본 보관 |
| `Evidence/00-INDEX/` | 실습 문서에 접근할 수 있도록 링크를 모음 |
| `Evidence/01-TERMINAL/` | 터미널 조작 및 권한 실습의 개념과 실행 기록 보관 |
| `Evidence/02-DOCKER/` | Docker 개념과 항목별 실행 기록을 분리하여 보관 |
| `Evidence/03-GIT/` | Git 개념, 설정 로그 및 VSCode GitHub 연동 증거 보관 |
| 각 실습 폴더의 `Images/` | 해당 실습의 화면 캡처 보관 |
| `Evidence/99-TROUBLESHOOTING/` | 발생한 문제의 원인 확인, 해결 과정 및 결과 정리 |

<details>
<summary><h1>체크리스트</h1></summary>
  
## 1. GitHub 레포지토리 생성
- [x] 레포지토리를 Public으로 생성
- [x] README.md 파일 생성 및 과제 규칙에 맞는 폴더 구조 생성

## 2. README.md
- [x] 프로젝트 개요와 미션 목표 요약
- [x] 실행 환경 작성 : OS, 셸, 터미널, Docker 버전, Git 버전
- [x] 수행 항목 체크리스트 작성 : 터미널, 권한, Docker, Dockerfile, 포트, 볼륨, Git, GitHub
- [x] 검증 방법 기록
- [x] 검증 결과 파일, 링크를 통해 본문에 연결
- [x] 트러블슈팅 2건 이상 기록
- [x] 기술 문서만으로 전체 수행 내용 파악 가능한지 확인

## 3. 터미널 조작
### [주요 개념](Evidence/01-TERMINAL/Main.md#터미널-조작) | [로그 파일](Evidence/01-TERMINAL/Terminal.md)
- [x] 현재 위치, 숨김 파일을 포함한 목록 확인 및 디렉토리 이동
- [x] 파일 및 폴더의 생성, 복사, 이동, 이름 변경, 삭제
- [x] 절대 경로, 상대 경로의 이해와 핵심 명령어 정리 및 출력 기록

## 4. 권한 실습
### [주요 개념](Evidence/01-TERMINAL/Main.md#권한-실습) | [로그 파일](Evidence/01-TERMINAL/Permission_Change.md)
- [x] r/w/x 권한 구성 및 숫자 표기 이해
- [x] 1개 이상의 파일·디렉토리 권한 확인·변경과 전후 비교 기록 

## 5. Docker 설치 및 운영
### [주요 개념](Evidence/02-DOCKER/Main.md) | [로그 파일](Evidence/02-DOCKER/Docker.md)
- [x] Docker 설치 및 버전·데몬 동작 확인
- [x] 이미지 다운로드와 목록 확인 및 컨테이너 실행·중지·목록 확인
- [x] hello-world 실행 및 Ubuntu 내부 명령 수행
- [x] 컨테이너 종료/유지 차이 관찰
- [x] 컨테이너 로그·리소스 확인 및 명령어·출력 기록

## 6. Dockerfile 사용
### [주요 개념](Evidence/02-DOCKER/Main.md) | [로그 파일](Evidence/02-DOCKER/DockerFile.md)
- [ ] 베이스 선택 및 커스텀 내용·목적 설명
- [x] 웹 소스·Dockerfile 작성 및 저장소 업로드
- [x] 이미지 빌드·컨테이너 실행 후 명령어·결과 기록

## 7. 포트 매핑 및 접속 테스트
### [주요 개념](Evidence/02-DOCKER/Main.md)  | [로그 파일](Evidence/02-DOCKER/Docker_Port.md)
- [x] 포트 매핑 후 서로 다른 포트로 2회 접속
- [x] 실행 명령어와 실제 화면 캡처 및 기록

## 8. 바인드 마운트
### [주요 개념](Evidence/02-DOCKER/Main.md) | [로그 파일](Evidence/02-DOCKER/Docker_Bind.md)
- [x] 호스트·컨테이너 경로 연결 및 바인드 마운트 진행
- [x] 호스트 파일 변경 반영 확인 및 명령어·변경 전후 비교 기록

## 9. 볼륨 영속성
### [주요 개념](Evidence/02-DOCKER/Main.md) | [로그 파일](Evidence/02-DOCKER/Docker_Volume.md)
- [x] 볼륨 생성·연결 및 데이터 저장
- [x] 기존 컨테이너 삭제 후 새 컨테이너에 동일 볼륨 연결
- [x] 데이터 유지 확인 및 명령어·삭제 전후 비교 기록

## 10. Git 설정 ∙ VSCode GitHub 연동
### [주요 개념](Evidence/03-GIT/Main.md) | [로그 파일](Evidence/03-GIT/Git.md)
- [x] Git·GitHub 역할 이해 및 사용자 정보·기본 브랜치 설정, git config --list 기록
- [x] VSCode GitHub 로그인·저장소 연동 및 증거 업로드

## 11. 트러블슈팅 사례 해결 및 기록
### 트러블슈팅 기록 | [사례 1](Evidence/99-TROUBLESHOOTING/TROUBLESHOOTING.md#사례-1) | [사례 2](Evidence/99-TROUBLESHOOTING/TROUBLESHOOTING.md#사례-2)
- [x] 사례 1 - 포트 매핑 후 index.html 브라우저 접속 시 한글 출력 깨짐
- [x] 사례 2 - 바인드 마운트 진행 중 컨테이너 이름 중복으로 인한 컨테이너 생성 실패
</details>

<details>
<summary><h1>보너스 과제</h1></summary>
  
## 1. Docker Compose 기초
- [ ] Compose 파일의 기본 구조 이해 및 단일 서비스 실행
- [ ] 실행 설정을 파일로 관리하는 이유 정리 및 실행 결과 기록

## 2. Docker Compose 멀티 컨테이너
- [ ] 웹 서버·보조 서비스 등 2개 이상 동시 실행
- [ ] 컨테이너 간 통신 확인 및 명령어·결과 기록

## 3. Docker Compose 운영
- [ ] up·down·ps·logs를 통한 실행·종료·상태·로그 관리 및 결과 기록

## 4. 환경변수 활용
- [ ] Dockerfile 또는 Compose에서 환경변수를 주입하여 서버 포트·모드 중 하나 변경
- [ ] 변경 전후 동작 비교 및 설정과 코드의 분리 의미 정리

## 5. GitHub SSH 키 설정
- [ ] SSH 키 생성·등록 및 HTTPS와 SSH 인증 방식 차이 이해
- [ ] SSH 방식의 원격 저장소 설정·푸시 성공 확인 및 결과 기록
- [Evidence](Evidence/00-INDEX/Evidence.md) 
</details>
