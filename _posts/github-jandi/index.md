---
title: Github 잔디가 적용되지 않을 때
date: 2022-05-23
tags:
- github
---

어제 분명히 커밋 & 푸시를 했는데.. 오늘 보니까 잔디에 적용이 안 돼 있다. ~~(개빡치네)~~

파일을 지우고 다시 커밋했는데도 적용이 안됨

(참고 : fork한 repository는 안되는게 맞다)   
.   
.   

찾아보니 유저이름과 이메일 설정이 뭔가 꼬인 것 같다.

### 계정 초기화
```bash
git config --global --unset user.name
git config --global --unset user.email
```

### 계정 재설정
```bash
git config user.name "userName"
git config user.email email@domain.com
```


출처 : https://rizni.tistory.com/207