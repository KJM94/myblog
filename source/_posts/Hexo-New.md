---
title: Hexo New
date: 2024-02-06 10:08:16
tags: Git
categories:
    - Git
---
# Hexo Blog 배포

## 블로그 구성에 필요한 파일 설치

- node.js 다운로드 : https://nodejs.org/en/
```
$ node -v
```
- git-scm 다운로드 : https://git-scm.com/
```
$ git --version
```
- hexo 설치 : npm 다운로드
```
$ npm install -g hexo-cli
```

## Github 설정

- 2개의 깃허브 저장소 생성
  - 포스트 버전관리
  - 포스트 배포용 관리

- 경로는 본인이 편한 곳으로
```
$ git clone 저장소 주소.git
```

## 블로그 만들기

- 임의의 경로 설정 후 폴더 생성
```
$ mkdir Blog
$ cd Blog
```
- 파일명을 만듦
```
$ hexo init blog
$ cd blog
$ npm install
$ npm install hexo-server --save
$ npm install hexo-deployer-git --save
```

- ERROR Deployer not found: git
- hexo-deployer-git을 설치 하지 않으면 deploy시 위와 같은 ERROR가 발생합니다.
- _config.yml 파일 설정

```
title: 제목을 지어주세요
subtitle: 부제목을 지어주세요
description: description을 지어주세요
author: YourName
```
- 블로그 URL 설정
```
url: https://rain0430.github.io
root: /
permalink: :year/:month/:day/:title/
permalink_defaults:
```
- 깃허브 연동
```
# Deployment
deploy:
  type: git
  repo: https://github.com/rain0430/rain0430.github.io.git
  branch: main
```

## 깃허브 배포

- 로컬테스트 동작 확인
```
$ hexo generate
$ hexo server
INFO  Start processing
INFO  Hexo is running at http://localhost:4000 . Press Ctrl+C to stop.
```

- gitignore 파일 설정
```
.DS_Store
Thumbs.db
db.json
*.log
node_modules/
public/
.deploy*/
```

- 배포
```
hexo g -d
```

기본테마가 마음에 들지 않는다면 Hexo에서 지원하는 여러 테마들이 존재하므로<br>
원하는 테마를 적용시켜 보자.

https://hexo.io/themes/