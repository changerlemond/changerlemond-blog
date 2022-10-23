---
title: "MongoDB Day Seoul 2022 (feat. data search)"
date: 2022-10-23T18:53:36+09:00
tags: ['mongodb', 'conference']
draft: false
---
2022 몽고디비 컨퍼런스를 다녀와서
<!--more--> 
#

우연히 MongoDB Conference가 있다는 것을 알게 되었다.
https://www.mongodb.com/events/mongodb-days-apac-2022/seoul

데이터베이스에 관심이 많은 편이었기도 했고, 현재 현업에서 몽고디비를 사용하고 있어서 이 컨퍼런스는 가야겠다 생각했다.  
이것도 다른 컨퍼런스처럼 선착순이나 추첨인가... 했는데 생각보다 빨리 신청하기도 했고, 추첨이 아니라서 바로 신청 완료된 메일이 왔었다.  

나에게는 첫 오프라인 컨퍼런스였는데, 너무 몽고디비 홍보이진 않을까? 우려도 했지만 생각보다 그렇지 않고 오히려 데이터 관리를 잘해야겠구나 생각하게 된 컨퍼런스였다.

<div style="text-align:center">
    <img src="/images/conference/mongodb-conference-0.jpeg" alt="mongodb-conference" />
</div>
<div style="text-align:center">
    <img src="/images/conference/mongodb-conference-1.jpeg" alt="mongodb-conference" />
</div>

가니까 사람이 굉장히 많았다. 엘리베이터에서 내리자마자 붐비는 인파에 줄을 서느라 정신이 없었다.
입장하기 위한 절차들을 하고 나서야 꾸며진 몽고디비 부스와 후원사인 aws, confluent, goodus data의 부스를 볼 수 있었다.   

부스 구경은 사실 입장할 때 수많은 인파로 도저히 할 수 없는 상태였고, 중간 쉬는 시간에야 둘러봤다.
컨플루언트 굿즈는 갔을 때 하나 남은 상태여서 받지 못했지만, 컨플루언트와 aws의 스티커들을 가져왔다. (~~보기 힘든 카프카 스티커 획득...~~)

<div style="text-align:center">
    <img src="/images/conference/mongodb-conference-4.jpeg" alt="mongodb-conference-stickers" />
</div>

한 공간에서 계속 컨퍼런스를 듣는 시간들이었는데, 생각보다 많이 몽고디비에서 잘 준비해 주셔서 불편함이 없었다.
있었다면 와이파이가 되지 않았다는 점...? 너무 잘 준비 해주신 덕에 컨퍼런스 듣기가 좋았다.
(~~식사 제공을 이렇게 잘해주실지 몰랐다~~)

<div style="text-align:center">
    <img src="/images/conference/mongodb-conference-2.jpeg" alt="mongodb-conference-stickers" />
</div>
<div style="text-align:center">
    <img src="/images/conference/mongodb-conference-3.jpeg" alt="mongodb-conference-stickers" />
</div>

<br />
<br />
<br />

## 컨퍼런스를 듣고 간략한 내용들과 느낀 점

### Keynote - MongoDB Developer Data Platform
첫 시작은 몽고디비의 VP인 Michael Cahill 님의 발표가 있었다.
영어 발표니까 아무래도 내가 못 알아들으면 어떡하지 했는데 번역기가 준비되어 있었다.
처음에 겁먹고 번역기를 썼는데, 생각보다 알아들을 수 있었던지라 (?) 그냥 바로바로 들었다. 
말씀을 빠르게 하시는 편도 아니었고, 중간중간 유머러스한 부분들도 있어서 영어 발표였지만 재밌게 들었다.

몽고디비의 키노트다보니 몽고디비의 여러 기능들을 소개 해주신 부분이 컸고, 여러 연혁들 소개도 있었다.
보통 이런 부분들이 좀 지루하고 진부하다고 느끼는 발표인 가능성이 높은데, 흥미롭게 들었다.

여러 이야기 중에서 가장 기억에 남는 부분이 있었다.

```
Developer productivity = Innovate faster

개발자의 생산성을 높이는 방법은 
개발자들이 데이터 관리에 대해서 빠르고 쉽게 일할 수 있도록 하는 것.
```

