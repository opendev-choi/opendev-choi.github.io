---
title: "Kudu Vm Error "
date: 2018-11-16 10:40:00 -0400
categories: kudu
---

## intro
회사의 프로젝트로 KUDU 로 여러가지 테스트를 위하여 뒤적거리다가 완전히 설치된 KUDU 가 깔려있는 서버를 4개 복제하였을 경우 Tablet Server 가 정상적으로 연결되지 않는것을 확인하였습니다.


## detail
오류 내용은 다음과 같습니다.

KUDU를 총 4개의 서버로 구성하여(1서버 설치후 복제) KUDU 실행시 Master 서버는 4개 모두 정상적으로 올라오나, Tablet server의 경우에는 1개만 정상적으로 등록되며, 나머지 Tablet server log 에서는 다음과 같은 에러 메세지를 떨어뜨립니다.

> 
W1116 11:14:42.817109  8245 heartbeater.cc:538] Failed to heartbeat to KUDU1:7051: Remote error: Failed to send heartbeat to master: Invalid argument: Tablet server cff17fa617c74bd0b32cbc00d9a11a25 is attempting to re-register with a different host/port. This is not currently supported. Old: {rpc_addresses { host: "KUDU1" port: 7050 } http_addresses { host: "KUDU1" port: 8050 } software_version: "kudu 1.6.0-cdh5.14.2 (rev 7ff8b43854b24a4ab8631ff70b978eb3901fa1da)" https_enabled: false} New: {rpc_addresses { host: "KUDU2" port: 7050 } http_addresses { host: "KUDU2" port: 8050 } software_version: "kudu 1.6.0-cdh5.14.2 (rev 7ff8b43854b24a4ab8631ff70b978eb3901fa1da)" https_enabled: false}

내용을 대충 읽어보자면 cff17fa617c74bd0b32cbc00d9a11a25 라는 UUID 를 가진 Tablet 서버가 다른 HOST/PORT 를 가진 체로 재등록을 시도한다고 나와 있습니다.

## solution
생각해보면 KUDU를 그대로 복제했기에 문제가 생긴 것으로 짐작하고 KUDU tablet server의 uuid 를 변경하는 방법을 찾아보았고, 다음 명령어를 통하여 KUDU tablet server 의 저장소를 포멧하여 문제를 해결할수 있게 되었습니다.

> sudo -u kudu kudu fs format --fs_wal_dir={tablet 서버의 디렉토리} --fs_data_dirs={tablet 서버의 디렉토리}

