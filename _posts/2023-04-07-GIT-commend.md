---
title: GIT 명령어
summary: GIT 명령어들을 알아보자
categories: [dev]
tags: [GIT]
date: 2023-04-07
comments: true
---



### 현재 GIT 사용자 확인

```bash	
git config --global user.name #사용자명
git config --global user.email #사용자이메일
git config -l #사용자 정보 확인

git config --global user.name = 가웨인 #사용자명 설정
```



### GIT 초기화

```ba
git init
```



### repository 연결

```bash
git remote add origin [git repository 주소]
```



### 메인 브랜치 이름 변경

```bash
git branch -M main
```



### 브랜치 확인
```bash
git branch # 로컬에 대한 브랜치 확인
git branch -a # 리모트 포함 모든 브랜치 확인
```



### 현재 상태, 로컬에서 수정 및 새로 추가된 파일들을 확인할 수 있다

```bash
git status
```



### stage 추가, commit 하려는 파일을 선택한다.

```bash
git add
git add {파일명}
```



### 커밋 생성

```bash
git commit
git commit -m "메세지 입력"
```



### 변경사항 리모트서버 업로드

```bash
git push origin main
```