몽고디비에서 이런 부분을 크게 생각하고 고객사들의 이야기를 반영해서 반영해 준다는 부분이 어떻게 보면 당연할지 몰라도, 제일 인상 깊었던 것 같다. 
이런 관점으로 만들어주신다면 지금보다 더 좋은 데이터베이스가 되어갈 것 같았다. 

NoSQL이 많이 쓰이는구나가 느껴진 부분이, 초기 12년보다 최근 12개월의 다운로드 수가 많다는 부분이었다.
(ㄴㅇㄱ) 하면서 신기했고, 몽고디비에서도 엄청난 비용을 투자하는 것을 보고 이렇게 투자해서 좋은 제품을 만들려고 노력하는구나 생각했다.
누적 10억 달러 투자라니... 와닿지 않는 수준의 숫자라서 놀라웠다.


#
### 시리즈 A 스타트업이 MongoDB Atlas Search를 선택한 이유
다음은 인프런의 이동욱 CTO 님의 발표가 있었다. 
사실 이 세션이 가장 기대가 되었었다.
아무래도 나도 스타트업에서 일하고 있다 보니 더 궁금했던 것 같고, 인프런을 쓰고 있는 유저의 입장으로 요즘 검색이 더 좋아졌다고 느껴서 사실은 더 궁금했다.

기술적인 이야기는 몽고디비 측의 세션이 또 있었어서 이동욱 님은 스타트업에서 겪은 이야기들을 많이 해주신 편이었다.  
나도 사실 당연하게는 Amazon OpenSearch를 생각했었는데 인프런에서는 OpenSearch, DynamoDB 등 서비스가 커지면서 변경해야 하는 스택들이 많았다고 한다.
이걸 MongoDB Atlas Search라는 하나의 솔루션으로 해결했다고 하셨다.
여기서 개발팀원들의 상황을 고려하신 점도 개인적으로 대단하다고 느꼈다.
당연할지 모르지만 당연하게 하기 힘든 부분이라고 생각하기 때문에.

가장 인상적인 말이 있었다. 
오늘 세션 중에서 가장 좋은 세션이었지만, 오늘 세션을 통틀어 가장 와닿는 말이 있었다.
너무 멋진 말이라고도 생각한다.

```
스타트업은 기술 부채 관리의 예술이라고 한다. 
내가 원하는 양만큼의 기술 부채를 통해 
회사의 레버리지를 높이느냐가 스타트업에게 가장 중요한 역량이다.
```

여러 발표들을 많이 하시는 편이라 그런지 말씀을 굉장히 잘하셔서 듣기 제일 좋은 세션이었다.
무엇보다 마지막은 회사 홍보를 너무 잘하신 게 아닐까 싶다. **"면접장에서 뵙겠습니다."** (ㅋㅋㅋ)


#
### MongoDB 6.0 새로운 기능
이번 발표는 몽고디비의 김준 기술총괄 전무님의 발표로 이어졌다.
몽고디비에 생각보다 많은 기능들이 들어갔다는 것을 느낀 발표였다.
사실 세션을 골라 듣는 걸 생각했으면 안 들었을 수도 있는데(?) 듣고 생각보다 음 몽고디비에 이런 것들이 추가된다고?라는 생각을 하며 들었다.

이 발표에서도 개발자의 생산성에 대한 언급이 많았다.
개발자의 생산성이 떨어지지 않도록 여러 기능들을 추가했구나 싶었다.

Time Series 기반의 데이터 처리가 가장 기억에 남는다. 
발표에서도 가장 맨 앞장에 있을 만큼 몽고디비에서도 주요 기능으로 생각하는 느낌이었다.
아무래도 이런 데이터가 많이 다룰 일은 많은데, 데이터가 너무 커서 다루기도 어렵고 관리가 어렵기 때문이다.

그리고 Cluster To Cluster Sync 기능. 
다른 몽고디비 클러스터로 데이터를 복제하고 동기화를 지원하며, 다양한 환경에서 Deploy가 가능했다.  
아쉬웠던 점은 아무래도 아직 단방향만 지원한다는 점, 곧 양방향도 지원한다는 이야기는 같이 있었다.

오 이런 게? 싶었던 건 Relational Migrator였다. 아무래도 RDBMS를 아직도 많이 사용하고 있어서 이게 익숙하고, NoSQL이 어렵다는 인식 둥 몽고디비를 쓰기 어려운 여러 장애물들이 있는 부분을 이러한 기능으로 해소하려 한 것 같았다.


