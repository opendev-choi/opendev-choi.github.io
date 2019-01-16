---
title: "배포 프로세스 없는 회사 사원의 Docker 사용기"
date: 2018-01-16 15:00:00 -0400
categories: open-source
---

## Introduce Docker
여태까지 업무중 상당량은 사실 테스트 서버와 실서버의 환경이 달라(OS, Version, 라이브러리 참조 등) 생기는 문제를 해결해야 하는 경우가 많았습니다.

특히나 라이브러리 혹은 잘쓰고 있던 cent6.7 -> cent7.x 버전으로 올려할 때도 문제가 생기고, tar 로 묶은 배포판을 만들더라도 나중에 설치할때 문제가 생기기도 합니다.

또한 최신 패키지를 사용하기 위해서는 Kafka, Kudu, Impala 와 같은 서비스를 설치해야 하는데 사실 이 패키지를 설치하고, 설정하고, 테스트까지의 과정이 적지 않은 시간이 들어갑니다.

사실 해결법은 간단합니다. OS 자체를 가상화시켜 실서버 위에 올리면 만사가 해결됩니다.

귀찮은 라이브러리 참조도, 똑같이 깔았는데 실행이 안되는 서비스도 가상화시켜 올리면 해결되기 때문입니다.

다만 문제는 OS 전체를 가상화 시키기 위해서는 용량이라던지 사실 너무 무거운것이 사실입니다.

그런 발상에서 나온 서비스가 바로 Docker 로, 모든 서비스를 Docker 를 통하여 가상화 시켜놓으면 실서버에서는 Docker를 통해서 Deploy 시키기만 하면 테스트 환경과 동일하게 사용이 가능합니다.


## In Our Company case
현재 필자가 다니는 회사에서는 엔진을 배포할때 tar 로 묶어 배포중입니다.

물론 다른 필요한 서비스(Kafka, Zookeeper, Elasticsearch 등) 은 모두 rpm 혹은 sh 로 묶어 있습니다.

이는 개발자의 스트레스와 

여태까지 많은 실패(실제 사이트 설치시)를 겪어 왔기에 더이상은 이런 문제가 발생하면 안된다고 생각하여 Docker 를 도입하기 위해서 도입을 준비중에 있습니다.


## How to build docker
도커는 빌드하는 방법이 상당히 쉽게 되어 있습니다.

Dockerfile 을 마치 순정 OS 에서 해당 서비스를 설치하듯이 명령어를 입력후 docker build 하나면 이미지를 만들 수 있습니다.

현재 kudu 를 이미지로 만들때 쓴 스크립트를 기준으로 설명드리자면,

```
# 현 도커 빌드 디렉토리의 구조
#.
#├── conf
#│   ├── master.gflagfile
#│   └── tserver.gflagfile
#├── Dockerfile
#├── kudu_service.sh
#└── repo
#    ├── cloudera.repo
#    └── mariadb.repo

# 어떤 이미지를 기반으로 빌드할것인지를 설정합니다.
# 현재는 centos 의 최신 버전을 사용하는것으로 되어 있습니다.
# 그 외에도 Java, Ubuntu, Python 등 다양한 환경에서 실행할수 있습니다.
FROM            centos:latest 
# 작성자와 같은 의미입니다. 큰 의미는 없습니다.
MAINTAINER      choijh@hamonsoft.co.kr

# 명령어를 실행합니다.
# kudu 설치를 위하여 repository 를 추가해줍니다.
RUN             yum-config-manager --add-repo https://archive.cloudera.com/cm6/6.1.0/redhat7/yum/cloudera-manager.repo

RUN             mkdir /home/service

# repo 파일들을 yum repo 폴더에 추가해줍니다.
# ADD 명령어는 외부(작업자 PC) 의 파일을 도커 이미지 내부로 넣어주는 역할을 합니다.
ADD             repo/* /etc/yum.repos.d/
ADD             kudu_service.sh /home/service/

# yum 을 이용하여 kudu 를 설치합니다.
# 따로 rpm 을 사용하지 않고도 설치 가능합니다.
RUN             yum install -y kudu-master
RUN             yum install -y kudu-tserver

ADD             conf/* /etc/kudu/conf/

# 외부로 연결해줄 포트를 지정합니다.
EXPOSE          8051
EXPOSE          8050
```

해당 스크립트와 같으며, 만약 시스템 구축을 해봤다면 난이도는 아주 쉽게 docker 를 build 할 수 있으며, 실행 자체도 아주 간단하게 

> docker run -d --name kudu_service -p 8051:8051 -p 8050:8050 -p 7051:7051 kudu /home/service/kudu_service.sh

와 같이 아주 쉽게 실행할수 있습니다.


## Air Gap Network
dockerfile 을 보시고는 이런 의문을 가지실수 있습니다.

> 음... 아무래도 패쇄망에서는 docker를 이용하지 못하지 않을까? 스크립트에서는 yum을 이용해서 설치하잖아?

사실 이는 쉽게 생각하자면 VMware 의 복사 기능을 생각하시면 답이 나옵니다.

만약 인터넷이 연결된 VM에서 프로그램을 설치한 뒤 그 VM 을 그대로 옮기면 이미 프로그램은 설치되었기때문에 전혀 문제없이 프로그램을 사용할수 있습니다.

docker 도 이와 같습니다.

dockerfile 은 이미지를 생성하기 위한 파일로써 이미지를 실행시켰을때 실행시키는 명령어를 모아놓은게 아닌, 빌드시에 이미 한번 생성하여 스냅샷을 찍은 뒤,

이미지를 실행시킬때 스냅샷을 불러와 실행시키게 됩니다.


## Any Disadvantages?
말만 들어보면 거의 사용을 안할수 없는 서비스입니다. 그렇다면 도커의 단점은 없을까요?

제가 기존의 패키지를 docker화 시켜본 경험으로써 느껴지는 단점은 다음과 같습니다.

1. 기존보다 환경에 대한 제약은 없어지나, 실제 설치하는 엔지니어가 도커에 대한 지식이 있어야 함
    - 필자가 다니는 회사는 연구소에서 개발하여 현장 엔지니어 분들이 설치/설정을 하게끔 되어 있습니다. 패키지를 빌드하는것까지는 개발자의 일이나 실 사이트에 나가게 되는 엔지니어의 관점에서는 외부로 통신되는 port, 혹은 각종 로그 config들의 경로등을 지정해주어야 합니다. 이는 엔지니어에게 어느정도의 docker 지식이 있어야 가능한 일이므로 엔지니어 또한 docker 의 지식을 가지고 있게끔 하여야 합니다.
2. tar 혹은 sh 보다 빌드하기 어려울수 있음
    - 이 단점은 docker 가 완전한 가상화가 아니라는것에서 기반합니다. 동일 centos 환경에서 빌드되나 docker 의 몇몇 특징(네트워킹, service 실행) 등 개발자가 알아햐 할 사항이 늘어납니다.


## conclusion
하지만, 사실 이런 단점들은 충분한 교육, 학습으로써 극복될수 있는 문제입니다.

현재의 일관성없는 배포, 환경을 타는 패키지, 설치에 시간을 뺏기는 문제 등을 docker 도입으로써 모두 해결할수 있기 때문에 도입을 긍정적으로 검토하고 있으며,

기존 패키지들을 docker로 묶기 위해서 고군분투 중입니다.

