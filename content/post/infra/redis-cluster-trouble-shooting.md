---
title: "좌충우돌 서버 인프라 구축기"
date: 2023-06-04T19:48:07+09:00
draft: false
---

개발 환경에 필요한 서버 인프라를 구축하면서 경험한 과정 정리
<!--more--> 

#
### AWS 완전관리형 서비스 → EC2 자체 구축
프로젝트에서 레디스와 엘라스틱 서치를 AWS의 완전관리형 서비스를 사용하고 있었다. 운영 환경에서는 이 완전관리형이 필요했지만, 개발 환경에서는 굳이 완전관리형 서비스를 사용할 필요가 없었다.
AWS 완전관리형 서비스에서 변경하려고 했던 이유는 다음과 같다.
> 1. 개발 및 테스트용 서버에서는 AWS의 완전관리형이 필요하지 않고, 기본적인 인프라 구축만 되어 있어도 괜찮다.
> 2. EC2에 직접 띄웠을 때와 완전관리형을 비교해보면 꽤 비용 차이가 있다.

물론 부담은 좀 크게 있었던 이유가 인프라 담당자가 없는 상태였고, 나 또한 인프라가 아닌 백엔드 기능개발을 주로 하고 있기 때문에 인프라 구축에 부담이 있었다.
그래서 구축하면서 좌충우돌했지만 그 기록을 정리해보고 인프라를 구축한 과정을 정리해보려고 한다.

<br>


### AWS 비용 파악 및 비교하기
현재 aws 완전관리형을 사용하고 있는 비용은 대략적으로 이러했다.
<div style="text-align:center">
    <img src="/images/infra/redis-cluster/aws-calculator.png" alt="aws-calculator" />
</div>

<br>
<br>

그리고 만약 바꾸게 된다면 대략 이정도 금액이 발생할 것으로 예상되었다.

<div style="text-align:center">
    <img src="/images/infra/redis-cluster/aws-calculator-2.png" alt="aws-calculator" />
</div>

<br>

이렇게 비용을 보더라도 완전관리형 서비스를 사용하기보다는 EC2에 자체 구축을 하면 좋을 것으로 예상되었다.

<br>

### 인프라 구축: docker-compose 사용하기
기존에 로컬 서버에도 레디스와 엘라스틱 서치, 키바나를 docker-compose로 구축해본 경험이 있긴 했다. 
물론 그때는 지금처럼 많은 고민을 하진 않았고, 레디스도 클러스터 모드가 아닌 단일 노드인 standalone으로만 구축해봤었다.
그래서 구축 내용에는 차이가 있겠지만, 가장 간단하게 구축이 가능하면서도 내가 아닌 다른 사람들도 인프라를 만질 때 부담이 없을 법한 방법으로 선택하게 되었다.

<br>

### CI/CD: AWS Codepipeline + Elastic Beanstalk 사용하기
CI/CD에 많은 고민을 했다. 내가 CI/CD를 구축하면서 신경썼던 부분은 아래와 같다.
> 1. 자동화할 수 있도록 한다.
> 2. 다른 사람들이 내가 만든 인프라를 만질 때 부담이 없도록 한다.

그동안 이렇게 진행해본 적이 없었는데, 특히나 1번의 경우는 더 신경쓰지 못했던 부분이었는데 사람이 직접 만지기보다는 자동화할 수 있는 것이 좋다고 생각했다.
또 무엇보다 내가 구축해놓은 인프라 코드를 만지기 부담스러워하지 않고, 쉽게 만질 수 있도록 하는 것을 중요하게 생각했다.

그래서 내가 고려했던 방법은 여러가지가 있었다.  

CI로는 Github Action, AWS CodeBuild가 있었다. 
여기서는 무난하게도 AWS CodeBuild를 선택했는데, 개인적으로 나는 아직 Github Action이 편하지만, **회사 프로젝트에서 AWS Codepipeline를 사용하고 있었기 때문에** 그에 맞춰서 선택했다.

그리고 CD로는 AWS CodeDeploy를 통해 AWS Elastic Beanstalk, AWS ECS, Cloudformation의 방법을 고려했다. 여기서 좀 갈등이 있었다.
내가 찾아본 각 CD의 장단점은 아래와 같았다.

1. AWS Elastic Beanstalk
> 장점: 오류가 있으면 롤백이 쉽고 현재 회사에서 사용하고 있는 배포 방식이라 누구나 쉽게 배포가 가능하다.  
> 단점: docker-compose에 최적화된 CD는 아닌 것으로 보였다. docker-compose.yml 파일명을 벗어나면 Dockerrun.aws.json 파일로 제공해야 해서 번거롭다.

