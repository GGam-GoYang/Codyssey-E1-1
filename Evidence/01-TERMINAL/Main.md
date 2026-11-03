# Terminal-Main

## 개요 : 본 문서는 터미널 사용 중 학습한 개념들을 아래와 같이 정리한 문서임.

### 폴더 생성 (MaKe DIRectory)
- 명령어 : mkdir <인자> <폴더명>
- 실행 결과: 해당 shell의 있는 폴더를 기준으로 입력한 폴더명을 가진 하위 폴더 생성
```text
me@vm_ubuntu~/Codyssey/Projects$ mkdir TESTDIR

me@vm_ubuntu:~$ mkdir -p Codyssey/Projects/E1-1
```

### 리스트 출력 (LiSt)
- 명령어 : ls
- 실행 결과: 해당 shell의 있는 폴더를 기준으로 폴더 내의 폴더∙파일 리스트를 출력
```text
me@vm_ubuntu~/Codyssey/Projects$ ls
E1-1  TESTDIR
```

### 경로 변경 (Change Directory)
- 명령어 : cd <절대경로, 상대경로 또는 ..>
- 실행 결과: 입력한 경로 또는 상위 폴더(.. 입력 시 )로 이동
```text
me@vm_ubuntu~/test$ cd ~/Codyssey/Projects/E1-1
me@vm_ubuntu~/Codyssey/Projects/E1-1$

me@vm_ubuntu~$ cd Codyssey/
me@vm_ubuntu~/Codyssey$

me@vm_ubuntu~/Codyssey/Projects/E1-1$ cd ..
me@vm_ubuntu~/Codyssey/Projects$
```
