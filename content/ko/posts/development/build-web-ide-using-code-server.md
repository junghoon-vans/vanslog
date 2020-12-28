---
title: "code-server를 이용해서 웹 IDE 구축하기"
date: 2020-12-03T04:34:19Z
series:
- 군대에서 코딩하기
categories:
- Development
tags:
- code-server
draft: false
description: >
  나만의 웹 IDE 만들기
---

서론
---

최근 웹으로 접속하는 개발환경인 `Web IDE`가 활발하게 활용되고 있다. 대표적으로 `Cloud9`, `Codeanywhere`, `구름IDE` 등이 있는데, 깃헙에서도 [Codespace](https://github.com/features/codespaces)라는 이름으로 비슷한 서비스를 준비중에 있다. 나는 VSCode를 웹에서 바로 사용 가능하다는 점이 마음에 들어 미리보기를 신청했으나 아쉽게도 당첨되지 않았다..

그래서 방법을 찾던 중 `code-server`이라는 오픈소스를 찾게 되었다. 해당 서비스는 VS Code를 웹 서버에서 동작할 수 있게 하는 프로젝트이다. `MIT 라이선스`로 제공되는 오픈 소스인 만큼 조건 없이 사용 가능하다는 것도 장점이다. 군대에서 제약없이 코딩하고자 하는 나에게 있어서 최고의 선택지였다.

개발 환경 구축
---

서론이 길었던 것 만큼 설치과정이 길거라고 생각한다면 그것은 큰 착각이다. 설치는 매우 간단하다. 다른 블로그 글을 참고해서 설치해보기도 했지만, 여타 방법보다는 [github repo](https://github.com/cdr/code-server)에 나와있는 설치법이 더 쉽고 빠르게 할 수 있다.

### 설치 및 실행
```bash
curl -fsSL https://code-server.dev/install.sh | sh
sudo systemctl enable --now code-server@$USER # 시스템 부팅 시 자동실행 등록
```

위 코드를 실행하면 `code-server`를 설치하고 시스템 부팅 시 자동실행이 가능하도록 등록하는 과정까지 마치게 된다. 동시에 `code-server`는 이미 동작 중에 있겠지만 바로 사용할 수는 없는데, 이는 기본적으로 `localhost(127.0.0.1)`의 접속만 허용하기 때문이다. 따라서 외부에서 접속 가능하게 하는 설정이 필요하다.

### 외부 접속 허용 및 비밀번호 설정
```bash
vi ~/.config/code-server/config.yaml
```

기본적으로 `code-server`의 설정 파일은 위 경로에 위치한 `config.yaml` 파일이다.

```yaml
bind-addr: 0.0.0.0:8080
auth: password
password: #설정할 비밀번호 입력
cert: false
```

- `bind-addr`: 해당 서비스 주소
- `auth`: 패스워드 설정 여부
- `password`: 패스워드 지정
- `cert`: 인증서 설정

> `bind-addr`값을 `127.0.0.1`에서 `0.0.0.0`으로 변경하는 것은 외부에서 접속하는 것도 허용함을 의미한다.

```bash
sudo systemctl restart code-server@$USER
```

모든 설정이 완료되었다면 `code-server`를 재실행해준다. 이제 브라우저에서 `ip-address:8080`으로 접속할 수 있게 되었다. 포트 번호를 넣지 않고 주소만으로 접속하고 싶다면 아래도 이어서 따라와주길 바란다.

### Nginx 구축

`Nginx`는 `Apache`와 같은 웹 서버 소프트웨어이다. 아파치가 이전에 많이 사용되었다면 최근 만들어지는 프로젝트의 다수는 Nginx로 구성하는 추세이다. 가벼우면서도 아차피보다 더 많은 프로세스를 감당할 수 있다는 장점도 있다.

이 Nginx는 `포워드 프록시`라는 기능을 제공하는데 이를 활용하면 서버 ip주소만으로 서비스 포트가 8080인 `code-server`를 접속할 수 있다.

#### bind-addr 초기화

```bash
vi ~/.config/code-server/config.yaml # bind-addr: 127.0.0.1:8080
sudo systemctl restart code-server@$USER
```

해당 과정을 위해서는 `code-server`의 `bind-addr`를 localhost로 다시 설정해주어야 한다.


#### Nginx 설치

```bash
sudo apt install -y nginx
```

#### `/etc/nginx/sites-available/code-server` 파일에 아래 설정 추가(sudo 권한)

```nginx
server {
    listen 80;
    listen [::]:80;
    server_name mydomain.com; # 도메인이 있다면 변경

    location / {
      proxy_pass http://localhost:8080/;
      proxy_set_header Host $host;
      proxy_set_header Upgrade $http_upgrade;
      proxy_set_header Connection upgrade;
      proxy_set_header Accept-Encoding gzip;
    }
}
```

> 80번 포트로 listen하고 해당 프록시를 localhost의 8080 포트로 pass하는 설정이다.

#### 설정 적용

```bash
sudo rm /etc/nginx/sites-enabled/default
sudo ln -s ../sites-available/code-server /etc/nginx/sites-enabled/code-server
sudo systemctl reload nginx.service
```

설정이 제대로 적용되었다면 이후부터는 ip주소만으로 브라우저에서 `code-server`을 이용할 수 있게 된다.

### 버전 업데이트

이 포스팅을 쓰는 도중에도 코드 서버는 `v3.7.3`에서 `v3.7.4`로 버전 업데이트가 이루어졌다. 몇 가지 버그 픽스와 기능 추가가 있는 작은 업데이트이었지만 `code-server`는 접속할 때마다 새로운 버전이 나왔음을 알려주었다. 버전을 업데이트하는 방법은 매우 간단하다.

```bash
curl -fsSL https://code-server.dev/install.sh | sh
```

설치할 때 사용한 코드를 동일하게 실행해주면 바로 적용된다. 이렇게 지속적으로 빠르게 업데이트가 되는 것은 이 프로젝트가 오픈 소스이기 때문에 얻을 수 있는 큰 장점이다. 이렇게 좋은 개발환경을 무료로 자유로이 사용할 수 있다는 것에 감사하고 있다.

추가적인 설정
---

이렇게 해서 개인 서버에 `code-server`를 설치해서 웹 브라우저를 통해 접속할 수 있는 환경을 만들어 보았다. 공식 문서에서는 추가적으로 `Nginx` 등을 활용하여 https 통신으로 암호화하는 것을 권장하는데, 자세한 내용은 [여기](https://github.com/cdr/code-server/blob/v3.7.4/doc/guide.md)를 통해 볼 수 있다. 해당 방법은 도메인네임이 있어야만 하므로 해당 비용을 감수하기 싫다면 굳이 진행하지 않아도 무관하다.