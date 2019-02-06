---
layout: archive
permalink: /about/
title: "Choi JongHyeok"
author_profile: true
---

![no_support_completion](/assets/img/41746152.jpg)

### Choi JongHyeok

birth : 1998.11.13

Collection Engine Developer. 

(주) 하몬소프트, 코어 연구팀

---

### Specialized in
Python, C++, Crawling, stabilization Damon

### Personal interests
DevOps, Code Perfect, Pythonic, Geek Codes, Bowling

### Hobby
Bowling, bicycle

### Dislike things
Bitter coffee, dirty codes

---

### Education

서울여자대학교 정보보호 교육원 — 200h, 수료

`2014.01 ~ 2015.11`
- 정보보호 관련 강의, 해킹 사례 분석

Exam SQL 튜닝의 시작 — 8h, 수료

`2018.04.19`
- SQL 튜닝 - SQL 계획 최적화, SQL 계획 분석

효율적인 R&D 를 위한 테스트 계획/전략, 금천구 — 7h, 수료

`2018.11.20`
- 테스트 방법론

### Skills

Python
- c++ 솔루션 python 리팩토링
- twisted를 사용한 네트워크 통신 모듈 개발
- rest api server/client 개발
- SSH, telnet 등 cli 데이터 파싱/분석 모듈 개발
- Elasticsearch/kafka 를 사용한 수집엔진 개발
- Google Protocol buffer 를 사용한 통신규격 개발

C++
- snmp를 사용한 네트워크 통신 개발
- Google Protocol buffer 를 사용한 통신규격 개발

tech
- mesos, marathon 을 통한 다중 서버 구성
- monit, cron 등을 이용한 linux 서버 환경 구성
- 다중 서버 수집 구조를 적용한 수집 프로세스 개발

---

### Career
(주) 하몬소프트 코어 연구팀 
`2016.10.4 ~`
- SNMP 를 이용한 서버/회선 성능 수집 엔진 개발
- CLI 사용한 IPT 장비 상태/성능 수집 엔진 개발
- sFlow, netFlow 등을 통한 Traffic 정보 수집 엔진 개발
- Rest API 를 통한 외부 연동 API 엔진 개발
- 실시간 로그 감시 엔진 개발
- c++ 솔루션 python 리팩토링

---

## Projects

### 네트워크 관제 솔루션
`2016.10 ~`
- 네트워크 장비 (Switch, Router) 회선 BPS, PPS 및 장비성능 정보 수집
- Trap, Syslog 및 장비 메세지 수집 및 이벤트 탐지
- 네트워크 장비 (Switch, Router) Config 정보 수집 및 변경탐지
- SNMP와 ICMP 를 통한 장비 상태 및 장비 회선 상태 장애탐지
- 수집 및 주기 관리 엔진 C++ -> Python 리팩토링
- C++ 엔진에서 발생하던 Handle, Resource 문제 해결
- 빌드 환경 통합을 위한 C++, Python 빌드 서버 구축

주로 버그수정 유지보수보다는 신규 개발, 개선 작업을 많이 하였으며,
빌드 환경등이 일관화되어 있지 않아 일관화 작업을 많이 하였고,

기존 엔진들의 고질적인 문제를 해결하기 위하여 C++ 구조에서 Python 으로 리팩토링 작업을 주로 하였습니다.


### 분할 수집 NMS 솔루션
`2018.04 ~`
- BigData Platform을 이용한 NMS 수집
- Kafka 를 통한 프로세스간 통신
- Kafka Partition 을 이용한 시스템 Scale Out 구조
- Google Protocol Buffer를 사용한 데이터 프로토콜
- ElasticSearch 를 통한 데이터 분석/관리
- RDBMS 속도 개선을 위한 E/S 사용
- 네트워크 장비 9,990대, 회선 150,000 회선 수집

기존의 패키지 구조가 대량의 데이터를 수집할 시에 단일 서버로 수집하게 되어 있어 대량의 데이터를 수집하지 못해 기존의 솔루션을 사용하지 않고 새로운 구조의 솔루션을 개발하였습니다.

실제로도 마이크로서비스 형태의 구조로 되어 있어 ScaleOut 이 용이한 모양이며, 여러개의 서비스가 올라가다 보니 Kafka 를 통하여 메세지를 주고받는 형태입니다.

### 트레픽 관제 솔루션
`2016.10 ~`
- NetFlow v9, sFlow v5 수집 경험
- Source IP, Destination IP, ProtoCol, Source Port 등 트래픽 기본 정보 수집

Router 에서 보내주는 FLow 를 통하여 트래픽의 세부 정보를 분석하여 저장하는 솔루션입니다.


### 교육청 네트워크 관제 사업
`2016.10 ~ 2017.06`
- 다중 계층 구조(본청 - 지원청)로 인한 데이터 분할 수집 구조
- 다중 계층간 데이터 동기화 기능 구현
- 경기, 경남, 경북, 전북 등 시도 시스템 통합 구축
- 타 교육기관과 연계하여 데이터 동기화 기능 구현
- Tcp port 응답시간/감시 엔진 개발

네트워크 구조가 복잡하게 되어 있고(4층 깊이의 Tree 형태) 서로간의 데이터 동기화가 얽혀있어 복잡한 구조였습니다.


### 쿠팡 종합관제
`2017.12 ~ 2018.05`
- 콜센터 녹취장비 수집을 통한 콜센터 상태 감시 엔진 개발
- VoiceGateWay 수집을 통한 장애감시 엔진 개발
- WatchDog 기능을 통한 로그 장애감시 엔진 개발
- 기타 네트워크 장비 CLI 수집을 통한 정규식 Parsing 엔진 개발

기존 패키지 내용보다는 추가로 개발되는일이 많아 CLI Parsing 할때 정규식을 많이 사용하였고, LOG 감시 자체도 실시간으로 감시하여 개발이 생각보다 많이 들어간 프로젝트였습니다.


### 소프트뱅크 Repeater 성능 수집 시스템 개발
`2017.01 ~ 2017.04`
- SMPP 프로토콜을 통한 통신 제어 구현
- SMPP 프로토콜을 통한 실 Repeater 통신 프로토콜 구현
- Repeater 사용률, 상태 기반의 이벤트 표현 및 소프트 뱅크 시스템 연동
- Repeater Firmware 원격 업데이트 엔진 개발

결과가 좋은 프로젝트는 아니었지만 일본 현지에서의 프로젝트로 Repeater 펌웨어와 API 를 맞춰보는 프로젝트였습니다.


### KT DDOS 안정화 작업
`2018.12 ~ 2019.02`
- Mesos - Marathon 기반의 Active/Standby 환경 구성
- Kudu Impala 기반의 대량 데이터 처리
- Kudu Impala Active/Standby 환경 구성

이 프로젝트는 제가 전부 개발한 사항이 아닌 시스템이 불안정하여(주기적으로 종료, 장애시 복구 오래걸림) 참여하게 되었던 프로젝트입니다.
모든 서비스들이 Active/StandBy 형태로 구축되어야 하기 때문에 난이도 있는 작업이었습니다.