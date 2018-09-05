---
layout: post
title: "requestIdleCallback"
date: 2016-05-23 11:35:00 +0300
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img:  
tags: [docker, shell, install]
---

스크립트에서 비동기로 백그라운드에서 처리해야 하는 작업들이 많이 늘어나고 있다.
이런 작업들 중에서 서버쪽으로 통계정보와 같은 데이터를 보낸다거나 하는 작업들은
브라우저가 놀고 있는 시간에 보내면 좋은데 이렇때 사용하는 유용한 함수가 있다.

requestIdleCallback 

아쉽게도 현재 브라우저에서는 지원을 하지 않고 Chrome Canary(46+) 부터 옵션을 켰을 때 작동을 한다.
Chrome Canary 다운로드 : https://www.google.co.kr/chrome/browser/canary.html

- 참고 사이트
https://developers.google.com/web/updates/2015/08/27/using-requestidlecallback
https://w3c.github.io/requestidlecallback/
