---
title: Hive Query Options
date: 2018-09-18 14:58:00 +0300
categories: [Big Data]
tags: [Hive, HDFS, Query]
---

## Hive Current Time
```sql
select from_unixtime(unix_timestamp(current_timestamp,'yyyyMMddHH'), 'yyyyMMddHH')
from dual
```

## hive.exec.dynamic.partition.mode
- strict (default) : 사용자가 실수로 모든 파티션을 덮어쓸 경우에 하나 이상의 정적 파티션을 지정해야 한다.
- nonstrict : 비제한 모드에서는 모든 파티션이 동적일 수 있다.

## hive.exec.max.dynamic.partitions
- Default : 1000
- 생성될 최대 dynamic 파티션 수, 지정해 주지 않으면 default 값이 설정 되고 이를 넘는 파티션이 생성되면 에러가 발생한다.

## hive.exec.max.dynamic.partitions.pernode 
