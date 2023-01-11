---
title: Git 블로그 deploy 권한 오류
date: 2022-02-07
tags:
- github
keywords:
- Git블로그
- GitBlog
---

오랜만에 git 블로그 포스팅을 좀 하려고 clone한 뒤

npm run deploy명령어를 때렸는데 전에 안뜨던 아이디/패스워드를 입력하라고 나왔다.

그래서 입력했는데 오류가.. 

오류 내용

```bash
Username for 'https://github.com': chaa822
Password for 'https://chaa822@github.com': 
remote: Support for password authentication was removed on August 13, 2021.
remote: Please see https://docs.github.com/en/get-started/getting-started-with-git/about-remote-repositories#cloning-with-https-urls for information on currently recommended modes of authentication.
fatal: Authentication failed for 'https://github.com/chaa822/chaa822.github.io.git/'

npm ERR! code ELIFECYCLE
npm ERR! errno 1
npm ERR! borderless@3.0.0 deploy: `gatsby clean && gatsby build && gh-pages -b master -d public`
npm ERR! Exit status 1
npm ERR! 
npm ERR! Failed at the borderless@3.0.0 deploy script.
npm ERR! This is probably not a problem with npm. There is likely additional logging output above.
```

아이디 패스워드 방식이 아닌 토큰 방식으로 변경 됐다고 한다.

[여기](https://wotres.tistory.com/entry/Github-%EC%97%90%EB%9F%AC-%ED%95%B4%EA%B2%B0%EB%B2%95-Authentication-failed-for-use-a-personal-access-token-instead)에 나온 방식대로 토큰을 생성한 뒤, 유저아이디/생성한 토큰을 입력하면 정상적으로 배포된다.

<!--
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
-->