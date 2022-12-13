---
title: "사이드 프로젝트 개발환경 셋팅하기"
date: 2022-12-13T23:35:17+09:00
tags: ['python', 'side-project']
draft: false
---
django 프로젝트 m1에서 셋팅하기! (feat. m1 오류와 싸우기)
<!--more--> 

#
### Python 버전 맞추기
현재 프로젝트에서의 버전은 3.6.15라고 적혀 있었다. 버전은 통일하면 좋고, 버전차이로 안되는 부분이 생길 수 있으니 맞춰주었다!
python의 여러 버전 관리를 위해서 pyenv를 설치하고, python 버전을 설치해주었다.  

mac os의 M1환경에서 homebrew가 설치되어 있는 환경에서 이렇게 진행했다.
```
brew install pyenv
```

다음 pyenv가 활성화될 수 있도록 설정한다. (.zshrc인 경우, bash인 경우는 .bash_profile)
```
echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\n  eval "$(pyenv init -)"\nfi' >> ~/.zshrc
```

적용될 수 있도록 아래의 명령어를 이어 실행한다.
```
exec "$SHELL"
```

이제 pyenv 사용할 준비는 끝났으니 pyenv로 원하는 버전을 설치해주면 된다!
```
pyenv install 3.6.15
```

global 커맨드로 사용할 버전을 시스템 설정으로 변경해준다.
```
pyenv global 3.6.15
```

python -V 로 버전을 확인해보면 적용된 것을 확인할 수 있다.

#
### django - postgres 연결
연동을 위해 postgresql을 설치한다.
```
brew install postgresql
```

m1의 경우 postgresql을 설정하고 django 서버를 실행해보면 오류를 만난다.
```
Psycopg2 error: Symbol not found _PQbackendPID
```

검색해보니 재설치하라는 이야기가 있었다. m1에서는 제대로 호환되지 않는 것인지(?) 이 오류가 나오는 것 같았다.
그래서 uninstall을 진행했다.

```
pip uninstall psycopg2
```

그리고 다시 m1에서 잘 호환되는 방식으로 설치했다.
```
brew install libpq --build-from-source
brew install openssl

export LDFLAGS="-L/opt/homebrew/opt/openssl@1.1/lib -L/opt/homebrew/opt/libpq/lib"
export CPPFLAGS="-I/opt/homebrew/opt/openssl@1.1/include -I/opt/homebrew/opt/libpq/include"

pip3 install psycopg2
```

이렇게 하니까 정상적으로 동작했다!


#
### python - django 서버를 오랜만에 셋팅하면서
python은 늘 가상환경 설정이 헷갈린다. 
pycharm을 쓰고 있어서 알아서 해주는 부분이 많기는 하지만!  
DB 연결과 Docker 설정은 어렵지 않게 끝냈다.  

이 때 또 느낀 건 프로젝트 리드미에 프로젝트 설정에 대해 잘 적어두어야 한다는 점이다.
어떻게 보면 당연한건데, 시간이 부족하면 잘 업데이트 되기 어려운 부분이기 때문에 지나치기가 쉽다.  
이번에 기존에 계시던 백엔드 개발자분께서 정리를 잘해주셔서 크게 어렵지 않게 셋팅할 수 있었다!  

-프로젝트 셋팅 이야기 끗-