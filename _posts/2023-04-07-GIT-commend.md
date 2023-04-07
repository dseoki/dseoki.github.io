---
title: GIT 명령어
summary: GIT 명령어들을 알아보자
categories: [dev]
tags: [GIT]
date: 2023-04-07
comments: true
---

### 현재 상태, 로컬에서 수정 및 새로 추가된 파일들을 확인할 수 있다
```linux
git status
```

### 브랜치 확인
```linux
git branch # 로컬에 대한 브랜치 확인
git branch -a # 리모트 포함 모든 브랜치 확인
```

### stage 추가, commit 하려는 파일을 선택한다.
```linux
git add
git add {파일명}
```

### 커밋 생성
```linux
git commit
git commit -m "메세지 입력"
```

### 변경사항 리모트서버 업로드
```linux
git push origin master
```