2. AWS ECS
> 장점: Elastic Beanstalk에서 ECS로 배포가 가능했다.  
> 단점: Elastic Beanstalk의 한계가 같이 묶일 수 밖에 없다. 단독으로 ECS 구축을 하기에는 아직 이해가 부족했으며, 롤백이 쉽지 않아 부담이 있는 배포 방식이었다.

3. Cloudformation
> 장점: 코드로 인프라를 관리할 수 있어 변경 관리에 용이하다.  
> 단점: 스택 생성 혹은 업데이트 시 오류가 있으면 롤백은 되지만 AWS CodeDeploy가 자동화로 진행되지 못한다. 다시 스택을 재생성해야 하기 때문에 부담이 있다.

위의 장단점들을 고려해봤을 때 결국 가장 좋다고 판단한 배포 방식은 **AWS Elastic Beanstalk**이었다. 
<U>회사 내부에서 가장 익숙한 배포 방식이고, 파일명의 경우 docker-compose.yml로 하면 문제는 없었기 때문이다.</U>

파일명을 원래는 docker-compose-dev.yml로 지정하고, local 파일을 docker-compose-local.yml로 지정하려 했지만 dev의 경우 docker-compose.yml로 지정해서 사용하는 것으로 택했다.
AWS Elastic Beanstalk에서 플랫폼이 docker인 경우 docker-compose.yml를 docker-compose up -d라는 명령어로 자동으로 실행하기 때문에, 파일명이 다르면 인식 오류가 발생하면서 Dockerrun.aws.json 파일을 제공해달라고 메시지가 나와 어쩔 수 없었다.

그래서 이렇게 CI/CD 방식을 선택했다. **최종 결정은 AWS Codepipeline + AWS Elastic Beanstalk이었다.**

<br>

### EC2 인스턴스 선택하기
EC2 인스턴스를 선택하는 것도 고민이었다. 내가 docker-compose를 통해 띄우는 서비스는 여러 서비스들이고, 특히 메모리를 많이 사용하는 서비스들이기 때문이었다.
테스트용이긴 해도, 이 서버가 이상이 생기면 서버 어플리케이션 동작에도 문제가 생기기 때문에 이 부분에서는 비용이 조금 들더라도 안정성을 고려했다.

레디스 클러스터 모드, 엘라스틱 서치 단일 노드, 키바나가 동시에 뜨면서 도커가 실행되어야 하는 환경이고, 부가적으로는 자바를 설치해주어야 했다. 
이런 것들을 고려했을 때 프리티어는 당연히 불가했다.
고려했던 EC2 인스턴스 종류는 t3.small과 t3.medium이었다.

애매하게도 메모리 2GB로는 약간 부족하거나 딱 맞을 것 같았고, 4GB로는 넉넉하게 남을 것 같다는 판단이었다. 결국 **t3.medium**으로 선택했다.
생각보다 small과 medium은 비용 차이가 좀 있기 때문에 선택에 고민이 많이 되었지만, 인프라 서버가 안정적으로 동작하는 것이 더 중요하다고 생각했기 때문에 medium으로 선택했다.

<br>

### docker-compose 파일 작성하기 - Elastic Search, Kibana
docker-compose 파일은 로컬 서버에 구축했던 것과 비슷하게 작성했다. 특히 Elastic Search와 Kibana는 많이 비슷했다. 다만 고려했던 점이 있다면 아래와 같다.
> username, password 설정  
> healthcheck 설정  
> depends_on 설정

username, password 설정은 로컬 서버에서는 필요가 없었지만, 개발 서버에서는 이 부분을 적용하도록 추가해주었다.
그리고 docker ps 로 컨테이너 상태를 바로 확인할 수 있게 healthcheck를 추가해주었다.

간단하게 위 내용들을 코드로 보면 아래와 같다.
```yaml
  elastic-search:
    environment:
      - discovery.type=single-node
      - xpack.security.enabled=true
      - ELASTIC_USERNAME=${ELASTICSEARCH_USERNAME}
      - ELASTIC_PASSWORD=${ELASTICSEARCH_PASSWORD}
    command:
      - /bin/bash
      - -c
      - |
        export ES_JAVA_OPTS="-Xmx512m -Xms512m"
        exec /usr/local/bin/docker-entrypoint.sh
    networks:
      - elastic-search-bridge
  kibana:
    environment:
      - ELASTICSEARCH_HOSTS=http://elastic-search:9200
      - xpack.security.enabled=true
      - ELASTICSEARCH_USERNAME=${ELASTICSEARCH_USERNAME}
      - ELASTICSEARCH_PASSWORD=${ELASTICSEARCH_PASSWORD}
    command:
      - /bin/bash
      - -c
      - |
        until $$(curl --output /dev/null --silent --head --fail -u ${ELASTICSEARCH_USERNAME}:${ELASTICSEARCH_PASSWORD} http://elastic-search:9200); do
          printf '.'
          sleep 5
        done
        exec /usr/local/bin/kibana-docker
    networks:
      - elastic-search-bridge
    depends_on:
      - elastic-search
networks:
  elastic-search-bridge:
    driver: bridge
```

