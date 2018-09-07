---
title: "Mobile Web에서 APP 설치 여부 판단"
date: 2014-12-04 13:58:00 +0300
categories: [Mobile]
tags: [app, intent, scheme]
---

우리가 일반적으로 사용하는 빈 iframe 에 app url을 날려봐서 요청이 없으면 timeout 으로 store url로 보내는 방식은
chrome version 25이상 부터는 정상 작동하지 않아 사용할 수 없다.

그래서 보통은 intent Scheme 을 사용하는데
여기에 통계를 위해 referrer를 붙였을 경우 무조건 store 로 이동하게 작동을 한다.
버그인지 이렇게 작동하게 끔 만들어 놓은건지...
Google 검색을 해봐도 딱히 해결책이 보이지 않는다.

```
Intent Scheme 에 referrer 를 붙이는 방식은 무조건 play store 로 이동
intent://some_data_sent_to_app#Intent;scheme=app_scheme;package=package_name&referrer=referrer_string;end
```

어떤분은 새창으로 링크 이동을 시켜 그 새창에서 여러 처리를 하는 방식을 쓰시는 분도 있던데.
방법이 지저분 하다...
좀 더 깔끔한 방법을 찾아 보거나. 아니면 Google에서 Intent Scheme 에 referrer를 붙였을 때도
App 과 Store를 정상적으로 이동하도록 수정되기를 기다리는 수밖에 없을 듯 하다.
 
-----------------------------------------------------------------------
삽질을 했네.............
역시 google 은 멍청하지 않았어.
아래와 같이 market 이동 scheme 에 파라미터를 붙여주면
아라서 app or market 이동을 해주고 있네..
거기에 url 로 자체 scheme 을 넘겨줄 경우 설치 후 마켓의 머튼이 자동으로 계속으로 변경된다.
역시.. Google..

market://details?id=[package_name]&url=[내부스킴]&referrer=[referrer String]