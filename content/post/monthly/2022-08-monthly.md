---
title: "2022. 08 회고"
date: 2022-09-21T01:58:00+09:00
tags: ['monthly']
draft: false
---
8월 회고하기
<!--more--> 

#
갈수록 회고가 늦는다. 벌써 9월 21일이면 9월도 다 가고 있는데 이제야 8월을 마무리해보기로 했다. 어쩌면 8월에는 정신없이 흘러간 부분들이 있기도 했지만, 이마저도 핑계인걸 안다.
사실 좀 혼돈의 8월이긴 해서 솔직하게 돌아볼 시간이 필요하긴 했다. 그러기엔 물리적인 시간이 이렇게 부족할 수 있구나를 느끼지만, 며칠 동안 8월을 돌아보고 9월도 미리 돌아보고 있다. 글을 쓰면서 8월도 다시 한번 돌아보려고 한다.




#
### 서비스 출시, 이대로 괜찮을까?
8월을 돌아보자면 이 생각을 가장 많이 했다. 
사실 9월 21일인 지금은 서비스 출시를 잘 하긴 했다. 
8월 말엔 정말 서비스 버그들을 급하게 고치는 프론트엔드 개발자분들의 바쁜 모습과 그걸 같이 체크하는 서버 개발자의 모습이 많았다.

지금까지 회사에서 이번으로 세 번째 프로덕트를 만들고 출시하고 있는데 항상 막바지에 매번 아쉬운 점이 있고, QA를 할 때마다 보이는 버그들은 어쩔 수 없다지만 이번에는 참 마음이 급했던 것 같다. 
그래서 내가 프론트엔드 앱 코드를 고치고 있지는 않았지만 최대한 도움을 드릴 수 있는 부분들은 빠르게 도움드리려고 노력했었다. 
서로가 프로덕트 출시일에 맞추느라 마음이 불안하기도, 어렵기도 했던 것 같다.

이렇게 매번 출시해도 괜찮을까 라는 생각을 이번엔 문득 한 적이 있다. 
매번 프로젝트 일정이 조금 짧긴 했지만, 어쩔 수 없는 상황이 있다는 것을 알면서도 프로젝트 일정을 조금이라도 여유 있게 잡을 순 없을까?라는 생각도 많이 들던 이번 서비스 프로젝트. 
6월 중순부터 착수했으니까 정말 약 3달 만에 새 프로덕트로 변신해서 나온 셈인데 물음표가 있긴 하다. 
속도와 정확도 사이, 항상 중간이 어려운 것이겠지만... 항상 개발의 어려움은 존재하지만... 이번만큼 어려웠던 적이 없었다. 
앞으로도 계속 기능 추가도 할 테고 개선도 할 텐데, 조금이라도 부담이 덜하면 어떨까 생각해보게 되었다. 

나뿐 아니라 다른 개발자분들이 지쳐있는 모습을 볼 때마다 마음이 좋지 못했고 또 무겁기도 했다. 
나 또한 8월에도 컨디션 조절에는 실패했다. 
이건 어쩔 수 없었다는 핑계를 또 대 보지만, 내 몸이 괜찮을까에 대한 반성도 많이 한다. 
실제로 8월에는 몸이 너무 아파서 휴가 쓴 날이 꽤나 되더라. 
휴가를 쓰고도 근무 시간은 220시간이라면... (내 몸 반성...)


#
### AWS 권한의 늪, DevOps의 삽질의 연속이지만...!
회사에서는 AWS를 쓰고 있고, 개발 리소스가 많이 부족하기 때문에 돈을 써서 해결할 수 있는 부분들은 돈을 써서 해결하려고 노력한다. 
그중 하나가 DocumentDB가 된 것 같다.

개발 DB도 관리를 못해서 사용하기 어려웠던 적이 있었기 때문에, 운영 서버가 아님을 다행으로 생각하면서도 운영 DB가 어떻게 되면 어떡하지?라는 걱정도 많이 했다. 
그래서 이번에 서비스 출시할 때는 운영 DB를 기존 MongoDB 세팅이 아니라 AWS의 도움을 받아 DocumentDB를 사용하기로 했다. 
개발이나 테스트는 이슈가 나도 큰 문제가 되지 않지만, 운영 DB가 그러면 정말 큰일이니까.

