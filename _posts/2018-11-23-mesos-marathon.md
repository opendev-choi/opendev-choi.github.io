---
title: "mesos marathon"
date: 2018-11-23 17:10:00 -0400
categories: open-source
---

## 참고
> [https://www.slideshare.net/AhmedBACHA1/cloud-infrastructure-on-apache-mesos] [1]
> [http://devbv.com/2017/04/12/mesos/] [2]

## intro
서비스를 사용할때 AWS 처럼 서비스 확장이 필요할떄, 혹은 서비스 서버중 한대가 죽더라도 서비스는 계속되어야 할 때 쓸만한 기술들을 정리해 보았습니다.

## mesos
mesos 는 기존의 여러대의 서버를 사용하였을 경우 90%, 10%, 10% 와 같이 리소스 사용중일 때 좀더 유동적으로 프로세스를 배분하여(Elastic Sharing) 사용할수 있게 해주는 apache 의 오픈 소스 프로젝트중 하나입니다.

여러 서버를 관리하기 쉬워지므로 많은 IT기업(airbnb, twitter, apple, netflix 등)에서 사용중이며, 계속 발전중인 오픈소스 프로젝트입니다.

![no_support_completion](/assets/img/mesos.jpg)

## marathon
marathon 은 mesos 환경에서 실행되는 데몬 스케쥴러 프로젝트이며 이를 이용하면 서버 3개 운용중일시 1개 서버가 죽더라도 스케쥴러가 나머지 2개 서버에 배분하여 무중단 서비스를 운용할수 있게끔 하는 프로젝트입니다.

## installation
설치 자체는 zookeeper 가 선행 설치되어 있어야 mesos 및 marathon 을 설치할수 있으며, mesos 는 centos7 기준 build 설치 필요, marathon 은 tar 만 풀면 바로 사용할수 있게끔 되어 있다.

설치 참고 링크
> [http://mesos.apache.org/documentation/latest/building/] [3] -- 아파치공식 mesos 빌드 가이드 문서
> [https://mesosphere.github.io/marathon/docs/] [4] -- marathon 설치 가이드 문서

mesos 설치시 tar 파일을 받아 압축을 푼 뒤 build 폴더를 만들고 configure 후 make && make install 을 해주면 build 폴더 안에 bin 폴더가 생기게 됩니다.

marathon 설치 가이드 문서에서는 ./bin/start 라고 되어 있으나, 실제로는 해당 파일을 찾지 못하고 tar 압축을 푼 경로의 /bin/marathon 파일을 통하여 실행시킬수 있습니다.

## configure
정상적으로 설치하게 되면 다음과 같은 화면이 보이게 되며 

![no_support_completion](/assets/img/marathon_dashboard.jpg)

이중 create 버튼을 누르게 되면 서비스를 생성할수 있습니다.

![no_support_completion](/assets/img/marathon_dashboard_2.jpg)

실로 간단합니다! 식별할 ID, CPU 코어수, 메모리, DISK 사용량, 몇개 서버에서 실행할것인지만 세팅하고 명령어를 쳐주면 알아서 할당되고, 알아서 실행되게 됩니다.

![no_support_completion](/assets/img/marathon_dashboard_3.jpg)

이와같이 Application 의 Scale을 단 버튼 하나로 쉽게 조절할수 있습니다.

## chronos
그 외에도 mesos 위에 올리는 여러 서버에서 자동으로 실행되는 chronos 라는 프로젝트도 존재합니다.

## 결론?
많은 IT 기업에서 사용하는걸로 이미 충분한 검증이 됬다고 생각합니다.
무중단 되어야 할 서비스, 혹은 대규모 단위 cronjob 을 할때는 충분히 사용할만 하다고 생각합니다.

만약 무중단 혹은 Scale up/down 이 쉬워야 하는 서비스를 개발하게 될 경우 저는 무조건 mesos-marathon 을 사용하게 될 것 같습니다.


<!-- 주석 및 참고 링크입니다. -->
[1]: https://www.slideshare.net/AhmedBACHA1/cloud-infrastructure-on-apache-mesos
[2]: http://devbv.com/2017/04/12/mesos/
[3]: http://mesos.apache.org/documentation/latest/building/
[4]: https://mesosphere.github.io/marathon/docs/