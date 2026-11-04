# Codyssey-E1-1
Codyssey E1-1 Personal Mission

## 본 문서는 코디세이 올인원 1기 입학 연수 1주차 미션을 위해 작성하는 기술 문서임.
### 개요 : 실행, 배포, 디버깅 등의 개발 과정을 내 컴퓨터 이외의 다른 환경에서도<br>범용적으로 진행할 수 있는 환경 구성이 목표.


## 실행 환경
| 항목 | 실제 값 |
| --- | --- |
| OS | ARM Ubuntu 24.04.4 LTS |
| 셸 | Bash |
| 터미널 | Putty 0.85 |
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

# 체크리스트
## 1. GitHub 레포지토리 생성
- [x] 레포지토리를 Public으로 생성
- [x] README.md 파일 생성 및 과제 규칙에 맞는 폴더 구조 생성

## 2. README.md
- [x] 프로젝트 개요와 미션 목표 요약
- [x] 실행 환경 작성 : OS, 셸, 터미널, Docker 버전, Git 버전
- [x] 수행 항목 체크리스트 작성 : 터미널, 권한, Docker, DockerFile, 포트, 볼륨, Git, GitHub
- [ ] 검증 방법 기록
- [ ] 검증 결과 파일, 링크를 통해 본문에 연결
- [ ] 트러블슈팅 2건 이상 기록
- [ ] 기술 문서 만으로 전체 수행 내용 파악 가능한지 확인

## 3. 터미널 조작
### [주요 개념](Evidence/01-TERMINAL/Main.md#터미널-조작) 
- [x] 현재 위치, 숨김 파일을 포함한 목록 확인 및 디렉토리 이동
- [x] 파일 및 폴더의 생성, 복사, 이동, 이름 변경, 삭제
- [x] 절대 경로, 상대 경로의 이해와 핵심 명령어 정리 및 출력 기록

## 4. 권한 실습
### [주요 개념](Evidence/01-TERMINAL/Main.md#권한-실습) 
- [x] r/w/x 권한 구성 및 숫자 표기 이해
- [x] 1개 이상의 파일·디렉토리 권한 확인·변경과 전후 비교 기록 

## 5. Docker 설치 및 운영
### [주요 개념](Evidence/02-DOCKER/Main.md) 
- [x] Docker 설치 및 버전·데몬 동작 확인
- [x] 이미지 다운로드와 목록 확인 및 컨테이너 실행·중지·목록 확인
- [x] hello-world 실행 및 Ubuntu 내부 명령 수행
- [x] 컨테이너 종료/유지 차이 관찰
- [x] 컨테이너 로그·리소스 확인 및 명령어·출력 기록

## 6. DockerFile 사용
### [주요 개념](Evidence/02-DOCKER/Main.md) 
- [ ] 베이스 선택 및 커스텀 내용·목적 설명
- [ ] 웹 소스·DockerFile 작성 및 저장소 업로드
- [ ] 이미지 빌드·컨테이너 실행 후 명령어·결과 기록

## 7. 포트 매핑 및 접속 테스트
### [주요 개념](Evidence/02-DOCKER/Main.md) 
- [ ] 포트 매핑 후 서로 다른 포트로 2회 접속
- [ ] 실행 명령어와 실제 화면 캡처 및 기록

## 8. 바인드 마운트
### [주요 개념](Evidence/02-DOCKER/Main.md) 
- [ ] 호스트·컨테이너 경로 연결 및 바인드 마운트 진행
- [ ] 호스트 파일 변경 반영 확인 및 명령어·변경 전후 비교 기록

## 9. 볼륨 영속성
### [주요 개념](Evidence/02-DOCKER/Main.md) 
- [ ] 볼륨 생성·연결 및 데이터 저장
- [ ] 기존 컨테이너 삭제 후 새 컨테이너에 동일 볼륨 연결
- [ ] 데이터 유지 확인 및 명령어·삭제 전후 비교 기록

## 10. Git 설정 ∙ VSCode GitHub 연동
### [주요 개념](Evidence/03-GIT/Main.md) 
- [ ] Git·GitHub 역할 이해 및 사용자 정보·기본 브랜치 설정, git config --list 기록
- [ ] VSCode GitHub 로그인·저장소 연동 및 증거 업로드

# 보너스 과제
## 1. Docker Compose 기초
- [ ] Compose 파일의 기본 구조 이해 및 단일 서비스 실행
- [ ] 실행 설정을 파일로 관리하는 이유 정리 및 실행 결과 기록

## 2. Docker Compose 멀티 컨테이너
- [ ] 웹 서버·보조 서비스 등 2개 이상 동시 실행
- [ ] 컨테이너 간 통신 확인 및 명령어·결과 기록

## 3. Docker Compose 운영
- [ ] up·down·ps·logs를 통한 실행·종료·상태·로그 관리 및 결과 기록

## 4. 환경변수 활용
- [ ] DockerFile 또는 Compose에서 환경변수를 주입하여 서버 포트·모드 중 하나 변경
- [ ] 변경 전후 동작 비교 및 설정과 코드의 분리 의미 정리

## 5. GitHub SSH 키 설정
- [ ] SSH 키 생성·등록 및 HTTPS와 SSH 인증 방식 차이 이해
- [ ] SSH 방식의 원격 저장소 설정·푸시 성공 확인 및 결과 기록
- [Evidence](Evidence/00-INDEX/Evidence.md) 
