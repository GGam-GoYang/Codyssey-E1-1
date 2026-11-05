# 트러블슈팅

## 사례 1
### index.html 브라우저 접속 시 한글 출력 깨짐

#### 문제

포트 매핑 및 SSH 터널로 웹페이지에 접속했으나 브라우저에서 한글이 깨져 표시됨.

#### 원인 가설 및 확인

HTML에 문자 인코딩 선언이 없어 브라우저가 문자를 올바르게 해석하지 못했을 가능성을 의심함.
확인 결과, index.html에 UTF-8 선언이 없었음.
```text
$ sudo docker exec e1-web-8080 cat /usr/share/nginx/html/index.html
```
```html
<title>Codyssey E1-1 Mission Page</title>
<body>
<h1>안녕하세요.</h1>
<h2>Dockerfile로 만든 이미지를 통해 제공하는 페이지입니다!</h2>
</body>
```

#### 해결

VM에서 `nano site/index.html`로 아래와 같이 수정함.
- `<head>` 태그 추가.
  - `<meta charset="UTF-8">` 추가.

```html
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>Codyssey E1-1 Mission Page</title>
</head>
<body>
  <h1>안녕하세요.</h1>
  <h2>Dockerfile로 만든 이미지를 통해 제공하는 페이지입니다!</h2>
</body>
</html>
```

기존 두 컨테이너(`e1-web-8080`, `e1-web-8081`)를 중지·삭제한 후, `e1-web:1.1` 이미지를 빌드하고 컨테이너를 재생성함.

```text
$ sudo docker stop e1-web-8080 e1-web-8081
e1-web-8080
e1-web-8081

$ sudo docker rm e1-web-8080 e1-web-8081
e1-web-8080
e1-web-8081

$ sudo docker build -t e1-web:1.1 .
Step 2/2 : COPY site/ /usr/share/nginx/html/
 ---> Using cache
 ---> 9466840bb879
Successfully built 9466840bb879
Successfully tagged e1-web:1.1

$ sudo docker run -d --name e1-web-8080 -p 127.0.0.1:8080:80 e1-web:1.1
e17075132e8ee2301f17f735ddc46fa7a606574b81613c0ec4a3d09c11411855

$ sudo docker run -d --name e1-web-8081 -p 127.0.0.1:8081:80 e1-web:1.1
f3e82867d8e3ccf1e78f7fbc619394abc05c1eca56c02d26ae58e91ffb01438b
```

#### 재검증 및 결과

- `cat site/index.html` 출력에서 수정 사항이 적용된 것을 확인함.
- 수정 후 브라우저에서 접속 시 한글이 정상 출력되는 것을 확인함.

#### 재발 방지 및 배운 점

- HTML 코드 작성 시 문자 인코딩 선언을 포함할 것.
- 호스트 파일 수정만으로는 Dockerfile의 `COPY`로 이미지를 빌드할 때 복사한 파일에는 반영되지 않음.

## 사례 2
### 사례2 내용
