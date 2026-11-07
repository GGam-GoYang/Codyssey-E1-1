[← README로 돌아가기](../../README.md) | [주요 개념](Main.md)

# GitHub SSH 실습 기록

개인 Windows 노트북의 PowerShell에서 진행함. 실행 기록을 발췌하고 사용자명은 마스킹함.

## 1. SSH 키 생성

Ed25519 키를 생성함.

```bash
PS D:\Codyssey\Projects\E1-1\Codyssey-E1-1> ssh-keygen -t ed25519 -C "Codyssey-GitHub" 
Generating public/private ed25519 key pair.
Enter file in which to save the key (C:\Users\<사용자명>/.ssh/id_ed25519): 
Created directory 'C:\\Users\\<사용자명>/.ssh'.
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in C:\Users\<사용자명>/.ssh/id_ed25519
Your public key has been saved in C:\Users\<사용자명>/.ssh/id_ed25519.pub
The key fingerprint is:
SHA256::**************** Codyssey-GitHub
The key's randomart image is:***********
```

## 2. 공개키 복사 및 GitHub 등록

공개키를 복사하여 GitHub의 SSH 키 설정에 등록함. 웹 등록 화면은 별도로 캡처하지 않음.

```bash
PS D:\Codyssey\Projects\E1-1\Codyssey-E1-1> Get-Content "$HOME\.ssh\id_ed25519.pub" | Set-Clipboard
```

## 3. SSH 인증 확인

`ssh -T`로 GitHub 계정 인증 성공을 확인함.

```bash
PS D:\Codyssey\Projects\E1-1\Codyssey-E1-1> ssh -T git@github.com
The authenticity of host 'github.com (20.200.245.247)' can't be established.
ED25519 key fingerprint is SHA256:****************
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'github.com' (ED25519) to the list of known hosts.
Enter passphrase for key 'C:\Users\<사용자명>/.ssh/id_ed25519': 
Enter passphrase for key 'C:\Users\<사용자명>/.ssh/id_ed25519': 
Hi GGam-GoYang! You've successfully authenticated, but GitHub does not provide shell access.
```

## 4. 원격 저장소 주소 변경

`origin`을 HTTPS에서 SSH 주소로 변경하고 현재 브랜치 상태를 확인함.

```bash
PS D:\Codyssey\Projects\E1-1\Codyssey-E1-1> git remote set-url origin git@github.com:GGam-GoYang/Codyssey-E1-1.git

PS D:\Codyssey\Projects\E1-1\Codyssey-E1-1> git remote -v
origin  git@github.com:GGam-GoYang/Codyssey-E1-1.git (fetch)
origin  git@github.com:GGam-GoYang/Codyssey-E1-1.git (push)

PS D:\Codyssey\Projects\E1-1\Codyssey-E1-1> git status -sb
## main...origin/main
```

## 5. PowerShell에서 SSH 푸시 확인

PowerShell에서 SSH 주소로 설정된 원격 저장소에 푸시 명령을 실행함.

```bash
PS D:\Codyssey\Projects\E1-1\Codyssey-E1-1> git push origin HEAD
The host key is not cached for this server:
  github.com (port 22)
You have no guarantee that the server is the computer you
think it is.
The server's ssh-ed25519 key fingerprint is:
  ssh-ed25519 255 SHA256:+DiY3wvvV6TuJJhbpZisF/zLDA0zPMSvHdkr4UvCOqU
If you trust this host, enter "y" to add the key to Plink's
cache and carry on connecting.
If you want to carry on connecting just once, without adding
the key to the cache, enter "n".
If you do not trust this host, press Return to abandon the
connection.
Store key in cache? (y/n, Return cancels connection, i for more info) y
Everything up-to-date

PS D:\Codyssey\Projects\E1-1\Codyssey-E1-1>
```

`Everything up-to-date`로 푸시 명령이 성공했으며 새로 전송할 커밋은 없었음.

SSH 키 생성·등록과 인증 방식은 [주요 개념](Main.md#5-github-ssh-키-설정)을 참고.
