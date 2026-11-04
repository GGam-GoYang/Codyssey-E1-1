# Docker 볼륨 영속성 검증 로그

## 1. 실습 구성

이름 있는 볼륨에 데이터를 저장하고, 기존 컨테이너를 삭제한 뒤 새 컨테이너에서 같은 데이터를 읽을 수 있는지 확인함.

| 항목 | 설정 |
| --- | --- |
| 볼륨 이름 | `e1-data` |
| 이미지 | `ubuntu:24.04` |
| 최초 컨테이너 | `e1-volume-a` |
| 재검증 컨테이너 | `e1-volume-b` |
| 컨테이너 내부 연결 경로 | `/data` |
| 저장 파일 | `/data/note.txt` |
| 저장 내용 | `Volume persistence test` |

<details>
<summary><h2>2. 초기 볼륨 목록 확인 및 생성</h2></summary>

초기 볼륨 목록을 확인함.

```text
~/Codyssey/Projects/E1-1/Docker$ sudo docker volume ls
DRIVER    VOLUME NAME
```

볼륨 생성 명령 — 표기 정리:

```bash
sudo docker volume create e1-data
```

실제 출력:

```text
e1-data
```

### 확인한 내용

- 초기 목록에는 볼륨이 없었음.
- `e1-data`라는 이름의 볼륨을 생성함.

</details>

<details>
<summary><h2>3. 첫 번째 컨테이너에서 데이터 저장·확인</h2></summary>

실행 명령 — 표기 정리:

```bash
sudo docker run --name e1-volume-a -v e1-data:/data ubuntu:24.04 sh -c 'echo "Volume persistence test" > /data/note.txt; cat /data/note.txt'
```

실제 출력:

```text
Volume persistence test
```

### 명령어 의미

| 구문 | 의미 |
| --- | --- |
| `--name e1-volume-a` | 새 컨테이너 이름 지정 |
| `-v e1-data:/data` | 이름 있는 볼륨을 컨테이너의 `/data`에 연결 |
| `sh -c '…'` | 문자열 안의 명령을 셸에서 실행 |
| `echo "Volume persistence test" > /data/note.txt` | 볼륨에 파일 생성 및 내용 저장 |
| `cat /data/note.txt` | 저장한 파일 내용 출력 |

### 확인한 내용

- `e1-data`를 연결한 컨테이너에서 파일을 작성함.
- 같은 명령 안에서 파일을 읽어 저장된 내용을 확인함.
- 이후 `docker ps -a`에서 `e1-volume-a`의 ID가 `0cc83cfe31ef`, 상태가 `Exited (0)`인 것을 확인함.
- 메인 셸이 명령을 마치고 정상 종료했으므로 컨테이너도 종료됨.

</details>

<details>
<summary><h2>4. 첫 번째 컨테이너 삭제 및 볼륨 유지 확인</h2></summary>

```text
~/Codyssey/Projects/E1-1/Docker$ sudo docker rm e1-volume-a
e1-volume-a
~/Codyssey/Projects/E1-1/Docker$ sudo docker volume ls
DRIVER    VOLUME NAME
local     e1-data
```

### 확인한 내용

- `e1-volume-a` 컨테이너를 삭제함.
- 컨테이너 삭제 후에도 볼륨 목록에 `e1-data`가 남아 있음을 확인함.
- 볼륨 자체는 삭제하지 않음.

</details>

<details>
<summary><h2>5. 새 컨테이너에서 데이터 유지 확인</h2></summary>

```text
~/Codyssey/Projects/E1-1/Docker$ sudo docker run --name e1-volume-b -v e1-data:/data:ro ubuntu:24.04 cat /data/note.txt
Volume persistence test
```

### 확인한 내용

- 다른 이름의 새 컨테이너 `e1-volume-b`에 동일한 `e1-data` 볼륨을 연결함.
- `ro`를 지정하여 읽기 전용으로 연결함.
- 새 컨테이너에서는 파일을 다시 작성하지 않고 기존 파일을 읽기만 함.
- 첫 번째 컨테이너에서 저장한 문구가 그대로 출력됨.

</details>

## 6. 컨테이너 삭제 전후 비교

| 구분 | 컨테이너 | 연결한 볼륨 | 파일 확인 결과 |
| --- | --- | --- | --- |
| 삭제 전 | `e1-volume-a` | `e1-data` | `Volume persistence test` |
| 기존 컨테이너 삭제 후 | `e1-volume-b` | `e1-data` | `Volume persistence test` |

### 결론

- 이름 있는 볼륨에 저장한 데이터는 해당 컨테이너를 삭제해도 유지됨.
- 새 컨테이너에 같은 볼륨을 연결하여 기존 데이터를 읽을 수 있음을 확인함.
- 이번 검증은 컨테이너 삭제 전후의 데이터 유지에 관한 것이며, 볼륨 자체를 삭제한 경우를 의미하지 않음.
