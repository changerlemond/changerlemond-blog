---
title: "엘라스틱 서치에 대해 알아보기"
date: 2023-02-26T22:01:54+09:00
tags: ['elastic-search', 'infra']
draft: false
---

엘라스틱 서치, 왜 사용할까?
<!--more--> 

#
회사에서 엘라스틱 서치 스택을 사용해보게 되었다. 많이 들어보기는 했고, 요즘 많이 쓴다고도 들었는데 어떤 때 쓰는지 좀 더 생각해보고 싶었다.
검색에 많이 쓰인다는 것은 알고 있었는데, 검색은 사실 데이터베이스에서 검색이 충분히 될거라고 생각하고 있었기 때문에 뭔가 최적화 되는 기술인가 생각했다.
왜 사용하는지, 엘라스틱 서치를 사용하면 어떤 점들이 이점인지 알아보면서 정리해보는 글이다.

#
### 엘라스틱 서치, 그게 뭔데요?
엘라스틱 서치를 검색하면 나오는 이야기다.

> Apache Lucene( 아파치 루씬 ) 기반의 Java 오픈소스 분산 검색 엔진  
> 방대한 양의 데이터를 신속하게, 거의 실시간( NRT, Near Real Time )으로 저장, 검색, 분석할 수 있다.

#
### RDB와 비슷한거 아닌가?
엘라스틱 서치는 그럼 RDB를 대체할 순 없을까? 생각해봤다.  
실시간으로 저장, 검색, 분석이 되는데 RDB와 비슷한 하나의 데이터베이스라는 생각이 들었다.  

하지만 저장 방식이 다르기 때문에 대체하기는 어려워보였다.

<br>

|       RDBMS        | Elastic Search |
|:------------------:|:--------------:|
|       Schema       |    Mapping     |
|      Database      |     Index      |
|       Table        |      Type      |
|        Row         |    Document    |
|       Column       |     Field      |
|    Primary Key     |      _id       |
|       Index        |    Analyze     |
| Physical partition |     Shard      |
| Logical partition  |     Route      |
|        SQL         |    QueryDSL    |

<br>

그럼 REST API를 이용할 때의 차이점도 있을까? 해서 차이점을 정리해보았다.

|   CRUD   |  RDBMS   |  Elastic Search  |
|:--------:|:--------:|:----------------:|
|   READ   |  SELECT  |       GET        |
|  UPDATE  |  UPDATE  |       PUT        |
|  CREATE  |  INSERT  |       POST       |
|  DELETE  |  DELETE  |      DELETE      |


#
### 엘라스틱 서치, 사용하면 어떤 점이 좋은데?
- 빠른 속도
Lucene을 기반으로 구축되기 때문에 전체 텍스트 검색이 RDBMS에 비해 굉장히 뛰어나다.  
거의 실시간 검색 플랫폼이라고 볼 수 있다. (일정 시간이 지나야 검색되지만 거의 실시간이다.)
- 분산 처리
샤드라는 개념이 있고, 이 샤드는 복제되어 장애 시 데이터 사본을 제공해준다.  
분산 처리가 되기 때문에 서버 확장에 유리하다.
- 검색 기능
엘라스틱 서치에서는 여러 단어로 변형하거나, 동의어나 유의어 등의 검색도 가능하다.  
여러 플러그인으로 형태소 기반 자연어 처리도 가능하다.
- 비정형 데이터를 저장하기 유리
검색 엔진이지만 대용량 스토리지처럼 사용도 가능하다. (로그 분석 시 많이 쓰인다. ELK!)

#
### 장점만 있진 않을텐데!
단점을 찾아보니 트랜잭션과 롤백의 기능은 없었다. 
분산 시스템 환경에서 시스템적 비용이 많이 드는 것이 트랜잭션과 롤백 기능인데, 그래서 두 기능을 지원하지 않는 것으로 보였다.

그리고 완전한 실시간 처리는 아니다. 
인메모리 버퍼를 사용하기 때문에 쓰기와 읽기를 동시에 할 경우, 세그먼트가 생성되기 전까지는 해당 데이터를 검색할 수 없다.

#
### 엘라스틱 서치, 어떤 구조로 되어 있을까?
장단점은 충분히 이해가 갔다. RDB에서도 like와 같은 기능이 사용 가능하지만, 검색 기능에 한계가 있는 점도 알 수 있었다.
엘라스틱 서치는 그럼 어떤 구성으로 되어 있을까?

<br>

**Cluster**  
- 엘라스틱 서치의 큰 시스템 단위
- 최소 하나 이상의 노드로 이루어진 노드의 집합
- 여러 대의 서버가 하나의 클러스터를 구성할 수 있고, 한 서버에 여러 개의 클러스터가 존재할 수도 있다.

**Node**
- 클러스터에 포함된 단일 서버
- 데이터를 저장하고, 클러스터의 색인화 및 검색 기능에 참여
- 역할에 따라 Master-eligible, Data, Ingest, Tribe 노드로 구분

**Master-eligible Node**
- 클러스터를 제어하는 마스터로 선택할 수 있는 노드
- 인덱싱 생성, 삭제
- 클러스터 노드의 추적, 관리
- 데이터 입력 시 할당할 샤드 선택 가능

**Data Node**
- 데이터가 저장되는 노드
- 데이터가 분산 저장되는 물리적 공간인 샤드가 배치되는 노드
- CRUD, 색인, 검색, 통계 등의 데이터 작업을 수행
- 많은 리소스(CPU, 메모리 등)를 필요로 함
- 모니터링 작업을 해야 하는 노드
- 마스터 노드와는 분리

**Ingest Node**
- 데이터를 변환하는 등 사전 처리 파이프라인을 실행하는 역할

**Coordination Only Node**
- 사용자의 요청을 받고 라운드 로빈 방식으로 분산을 하는 노드
- 로드밸런싱 역할

**Index**
- 데이터베이스와 대응하는 개념

**Shard**
- 인덱스 내부에는 색인된 데이터들이 존재하는데, 하나로 뭉쳐서 존재하지 않고 물리적인 공간에 여러 부분으로 나누어 존재하는 부분
- 샤드는 데이터의 원본인 프라이머리 샤드와 복제본인 레플리카 샤드(장애 복구용)로 나누어진다.

**Segment**
- 문서의 빠른 검색을 위해 설계된 자료 구조
- 샤드의 데이터를 가지고 있는 물리적인 파일
- 각 샤드는 다수의 세그먼트로 구성되어 있으므로 검색 요청을 분산 처리하여 훨씬 효율적인 검색이 가능

#
### 엘라스틱 서치, 본격적으로!
현업에서 검색 기능을 최적화하기 위한 작업을 진행하고 있다. 
엘라스틱 서치에 대해 많이 들어봤고, 설치해서 띄워보기는 했지만 데이터를 다뤄보는 것은 처음이었기에 개념에 대해서 다시 정리해보았다.

다음글은 엘라스틱 서치에서 검색 기능을 최적화한 경험을 적어보려고 한다!