그리고 로컬 서버에서는 설정을 모르고 못해줬던 부분이 kibana는 elastic search가 실행된 후 실행되어야 오류가 없이 실행될 수 있기 때문에 <U>depends_on의 설정</U>을 해주었다.
<U>depends_on은 시작의 순서는 줄 수 있지만 완전한 실행 후에 실행되는 것은 아니기 때문에 healthcheck를 통해 확인해준 후 실행될 수 있도록 로직을 추가했다.</U>

다른 부분은 로컬에서도 단일 노드로 구축하면서 JVM 설정을 함께 해주었기 때문에 큰 차이는 없었다.
다만 잘 모르고 있다가 알게 된 점은 **JVM 설정은 Elastic Search 서비스가 돌아가는 메모리의 50%를 설정해주는 것이 좋다는 점**이었다.

<br>

### docker-compose 파일 작성하기 - Redis
레디스가 기존 로컬 서버에서와 달리 클러스터 모드로 생성해주어야 했다. 기존 개발 서버에서도 클러스터 모드로 사용중이었으며, 현재 프로젝트의 스테이지는 로컬 - 개발 - 운영이다.
그러니까 개발 스테이지 다음 바로 운영 스테이지이기 때문에 **최대한 운영 서버와 비슷한 환경으로 구축해주어야 한다고 생각**했다.

그래서 클러스터 모드로 만들어주기 위해 클러스터 생성에 필요한 최소 3개의 마스터 노드로 구성하고, 외부에서 접근이 가능하도록 ip와 port를 지정해주었다.
또한 같은 vpc를 사용하고 있는 ec2에 연결할 것이기 때문에 ec2의 private ip로 생성해주었다.

여기서 잘 몰랐던 점은 도커 네트워크였다. 
<U>도커 네트워크와 ec2 네트워크는 별개</U>이기 때문에, ec2의 private ip로 다른 ec2에서 연결하여 사용하려면 도커 네트워크로 생성되지 않고 ec2 네트워크로 생성해줄 수 있게 해주어야 한다는 점이다.

그래서 docker-compose에 network를 bridge로 사용하게 설정해주고, ports를 각 노드들마다 독립적인 포트들로 열어주었다.
그리고 redis.conf를 각 노드마다 별도로 설정해주면서 cluster-announce-ip와 cluster-announce-port, cluster-announce-bus-port를 ec2의 private ip로, 그리고 각 노드의 port, bus-port로 설정해주었다.

간단하게 해당 부분에 대해 코드로 작성하면 아래와 같다.
```yaml
redis-master-node-1:
  image: redis:6.0.9
  container_name: redis-master-node-1
  ports:
    - 6379:6379
    - 16379:16379
  command: redis-server /usr/local/etc/redis/redis.conf
  volumes:
    - ./redis/redis-1.conf:/usr/local/etc/redis/redis.conf
  networks:
    - redis-bridge

redis-master-node-2:
  image: redis:6.0.9
  container_name: redis-master-node-2
  ports:
    - 6380:6380
    - 16380:16380
  command: redis-server /usr/local/etc/redis/redis.conf
  volumes:
    - ./redis/redis-2.conf:/usr/local/etc/redis/redis.conf
  networks:
    - redis-bridge

redis-master-node-3:
  image: redis:6.0.9
  container_name: redis-master-node-3
  ports:
    - 6381:6381
    - 16381:16381
  command: redis-server /usr/local/etc/redis/redis.conf
  volumes:
    - ./redis/redis-3.conf:/usr/local/etc/redis/redis.conf
  networks:
    - redis-bridge

networks:
  redis-bridge:
    driver: bridge
```

중요한 부분은 **ports**와 **network**이다. <U>특히 ports는 제대로 열어주지 않으면 외부 ip에서 해당 port로 접근할 수 없어 클러스터 구성이 제대로 되었더라도 연결될 수 없다.
network의 경우에도 bridge로 열어주지 않으면 클러스터 구성이 제대로 되지 않을 수 있다.</U>  

다음은 redis.conf 파일에 대한 설정이다.
```conf
bind 0.0.0.0
port 6379
cluster-enabled yes
cluster-config-file node.conf
cluster-node-timeout 5000
appendonly yes
protected-mode no
cluster-announce-ip ${ec2-private-ip}
cluster-announce-port 6379
cluster-announce-bus-port 16379
```