그래서 처음 세팅을 해볼 때 애를 먹었다. 
아니 주소가 이렇게 달라..? (ㅠㅠ) 이건 따로 개발 삽질 이야기로 적어봐야 할 것 같다. 
ELB 포함..

ELB 설정도 원래 사용은 했는데 당시에 내가 설정하지 않았었고, 이번 서버는 설정을 직접 해봤는데 진짜 고려할게 많더라. 
뭐만 하려 하면 권한이 없고... 권한이 없고... 또 없고... AWS의 권한은 어디까지인가... 를 느끼며 어렵게 했었다. 
그래서 동료 개발자 분과 새벽 2시까지 고생했던 기억이 있다. 
늘 느끼지만 DevOps는 삽질하면서 배우는 편이라 하루 종일 서버 세팅을 하기도 했는데 그게 참 많이 배우기도 한 것 같아서 좋았다. 
서버 이름도 내가 지었음 (^^!)


#
### QA 엔지니어 간접경험
앱을 만들면 디바이스에서 어떤 일이 일어날지 알 수가 없고 유저는 예측한 행동을 벗어나는 것들을 할 수도 있기에 개발 테스트 같이 하지 않고 정말 유저처럼 사용해보기도 하고 이상하게 사용해보기도 한다. 
QA 엔지니어분이 있으시면 좋겠지만 내부에는 그럴 인력이 없고, 근데 메인 업무들은 각자 있다 보니 잘 안 하게 되는 것이 QA인 것 같았다.

마침 회사에서 QA 데이를 만들어서 하루 종일 QA 하고, QA에서 나온 이슈들을 분석해서 해결점들을 파악하는 시간을 가졌다. 
내부 인원이 많은 편은 아니지만 하루 종일 이렇게 해보니 힘들긴 했다. 🥲 
초기에 QA를 도와주셨던 엔지니어분께서 QA에 대해서 중요성을 알려주셔서 그 부분을 크게 배웠다. 
어쩌면 개발자인 나로서는 간혹 이런 행동을 대체 왜 하는 거야 (ㅠㅠ) 하면서 버그를 고친 적이 가끔 있는데 그러지 말아야겠다고 느낀 때였다. 
이때 아마도 QA의 중요성을 다들 더 알아주지 않았을까? 생각해봤다. (물론 그건 모르는 일이지만...)



#
### 힘들수록 더 이야기하도록!
앞서 이야기가 있지만 프로젝트 기간이 짧았다. 
그렇다 보니 압박감도, 부담감도 있었는데 이런 부분들은 나만 있을 것 같지 않았다. 
나는 그래도 1년 정도 다니면서 프로덕트를 두 번 정도 내보았다면 최근에 오신 분들은 그렇지 않은 분들도 계시니까 더 그러실 거라 생각했다.

그래서 힘든 시간들 속에서도 중간중간 백엔드 이슈 상황은 어떤지, 우선순위는 어떻게 정할지, 어떤 어려움이 있는지 들어보고 같이 해결방법을 고민하는 시간을 가져보고 바쁘지만 커피챗 하는 시간도 가져보려고 노력했었다. 
업무 이야기 말고 그동안 못한 기술 스택 이야기라던가, 같이 일하는 방법도 이야기해보고... 해보면서 이런 시간이 필요하구나 많이 느끼게 된 것 같다. 
이야기하면서 조금이라도 즐겁게, 재밌게 프로덕트를 만들어나가는 과정으로 하기 위해 노력해봤는데 좋다고 생각했다. 
바쁘더라도 너무 일에 파묻혀서 일만 하지 말아야겠다고 생각하게 된 때였다. 
알곤 있지만 이게 참 어렵달까, 근데 새삼 또 느끼게 되는 점이었다.  

8월에 가장 많이 들었던 말은 "클레어랑 이야기하고 싶은데 바빠 보이셔서 (혹은 힘들어 보이셔서) 티타임이나 런치를 요청 못하겠다." 였는데 개인적으로는 반성하게 되는 말이었다. 
업무도 중요하지만 동료들과 같이 이야기하는 시간은 소중한데 말이다.


