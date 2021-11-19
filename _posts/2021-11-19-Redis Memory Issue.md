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
   - 서비스 날짜별 PV 를 저장하기 위해 Redis 를 사용함
   - 날짜별 누적 데이터라 15M 정도의 value Size 를 가지고 있음
   - Redis 전체 메모리 약 15G
   - TTL 없음
### 원인
   - Item 에 Save 발생시 Master, Slave 싱크를 위해 메모리를 복재 하여 사용함
   - 이 복재 과정에서 또다시 Save 발생시 메모리 과사용
### 해결