#
### MongoDB Atlas Search Deep-dive Session
점심을 먹고 나서 다음 발표는 몽고디비의 최근한 이사님께서 해주셨다. 
Search에 대한 세션이 별도로 있는 점, 그리고 다른 세션에서도 Search에 대한 언급이 종종 있었던 걸로 보아 정말 데이터 검색에 대한 니즈는 계속 상승한다고 느꼈다.

나도 현업에서 해볼 기회는 없었지만 원래 이 분야에 관심이 많았던지라, 개인적으로 Elastic Search를 공부 중인데 이 분야는 계속해서 커지겠다는 생각도 함께 들었다.

발표에서 언급됐던 Apache Lucene이 궁금해서 깃헙 코드를 보면서 발표를 들었다. 신기방기...
간단하게 Analyzer, Inverted Index, Score에 대한 설명을 해주시고 Search에 대해 깊게 설명해 주셨다.

그리고 어떻게 사용해야 하며, 어떻게 사용하면 안 되는지에 대한 이야기가 이어졌는데 들으면서 생각한 건 역시 잘 사용해야 한다는 점.
당연한 것일지도 모르겠는데 개발하다 보면 데이터 운영은 정말 비효율인 걸 알면서도 두게 되는 경향이 있기 때문인 것 같다.

Atlas Search를 쓰게 될 날이 오게 될지 모르겠지만, 매우 흥미롭긴 했다.
특히 개인적으로는 설명해 주시면서 나온 lucene이 흥미로웠다. 나중에 또 찾아봐야지 싶은!


#
### Confluent와 함께 분산된 데이터를 MongoDB로 연결하기
다음은 후원사였던 Confluent의 김현수 상무님의 발표가 있었다. 
Confluent는 Kafka를 쓰고 있어서 나한테는 워낙 익숙한 회사였다.

주로 Confluent를 어떻게 쓰는지 미리 찍어온 데모를 보여주면서 Confluent 편해요! 써보세요!의 이야기가 주된 것이었기 때문에 적을 내용이 많진 않다.
다만 하나 오 그렇지! 이렇게 다들 알았으면! 하는 내용은 있었다.

```
최신 클라우드 데이터 플랫폼에는 Data in Motion 기술이 필요하다.
모든 것이 정지되어 있다는 생각을 바꿔 모든 것이 움직인다고 생각해야 한다.
```

데이터에 대한 니즈는 정말 많지만, 이걸 잘 관리하고 운영하는 건 힘든 일이다.
Confluent에서 이러한 니즈를 잘 파고들어 서비스를 잘 만들었다고 생각한다. 데모 보는데 굉장히 편해 보였다.
나도 카프카 공부할 때 무료로 잠깐 써봤었는데 설정이 간편하긴 했다.


#
### MongoDB Atlas App Service
잠시 휴식시간에 커피를 마셨다. 커피를 제공해 주셔서 한번 마셔봤는데 다행히 커피 효과를 받고 다시 세션을 들었다.
다음 세션은 몽고디비의 김규동 상무님께서 발표하셨다.

서버리스에 대한 설명인지라 백엔드를 하는 내가 크게 흥미가 있거나 와닿는 내용은 아니었다.
프론트엔드만 하시는 분들이 오히려 재밌을만한 부분이 아닐까. 그래서 크게 적을 내용이 없었다 (ㅜㅜ)
이건 현장에서 시연해 주셨는데 개인적으론 그냥 그렇구나... 하면서 들었던 세션이었다.


#
### MongoDB Atlas on AWS와 함께하는 MSA Journey
궁금했던 세션 중 하나였는데, 이 발표는 AWS의 SA를 하고 계신 윤기원 파트너님이 해주셨다.
이야기 해주셨던 주제는 비즈니스 / 데이터 트렌드 & 마이크로 서비스, MSA 구조에 맞는 목적별 디비를 사용해야 하는 이유, 몽고디비 사용 사례와 고객 사례를 발표해 주셨다.

요즘 트렌드도 알려주셔서 참고가 되었다.
```aidl
비즈니스 / 데이터 트렌드
- 사용자와 데이터의 폭발적인 증가: 다양한 데이터 소스와 데이터 종류
- 빠르게 변화하는 시장과 고객의 요구: DevOps 도입으로 인한 변화율 증가
- 모놀리식 시스템에서 Micro-Service로의 변화: 다양한 개발 언어와 데이터베이스의 활용
```

