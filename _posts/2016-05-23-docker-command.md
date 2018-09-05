---
layout: post
title: "docker 기본 명령어"
date: 2016-05-23 11:35:00 +0300
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img:  
tags: [docker, shell, install]
---

### machine 리스트 확인
```shell
# docker-machine ls
```

### 시작 / 종료
```shell
# docker-maschine start default
# docker-maschine stop default
```

### 환경설정
```shell
# docker-machine env default
```

### 환경변수 설정
```shell
# eval "$(docker-machine env default)"
```

- 참고 : http://blog.saltfactory.net/docker/running-docker-on-mac-using-with-docker-machine.html
