---
title: 드래그 방지 해제
date: 2022-02-01
tags:
- disable-drag
keywords:
- 드래그 방지 해제
---

아래 코드를 개발자 도구에서 실행하거나, 즐겨찾기 url에 붙여넣기한 후 실행한다. 

```javascript
function naver(q){
    void(z=q.body.appendChild(q.createElement('script')));
    void(z.language='javascript');
    void(z.type='text/javascript');
    void(z.src='http://userscripts.org/scripts/source/61326.user.js');
}

function selfw(w) {
    try {
        naver(w.document);
    } catch(e){}

    for (var i = 0; i < w.frames.length; i++) {
        try {
            selfw(w.frames[i]);
        } catch(e){}
    }
}
selfw(self);

(function() {
    var e, i, all;

    document.onselectstart = null;
    document.oncontextmenu = null;

    all = document.getElementsByTagName("*");

    for(i = 0; i < all.length; i += 1) {
        e = all[i];
        e.onselectstart = null;
        e.oncontextmenu = null;
    }
})();
```
