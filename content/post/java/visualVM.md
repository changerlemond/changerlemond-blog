---
title: "VisualVM 사용하기"
date: 2022-10-30T22:45:38+09:00
tags: ['java']
draft: false
---

VisualVM 사용하기
<!--more-->

#
### VisualVM이란 무엇일까?
JVM으로 구동되는 Application의 Thead 및 GC 사용량을 모니터링할 수 있는 JDK 도구  
OpenJDK는 내장되어 있지 않기 때문에 https://visualvm.github.io/ 에서 다운받아야 한다!


#
### VisualVM 실행하기
VisualVM을 실행하면 이런 화면을 만날 수 있다.
<div style="text-align:center">
    <img src="/images/java/visualVM_0.png" alt="visualVM_start" />
</div>

<br/>

여기 목록에서 주요하게 볼 부분은 Local, Remote이다.  
로컬은 현재 컴퓨터에서 JVM으로 돌아가고 있는 Application 목록이고, 리모트는 로컬 컴퓨터가 아닌 다른 서버에 있는 Application 목록이다.  

로컬의 경우 별다른 설정을 하지 않아도 pid와 함께 JVM으로 돌아가고 있는 어플리케이션들이 보이지만, 리모트의 경우 별도로 연결을 해주어야 한다.

<br/>

우선 자바 어플리케이션 run.sh에 해당 내용을 설정해준다.
```java
java
-Dcom.sun.management.jmxremote=true
-Dcom.sun.management.jmxremote.local.only=false
-Dcom.sun.management.jmxremote.port=[JMX PORT 설정]
-Dcom.sun.management.jmxremote.ssl=false
-Dcom.sun.management.jmxremote.authenticate=false
-Djava.rmi.server.hostname=[붙을 서버 IP]
-Dcom.sun.management.jmxremote.rmi.port=[JMX PORT 설정] 
```

다음 remote를 우클릭해서 Add Remote Host...를 클릭한다.
<div style="text-align:center">
    <img src="/images/java/visualVM_1.png" alt="visualVM_add_remote" />
</div>

<br/>

host name에 연결할 remote ip를 입력해서 ok를 클릭해 연결한다. 정상적으로 연결됐다면 컴퓨터 모양 옆에 초록색 동그라미가 함께 떠있을 것이다.
해당 host를 우클릭하면 아래와 같은 메뉴들이 뜨는데, 여기서 Add JMX Connection을 선택한다.

<div style="text-align:center">
    <img src="/images/java/visualVM_2.png" alt="visualVM_JMX_Connection" />
</div>

connection에 ip는 자동으로 적혀있을 것이고, 뒤에 서버 port를 적어서 연결하면 완료된다.

<br/>

아래와 같이 시각적으로 모니터링을 할 수 있다. GC, Heap Dump 등 여러 지표들을 볼 수 있다.

<div style="text-align:center">
    <img src="/images/java/visualVM_3.png" alt="visualVM_Monitoring" />
</div>