#
### 금요일마다 무슨 일이 있었나
어쩌다 보니 금요일마다 오피스를 나간 적이 많은데, 그때마다 예상치 못한 일들이 생겨서 퇴근을 늦은 밤 혹은 새벽에 했던 것 같다. 
(강북으로 가는 택시가 이렇게 안 잡히는지 이제야 알았다...) 
하필 그 일들이 조금 지치는 일들이었어서 감정적으로 소모가 처음으로 많이 있었던 8월이었다.

거의 매주마다 그랬다 보니까 주말에 하루는 뻗어버렸는데, 그래서 이번 달은 스터디 참여율이 엉망이었다. 
결국 토요일의 스터디는 종료되기도 했다. 
그래서 8월은 개인적인 공부나 커리어에 있어서는 많이 아쉽다. 
주말에 하루 정도는 쉬려고 하고, 스터디는 잘 참여하려고 노력하는 편인데 그게 전혀 이루어지지 못했다. 
거의 내 개인 생활을 회사에서 일어난 일련의 일들로 인해 많이 망가졌는데, 9월에는 그러지 않겠지? 했지만 아직 현재 진행 중인 게 어렵지만... 
개인의 생활과 건강은 잘 챙기도록 노력해봐야겠다고 반성했다. (매 회고마다 내 건강 이야기가 나올 수밖에 없는 회고라 슬프다 ㅜㅜ)



#
### 환경의 변화, 성공적!
사실 8월은 버거웠던 측면이 많지만 그래서 스스로 그 힘든 부분들을 이겨내기 위해 여러 가지를 시도했다. 
카페에 가서 일을 해보기도 했는데 내가 좋아하는 카페들을 찾아 나섰다. 
그렇게 잠깐 나갔다 오면 좀 괜찮더라.

그리고 주변 분들과 저녁도 자주 먹었다. 
보통 이렇게 저녁을 같이 자주 먹지 않는데 8월은 내가 말도 많이 하고, 친하다 하는 분들이랑은 거의 대부분 만났던 것 같다. 
만나서 여러 이야기들을 나누면서 환기를 시키기도, 또 정신 차리기도 (?) 했었다.

무엇보다 가장 좋았던 건 자연! 주말에 서울숲과 북서울 꿈의 숲을 가보았다. 
평소에 숲을 구경하고 걸으면서 여러 생각을 하기도, 비우기도 하는 과정을 참 좋아한다. 
제주도 같이 어느 지역에 놀러 가야만 가능한 줄 알았는데 서울에도 이러한 공간들이 있고, 가서 힐링하고 오니까 많이 비워지기도 했고 새로운 마음으로 할 수 있어서 좋더라. 
앞으로는 이런 환기도 잘 시켜줘야겠다고 생각했다!


#
8월은 버거웠다는 표현이 많지만, 그럼에도 8월을 돌아보니 의미가 있는 이유는 여러 일들을 생각보다 많이 해봤다는 점. 
그리고 그 과정 속에서 내가 주도적으로 생각하고, 우선순위도 결정해보고, 팀원들의 어려움을 해소해주려 했다는 점들이 있었던 것 같다.

사실 나는 내가 힘들면 상황 자체를 보통 외면하는 경우가 많았는데 요즘은 정면돌파로 맞서고 있다는 점. 
사실 내 성향 자체가 너무 곧은 편이라 부러질까 봐 걱정된다는 분들이 많은데, 이제 그 유연성도 조금씩은 생겨나고 있다고 생각한다. 
이러면서 조금씩 성장하는 게 아닐까 생각해본다.

8월에 들었던 말 중에 기억에 남는 말이 있다. 
세상은 생각보다 우당탕탕 돌아가고 있다는 것. 그러니까 너무 완벽하게, 다 잘할 수도 없고 그렇게 안되더라도 생각보다 잘 돌아간다는 것. 
그러니까 너무 부담 가지면서 할 필요는 없다는 것이다. 
조금씩 유연해질 부분들에 유연해질 수 있는 내가 되길 바라며, 9월 회고도 곧 써야 할 것 같다 (...)