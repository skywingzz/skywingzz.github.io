---
title: Redis Memory Issue
date: 2021-11-1 15:50:00 +0300
categories: [REDIS]
tags: [REDIS]
---

## Redis Memory 이슈 해결
### 현상
- Redis 운영중 Redis Memory Leak 발생하여 WAS 적채 발생

### Redis 사용 상황
- 페이지 & 날짜별 PV 를 저장하기 위해 Redis 를 사용함
- 날짜별 누적 데이터라 15M 정도의 value Size 를 가지고 있음
- Redis 전체 메모리 약 35G
- TTL 없음

### 원인
- 값을 저장하기 위한 Save 시 master, slave Sync 을 위한 rdb 파일 생성
- save 도중 save 작업이 또 발생시 메모리를 추가로 사용하게 됨
- 참고 : https://mozi.tistory.com/523

### 해결
- 페이지 + 날짜 단위로 Key 분리
- 유지할 날짜 만큼 TTL 설정
- incrby 로 카운트 저장되도록 수정

### 결론
- 전체 Key 는 증가하였지만 TTL 을 주어 사용량에는 큰 변화는 없음
- 키를 분리하여 영향도를 최조화함
- Top PV 를 추출 하는 로직이 있었으나 scan 으로 해결
