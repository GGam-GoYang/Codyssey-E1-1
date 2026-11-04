# 터미널 조작 로그

## 1. 현재 위치(절대 경로)와 목록 확인

```text
~$ cd Codyssey/Projects/E1-1/
~/Codyssey/Projects/E1-1$ pwd
/home/student/Codyssey/Projects/E1-1
~/Codyssey/Projects/E1-1$ ls -la
total 8
drwxrwxr-x 2 student student 4096 Nov  4 12:38 .
drwxrwxr-x 3 student student 4096 Nov  4 09:07 ..
```

## 2. 디렉토리·파일 생성

```text
~/Codyssey/Projects/E1-1$ mkdir Terminal
~/Codyssey/Projects/E1-1$ cd Terminal/

~/Codyssey/Projects/E1-1/Terminal$ mkdir original copied
~/Codyssey/Projects/E1-1/Terminal$ printf 'My Terminal\n' > original/note.txt
~/Codyssey/Projects/E1-1/Terminal$ touch .hidden-file
```

## 3. 숨김 파일을 포함한 목록 확인

```text
~/Codyssey/Projects/E1-1/Terminal$ ls
copied  original

~/Codyssey/Projects/E1-1/Terminal$ ls -la
total 16
drwxrwxr-x 4 student student 4096 Nov  4 12:40 .
drwxrwxr-x 3 student student 4096 Nov  4 12:39 ..
-rw-rw-r-- 1 student student    0 Nov  4 12:40 .hidden-file
drwxrwxr-x 2 student student 4096 Nov  4 12:39 copied
drwxrwxr-x 2 student student 4096 Nov  4 12:40 original
```

## 4. 파일 복사와 내용 확인

```text
~/Codyssey/Projects/E1-1/Terminal$ cp original/note.txt copied/note-copy.txt
~/Codyssey/Projects/E1-1/Terminal$ cat copied/note-copy.txt
My Terminal
```

## 5. 파일 이름 변경

```text
~/Codyssey/Projects/E1-1/Terminal$ mv copied/note-copy.txt copied/renamed.txt
~/Codyssey/Projects/E1-1/Terminal$ ls -l copied
total 4
-rw-rw-r-- 1 student student 12 Nov  4 12:40 renamed.txt
```

## 6. 파일 이동

```text
~/Codyssey/Projects/E1-1/Terminal$ mv copied/renamed.txt original/
~/Codyssey/Projects/E1-1/Terminal$ ls -l original copied
copied:
total 0

original:
total 8
-rw-rw-r-- 1 student student 12 Nov  4 12:40 note.txt
-rw-rw-r-- 1 student student 12 Nov  4 12:40 renamed.txt
```

## 7. 상대 경로를 이용한 디렉토리 이동

```text
~/Codyssey/Projects/E1-1/Terminal$ cd original/
~/Codyssey/Projects/E1-1/Terminal/original$ pwd
/home/student/Codyssey/Projects/E1-1/Terminal/original
~/Codyssey/Projects/E1-1/Terminal/original$  cd ..
~/Codyssey/Projects/E1-1/Terminal$ pwd
/home/student/Codyssey/Projects/E1-1/Terminal
```

## 8. 절대 경로를 이용한 디렉토리 이동

```text
~/Codyssey/Projects/E1-1/Terminal$ cd /tmp
/tmp$ pwd
/tmp
/tmp$ cd ~/Codyssey/Projects/E1-1/Terminal/
~/Codyssey/Projects/E1-1/Terminal$ pwd
/home/student/Codyssey/Projects/E1-1/Terminal
```

## 9. 파일 및 디렉토리 삭제와 결과 확인

```text
~/Codyssey/Projects/E1-1/Terminal$ rm original/renamed.txt
~/Codyssey/Projects/E1-1/Terminal$ rmdir copied
~/Codyssey/Projects/E1-1/Terminal$ ls -la
total 12
drwxrwxr-x 3 student student 4096 Nov  4 12:45 .
drwxrwxr-x 3 student student 4096 Nov  4 12:39 ..
-rw-rw-r-- 1 student student    0 Nov  4 12:40 .hidden-file
drwxrwxr-x 2 student student 4096 Nov  4 12:45 original
~/Codyssey/Projects/E1-1/Terminal$ ls -l original/
total 4
-rw-rw-r-- 1 student student 12 Nov  4 12:40 note.txt
```