다른 노드들은 port, cluster-announce-port, cluster-announce-bus-port만 docker-compose.yml에 설정한 것과 동일하게 각각 만들어주면 된다.
**여기서 중요한 점은 bind, cluster-announce-ip, cluster-announce-port, cluster-announce-bus-port를 꼭 설정해주어야 한다는 점이다.**
<U>그렇지 않으면 클러스터가 제대로 구성될 수 없다. 각 노드들이 서로를 찾지 못하기 때문이다.</U>

bind의 경우는 모든 ip가 접근이 다 가능한 상태인데, 다 열어주는 것이 좋은 것만은 아니지만 redis-stat으로 클러스터 모니터링을 할 예정이었기 때문에 우선은 다 열어주었다.
개발, 테스트 목적이어서 다 열어주었지만 <U>운영 환경에서는 필요한 ip만 열어주고 password를 설정해주는 것이 좋다고 한다.</U> 현재 개발 서버에서는 해당 부분을 설정하지 않고 진행했다.

또한 여기서 **볼륨을 설정해준 점이 특징**이다. <U>각 노드들에 대한 설정 파일을 도커 컨테이너에서 인식할 수 있어야 하기 때문에 설정 파일을 마운트해주는 작업을 진행했다.</U>
코드는 일부 코드만 보여주고 있지만 볼륨에는 각 컨테이너들에 대한 데이터들을 저장할 수 있게 할 수도 잇다. 나의 경우는 <U>마운트한 데이터들을 ebs에 저장하도록 설정해주면서 데이터를 보존할 수 있게</U> 해주었다.

이렇게 다 구성을 해주고 나면 cluster create 명령어로 생성해주고, cluster info 명령어로 클러스터가 제대로 구성되었는지 확인해주면 된다.
```bash
#!/bin/bash
redis-cli --cluster create ${ec2-private-ip}:6379 ${ec2-private-ip}:6380 ${ec2-private-ip}:6381 --cluster-yes
```
```bash
#!/bin/bash
redis-cli -h ${ec2-private-ip} -p 6379 cluster info
```

이렇게 하면 클러스터 구성은 끝났고, 외부 ec2에서 레디스의 클러스터에 접근할 수 있도록 설정하면 된다. 나는 spring application에 연결할 것이기 때문에 spring application에서 접근할 수 있도록 설정해주었다.
```yaml
spring:
  redis:
    cluster:
      nodes:
        - ${ec2-private-ip}:6379
        - ${ec2-private-ip}:6380
        - ${ec2-private-ip}:6381
```

이렇게 하면 스프링 어플리케이션에서 레디스 클러스터에 접근할 수 있게 된다.

<br>

### 각 서비스 모니터링하기
각 서비스들을 어떻게 모니터링해줄 수 있지를 고민하다가 현재는 **레디스의 경우 redis-stat**으로 간단하게 모니터링 할 수 있게 설정해주었고, **엘라스틱 서치의 경우 키바나**를 통해 모니터링 할 수 있게 설정해주었다.
추후에는 현재 기본으로 구축된 prometheus와 grafana를 통해 모니터링 할 수 있도록 설정해주려고 한다.

<br>

### 여러 삽질과 경험한 소감
글로 정리하고 보니 빨리 할 수 있었을 것 같은데, 실제 이걸 구축할 때는 여러 삽질들을 했다. 특히 레디스 클러스터를 만들어줄 때는 ip와 bus port에서 혼선이 있었다.
그리고 도커 네트워크에도 많이 어려움을 겪었다. 그래서 설정 파일을 몇번이나 테스트했는지 모르겠다 (...)

그래서 되돌아보면 더 공부해야 할 부분들을 찾을 수도 있었고, 다음에는 더 잘 할 수 있을 것 같다는 생각이 들었다.
그리고 여전히 어려웠던 점이라면 나는 스타트업에서 일하고 있고 규모가 작기 때문에, 아직 인프라를 전문으로 하는 분이 없어서 어디까지 해야 적당할지 판단하는 것이 어려웠다.
최대한으로 구축한다고 해보았지만 어려웠다. 인프라의 세계란 (...)

그리고 그동안은 AWS 인프라를 만지는 것에 대한 두려움이 컸다. 
현업에 와서 인프라에 대한 경험이 좋지 않았기 때문에 피하고 싶은 마음이 컸는데, 이번 인프라를 구축해보면서 인프라에 대해 더 공부하고 싶어졌고 두려움도 조금은 떨쳐낼 수 있었던 것 같다.

백엔드 개발자로 기능 개발 뿐 아니라 인프라에 대한 지식도 많이 쌓으면서 인프라에 대한 경험도 두려워하지 말고 많이 해봐야겠다는 생각이 들었다.