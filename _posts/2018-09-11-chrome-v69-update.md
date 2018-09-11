---
title: Heap memory dump
date: 2012-12-05 15:50:00 +0300
categories: [JAVA]
tags: [Java, Heap Dump]
---

### pid 확인
```shell
ps –ef | grep java
```

### heap 상태 확인
```shell
jmap –heap [pid]
```

### 클래스별 객체수와 메모리 사용량 확인
```shell
jmap -histo [pid] | more
```

### Heap dump 파일 생성
```shell
jmap –heap:format=b [pid]
```

**생성된 덤프 파일을 MAT 등을 통하여 분석**
