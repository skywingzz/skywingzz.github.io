---
title: Feign Client Config 중복 사용
date: 2019-08-06 18:40:00 +0300
categories: [JAVA, Feign]
tags: [Java, Spring-boot, Feign]
---

## 목적
Feign Client 를 동일 Config 를 사용하고 서비스별로 Class 를 분리하고자 한다.

## 오류
아래와 같이 동일 name 으로 지정 했더니 오류 발생
```
@FeignClient(name = "feignClient", contextId = "feignClientForMall", fallbackFactory = FeignClientForMallFallbackFactory.class)
```

Spring boot 2.1 부터 bean name 중복 등록시 아래와 같이 오류 발생
```
The bean ‘dp.FeignClientSpecification’, defined in null, could not be registered. A bean with that name has already been defined in null and overriding is disabled.
```

이 오류를 무시 하도록 하여 설정 할 수 있다.
```
spring.main.allow-bean-definition-overriding=true
```

하지만 뭔가 찜찜하여 FeignClient Spec 을 찾아 봤다.

## 해결
Feign Client Config 에 ContextId 가 중복되지 않게 명시적으로 지정한다.
```
@FeignClient(name = "feignClient", contextId = "feignClientForMall", fallbackFactory = FeignClientForMallFallbackFactory.class)
```