사실 모놀리 아키텍처에 대한 한계는 많이들 알고 있는 것 같다. 
나 또한 이제 왜 모놀리식은 한계가 있다고 말하는지 알고 있었지만 더 명확하게 알려주셔서 좋았다.

```
- 확장성 한계
- 성능 이슈
- 개발자들의 유연성 저해
- 개발 및 배포 지연

통합된 하나의 관계형 데이터베이스는 
현대 비지니스 애플리케이션 요구사항을 더 이상 충족할 수 없다.
```

MSA 구조에 맞는 목적별 디비를 사용해야 하는 이유는 **확장성, 성능, 가용성**으로 설명해 주셨다.
사실 이렇게 해본 적은 아직 없는데, 잘 기억해 둘 내용들이 많았던 것 같다.

고객사별 사례를 설명해 주신 것도 좋았는데, 다른 회사들도 MSA로 전환을 많이 하는구나 느꼈다.
MSA는 사실 의견이 많이 갈리는 주제인 것 같은데 상황에 맞게 잘 사용한다면 효율적인 아키텍처임은 확실하다고 생각한다.


#
### 네이버의 MongoDB 활용사례
마지막 세션은 네이버 클라우드의 이재웅 매니저님께서 해주셨다.
개인적으로 인프런, 네이버 발표를 기대하고 왔었는데 네이버 세션은 조금 아쉬웠다.
연사분도 말씀하시긴 했지만, 준비가 많이 되어 있는 느낌은 아니어서 그랬던 것 같다.

네이버에서도 역시나 NoSQL를 생각보다 많이 사용하고 있었는데, MongoDB에 대한 니즈가 많이 증가했었다고 한다.
그래서 몽고디비 운영을 위해 모니터링, 알림, DB 설치 자동화, 운영 표준, 경험 등을 고민하였고 정식 운영은 2017년부터 시작했는데, MongoDB Korea와 2019년부터 공식적으로 파트너십을 맺어 많은 도움을 받았다고 한다. 
버전은 사내에서 4.4까지 사용하고 있다고 한다. 

사내에 발생했던 이슈들은 예시로 몇 가지 소개해 주셨다.

```
- Orphaned Documents issue
- Logiclal Sessions 삭제 불가 현상 (single Replica Set)
- Secondary 재구축 방법
- zstd compress method
- Signing key issue
```

몽고디비는 아무래도 한국어 채널이 없다는 게 아쉽다는 것에 동의한다.
이동욱 님도 언급해 주셨는데, 이 부분이 고객사들이 이야기해 주고 있는 만큼 개선되면 좋겠다 싶은 부분이었다.


#

길고 긴 세션이 4시 30분쯤 끝나고 뒤에는 경품 추첨 시간이 있었다.
명함 뽑기였는데 나는... 당연히 구경하고 있었고 (^^)
사실 오늘 컨퍼런스에 대한 설문조사를 하면 몽고디비 굿즈를 주셔서 그걸 기다리고 있었다.

<div style="text-align:center">
    <img src="/images/conference/mongodb-conference-5.jpeg" alt="mongodb-conference-stickers" />
</div>
<div style="text-align:center">
    <img src="/images/conference/mongodb-conference-6.jpeg" alt="mongodb-conference-stickers" />
</div>

몽고디비 굿즈는 처음이기도 했지만 신경을 꽤나 써준 게 느껴졌었다.


#
컨퍼런스 전체적으로 신경을 많이 써준 컨퍼런스라는 느낌을 받았다.
그리고 평소 관심 있던 데이터 컨퍼런스라 더 재미있었고, 느끼는 점도 많았고... (반성을 많이 했다는 게 맞을 것 같다 ^^)
같이 갔던 개발자 동료분과 관련해서 여러 이야기들도 나누면서 들어서인지 더 재밌게 들을 수 있었다!

다음 몽고디비 컨퍼런스도 들으러 갈 것 같다. 매우 유익했던 시간이었다.
다만 쉬는 시간이 매우 짧은지라 컨퍼런스를 9시 30분부터 4시 30분까지 계속 듣다 보니... 일하는 것보다 더 힘들었다는 후문 ^^;
근데 그만큼 느끼고, 배우고, 생각할 점들이 많아진 것들이 있어서 뜻깊은 하루였다.