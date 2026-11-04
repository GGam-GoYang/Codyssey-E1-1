# 파일·디렉토리 권한 변경 로그

## 1. 파일 권한 변경 전 확인

```text
~/Codyssey/Projects/E1-1/Terminal$ ls -l original/note.txt
-rw-rw-r-- 1 student student 12 Nov  4 12:40 original/note.txt
```

## 2. 파일 권한을 640으로 변경하고 확인

```text
~/Codyssey/Projects/E1-1/Terminal$ chmod 640 original/note.txt
~/Codyssey/Projects/E1-1/Terminal$ ls -l original/note.txt
-rw-r----- 1 student student 12 Nov  4 12:40 original/note.txt
```

## 3. 파일 권한을 600으로 변경하고 확인

```text
~/Codyssey/Projects/E1-1/Terminal$ chmod 600 original/note.txt
~/Codyssey/Projects/E1-1/Terminal$ ls -l original/note.txt
-rw------- 1 student student 12 Nov  4 12:40 original/note.txt
```

## 4. 디렉토리 생성 및 변경 전 권한 확인

```text
~/Codyssey/Projects/E1-1/Terminal$ mkdir permission-dir
~/Codyssey/Projects/E1-1/Terminal$ ls -ld permission-dir/
drwxrwxr-x 2 student student 4096 Nov  4 12:51 permission-dir/
```

## 5. 디렉토리 권한을 750으로 변경하고 확인

```text
~/Codyssey/Projects/E1-1/Terminal$ chmod 750 permission-dir/
~/Codyssey/Projects/E1-1/Terminal$ ls -ld permission-dir/
drwxr-x--- 2 student student 4096 Nov  4 12:51 permission-dir/
```

## 6. 디렉토리 권한을 700으로 변경하고 확인

```text
~/Codyssey/Projects/E1-1/Terminal$ chmod 700 permission-dir/
~/Codyssey/Projects/E1-1/Terminal$ ls -ld permission-dir/
drwx------ 2 student student 4096 Nov  4 12:51 permission-dir/
```

## 7. 첨부 출력 기준 변경 전후 요약
### 권한 구조 해석의 경우 [Main.md](Main.md#권한-실습) 를 참고할 것.
| 대상 | 변경 전 | 첫 번째 변경 | 두 번째 변경 |
| --- | --- | --- | --- |
| 파일 `original/note.txt` | `-rw-rw-r--` (664) | `-rw-r-----` (640) | `-rw-------` (600) |
| 디렉토리 `permission-dir/` | `drwxrwxr-x` (775) | `drwxr-x---` (750) | `drwx------` (700) |
