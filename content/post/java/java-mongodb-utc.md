---
title: "UTC, 데이터를 불러올 때 어떻게 하면 좋을까? (feat. Java & MongoDB)"
date: 2022-09-24T17:05:00+09:00
tags: ['java', 'mongodb']
draft: false
---
UTC, 데이터를 불러올 때 어떻게 하면 좋을까? (feat. Java & MongoDB)
<!--more--> 


현재 나는 몽고디비를 사용하고 있다.
몽고디비를 사용하면서 놓친 점, 바로 UTC.

몽고디비는 기본적으로 UTC의 시간대로 제공하고 있다. 
데이터의 일관성을 위해서 서버도 UTC로 관리를 하고 있는데 이때 어려운 점이 있다. 
보기 어려운 것도 어려운 건데, 이거보다 이슈가 되는 점이란?

```
시간 기준으로 데이터를 불러오는 건 어떻게 가져와야 하지?
```

<br />

오늘 하루 유저가 쓴 글의 리스트를 가져온다고 해보자.

```java
package com.test.java;

import org.bson.types.ObjectId;

import org.springframework.data.mongodb.repository.MongoRepository;
import org.springframework.data.mongodb.repository.Query;

import java.time.LocalDateTime;
import java.util.List;

public interface PostRepository extends MongoRepository<Post, ObjectId> {

    @Query(value = "{'userId': ?0, 'createdDateTime': {'$gte': ?1,'$lte': ?2}}")
    List<Post> findPostByUserIdToday (ObjectId userId, LocalDateTIme from, LocalDateTime to);

}
```

아마 데이터베이스에 데이터를 가져올 때부터 이 기준으로 가져오게 될 텐데, 그럼 여기에 들어가는 LocalDateTIme이란?
Java 8부터 지원하고 있고, 로컬 날짜 클래스이다.

여기서 문제란 createdDateTime은 어떤 시간으로 들어올지 모르는데, 무조건 한국 서비스라고 한다면 한국 날짜와 시간이 들어올 것이다. 
근데 MongoDB의 데이터베이스는 UTC잖아...? 
그럼 데이터를 가져오는 기준이 잘못되어 원하지 않는 데이터가 나온다.


이때 어떻게 해볼 수 있을까, 고민하다가 ZoneDateTIme을 사용하기로 했다. 
타임존을 반영하는 LocalDateTIme이라고 볼 수 있다.

이런 식으로 사용해볼 수 있다.

```java
@Transactional
public Optional<Post> getPostsByUserId(ObjectId userId) {
    LocalDate today = LocalDateTime.now(ZoneId.of("Asia/Seoul")).toLocalDate();
    LocalDateTime from = todayDate.atStartOfDay(ZoneId.of("Asia/Seoul")).withZoneSameInstant(ZoneId.of("UTC")).toLocalDateTime();
    LocalDateTime to = todayDate.atTime(LocalTime.MAX).atZone(ZoneId.of("Asia/Seoul")).withZoneSameInstant(ZoneId.of("UTC")).toLocalDateTime();
    return repository.findPostByUserIdBetween(userId, from, to);
}
```


위 코드는 한국 시간만 대응하는 코드로 적어본 코드이다. 코드의 순서란 이렇다.

```
1. 현재 시간을 한국 시간으로 바꾼다.

2. 한국 시간을 오늘의 일자로 생각하면 오늘의 처음 시간인 0시 0분을 계산하고, 마지막 시간인 23시 59분 59초를 계산한다.

3. 이 한국 시간을 다시 UTC로 변환한 다음 DB 쿼리 조건에 넣는다.
```

그럼 각 타임존별로 받을 수 있을까? 가능하다. 
현재 한국시간으로 하드코딩되어 있는 부분을 클라이언트로부터 받아서 처리할 수 있을 것이다. 
위 코드에서는 인자 값이 하나 더 늘어나게 될 것이다.

아마도 몽고디비가 아니라 일반 RDB였다면 이런 것들이 대응되는 것으로 알고 있는데, 몽고디비는 기본이 UTC이기 때문에 이러한 날짜나 시간 기준으로 가져오는 데이터에는 주의 요망!