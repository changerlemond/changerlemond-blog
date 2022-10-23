---
title: "MongoDB 완전관리형, DocumentDB 사용하기"
date: 2022-10-03T01:23:00+09:00
tags: ['mongoDB']
draft: false
---
AWS DocumentDB 첫 셋팅 이야기
<!--more--> 

현재 프로젝트에서 MongoDB를 사용하고 있는데, DB만을 관리할 수가 없어서 AWS에서 제공하는 DocumentDB를 사용하게 되었다. AWS에서 제공하고 있는데, MongoDB와 완전히 호환되는 서비스이다.

https://ap-northeast-2.console.aws.amazon.com/docdb/home?region=ap-northeast-2#landing


내가 MongoDB를 사용하는 이유는 아래와 같다. MongoDB를 검색하면 "몽고디비 쓰지 마세요"가 뜨는 게 왠지 웃프지만 장점이 많다.

- RDB에 비해 성능이 100배 이상 빠르다.
- 스키마 관리가 필요 없다.
- 샤드 추가가 간편하다.


단점도 분명 존재한다.

- 복잡한 쿼리를 사용할 수 없다. (비관계형 DB라서 join이 안된다...^^)
- 메모리 사용량이 크다.
- 데이터 일관성이 보장되지 않는다.


그럼에도 스키마 관리가 필요하지 않고 속도가 빠르기 때문에 MongoDB를 사용하고 있다. DB도 관리해주지 않으면 서버가 내려갈 수 있기 때문에 운영이 필요한데, 운영을 할 리소스가 되지 않아 알아보게 된 것이 DocumentDB이다.



DocumentDB를 만들어본 과정을 기록해본다. Amazon DocumentDB 시작을 누르면 아래의 화면부터 시작한다.

<div style="text-align:center">
    <img src="/images/mongo/documentdb-1.png" alt="documentdb-start" />
</div>

여기서 중요한 건 아무래도 인스턴스 클래스이다. 기본적으로는 db.r6g.large로 되어 있으면 금액부터가 (....) 
가장 낮은 클래스는 db.t3.medium이다. 
무료 평가판으로 사용이 가능하기 때문에 이 버전부터 사용하면 좋을 것 같다. (참고로 r은 굉장히 높은 금액이라고 들었다. 
밑에 보면 금액이 얼마 나오는지 대략 알 수 있기 때문에 요금 페이지 혹은 비용 최적화 설명서를 보면 된다.)
인스턴스 개수도 기본적으로 3개를 제공하는데, 데이터베이스 규모에 따라 결정하면 될 것 같다. 
나는 여기서 1개로 변경했다.

인증의 경우는 mongoDB의 마스터 계정을 만들어주는 것이다. 
이름과 비밀번호를 모두 만들어주면 된다.
<div style="text-align:center">
    <img src="/images/mongo/documentdb-2.png" alt="documentDB-setting" />
</div>
<div style="text-align:center">
    <img src="/images/mongo/documentdb-3.png" alt="documentDB-setting" />
</div>
밑에 고급 설정을 표시하면 vpc가 필요하면 별도 설정이 가능하고, 클러스터 옵션을 보면 mongoDB인 27017 포트로 되어 있다.

여기서 중요하게 본 건 백업이다. 
백업 기간은 최대 1일부터 35일까지 가능하고 기간에 따라 추가로 요금이 나가는 것은 없기 때문에 가장 긴 기간으로 해놔도 좋을 것 같다.

그리고 데이터베이스이기 때문에 삭제 보호가 기본적으로 되어 있다. 
이것도 고급 설정이 가능하다.
생성을 누르면 클러스터가 생성되는데 리전 클러스터와 복제본 인스턴스가 만들어진다. 
DB이기 때문에 원본과 복제본이 만들어지는 것으로 보인다.

나는 참고로 tls 설정을 사용하지 않았다. 
같은 vpc를 쓰고 있기 때문에 tls는 기본적으로 true가 되어 있어 파라미터 그룹을 별도로 설정해주었다. 
파라미터 그룹을 새로 생성해주고, 파라미터 설정을 tls를 바꿔준다.

그리고 클러스터를 수정 화면에 들어가서 클러스터 옵션을 tls-disabled 설정으로 변경해준다. 
파라미터 그룹은 사용 중이면 옵션 변경이 되지 않아서, 새로 옵션을 적용해줄 파라미터를 만들어준 후 새롭게 적용시켜주었다. 
새롭게 적용시켜 준 후에는 반드시 클러스터를 재실행시켜줘야 한다. 
이렇게 pending-reboot 상태라는 건 설정이 변경되어 재부팅이 필요한데 pending 되어있다는 상태이다.   

<div style="text-align:center">
    <img src="/images/mongo/documentdb-4.png" alt="documentDB-pending" />
</div>

재부팅은 클러스터 목록에서 재부팅이 필요한 클러스터를 클릭해 작업을 누르면 재부팅 메뉴를 찾을 수 있다.  

<div style="text-align:center">
    <img src="/images/mongo/documentdb-5.png" alt="documentDB-reboot" />
</div>
<div style="text-align:center">
    <img src="/images/mongo/documentdb-6.png" alt="documentDB-complete" />
</div>

서버의 DB와 연결을 해보려고 한다. mongoDB 주소는 이렇게 연결해주면 된다.

```
mongodb://본인 아이디:본인 패스워드@db 주소:27017/database collection 이름?tls=false&retryWrites=false
```
여기서 tls는 false 설정을 해주었기 때문이고, retryWrites는 false로 설정해야 문제없이 서버에 연결된다.

MongoDB는 MongoDB Compass를 이용하면 DB에 접근하기가 쉽다. 
Compass에도 접속하는 방법은 어렵지 않다. 
같은 uri로 접속하되 proxy/ssh를 설정해주면 접속이 완료된다. 
MongoDB 완전 관리형이기 때문에 동일하게 사용할 수 있다.

처음 DocumentDB를 생성하고 접속할 때 tls와 retryWrites 설정으로 고생했던 기억이 있어서 남겨두게 되었다. 끝!
