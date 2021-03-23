---
title: base guide
date: 2021-03-23 11:57:22
tags: git
---

# 깃허브에 소스 업로드

```
git remote
```
현재 폴더의 원격 레퍼지토리 확인 명령어

```
git remote add 원격저장소이름 https://github.com/유저네임/레퍼지토리네임.git
```

깃허브 저장소의 주소로 원격저장소 생성

작업폴더와 깃허브 저장소가 연동됨.

```
git push -u origin master
```
push는 업로드 명령어

## 수정 후 업데이트

```
git add 파일명
git commit -m "msg"
git push
```
commit은 수정한 내용에 대한 코멘터리

## 소스 내려 받기

깃허브 원격 저장소의 Clone or download에서 주소를 복사

복사를 시킬 폴더 생성

IDE로 폴더 오픈 후, 터미널에서 클론 명령어

```
git clone https://github.com/유저네임/원격저장소이름.git
```

내려받아진 폴더로 터미널 이동

```
cd 폴더
```

깃 로그 명령어를 통해 이전 작업내용 확인가능
```
git log
```

이전 반영된 파일 받아오기

```
git pull 원격저장소명 브랜치명
```
