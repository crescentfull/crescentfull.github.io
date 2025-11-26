---
title: "mongoDB 연결 오류"
description: "mongoDB 연결 오류와 가상환경 설정의 험난한 여정"
date: 2022-08-05 10:00:00 +0900
categories: [Error]
tags: [Error, mongoDB, miniconda, anaconda, virtualenv]
---

✅ mongoDB를 homebrew를 이용하여 설치하였습니다. (macOS M1)

## no.1 DBException error.. 왜 자꾸 db폴더를 못찾아 너..
토이프로젝트를 하다가 분명 첫날에는 잘되었는데 갑자기 db경로를 찾지 못하는 상황이 벌어졌다.

찾아보니 macOS Catalina 버전 이후에는 root인 파일경로에서 쓰기를 할 수 없도록 업데이트 되었기 때문에 경로를 /Users/{username} 쪽으로 변경해줘야 한다는 것을 보았다.

### 
![](https://velog.velcdn.com/images/sicksong/post/4c893544-fe8c-42b1-956b-7c2ced8b25f1/image.png)
> 터미널에서 brew --prefix 명령어를 실행하면 brew 설치파일의 원경로 확인 가능

/opt/homebrew/etc/mongod.conf 파일을 열어서 log directory 와 data directory를 변경시켜주었다.
```bash
log directory
/Users/{username}/mongodb/log/mongo.log

data directory
/Users/{username}/data/db
```
자 그리고 실행
```bash
❯ mongod
{"t":{"$date":"2022-08-04T20:22:55.673+09:00"},"s":"I",  "c":"CONTROL",  "id":23285,   "ctx":"thread1","msg":"Automatically disabling TLS 1.0, to force-enable TLS 1.0 specify --sslDisabledProtocols 'none'"}
{"t":{"$date":"2022-08-04T20:22:55.674+09:00"},"s":"I",  "c":"NETWORK",  "id":4915701, "ctx":"thread1","msg":"Initialized wire specification","attr":{"spec":{"incomingExternalClient":{"minWireVersion":0,"maxWireVersion":13},"incomingInternalClient":{"minWireVersion":0,"maxWireVersion":13},"outgoing":{"minWireVersion":0,"maxWireVersion":13},"isInternalClient":true}}}
{"t":{"$date":"2022-08-04T20:22:55.686+09:00"},"s":"W",  "c":"ASIO",     "id":22601,   "ctx":"thread1","msg":"No TransportLayer configured during NetworkInterface startup"}
{"t":{"$date":"2022-08-04T20:22:55.686+09:00"},"s":"I",  "c":"NETWORK",  "id":4648602, "ctx":"thread1","msg":"Implicit TCP FastOpen in use."}
{"t":{"$date":"2022-08-04T20:22:55.691+09:00"},"s":"W",  "c":"ASIO",     "id":22601,   "ctx":"thread1","msg":"No TransportLayer configured during NetworkInterface startup"}
{"t":{"$date":"2022-08-04T20:22:55.692+09:00"},"s":"I",  "c":"REPL",     "id":5123008, "ctx":"thread1","msg":"Successfully registered PrimaryOnlyService","attr":{"service":"TenantMigrationDonorService","ns":"config.tenantMigrationDonors"}}
{"t":{"$date":"2022-08-04T20:22:55.692+09:00"},"s":"I",  "c":"REPL",     "id":5123008, "ctx":"thread1","msg":"Successfully registered PrimaryOnlyService","attr":{"service":"TenantMigrationRecipientService","ns":"config.tenantMigrationRecipients"}}
{"t":{"$date":"2022-08-04T20:22:55.692+09:00"},"s":"I",  "c":"CONTROL",  "id":5945603, "ctx":"thread1","msg":"Multi threading initialized"}
❗️{"t":{"$date":"2022-08-04T20:22:55.693+09:00"},"s":"I",  "c":"CONTROL",  "id":4615611, "ctx":"initandlisten","msg":"MongoDB starting","attr":{"pid":21294,"port":27017,🥲"dbPath":"/data/db","architecture":"64-bit","host":"{username}ui-MacBookPro.local"}}
{"t":{"$date":"2022-08-04T20:22:55.693+09:00"},"s":"I",  "c":"CONTROL",  "id":23403,   "ctx":"initandlisten","msg":"Build Info","attr":{"buildInfo":{"version":"5.0.10","gitVersion":"bbf5bc7e16d1713c9349a09adf4901ca37472e66","modules":[],"allocator":"system","environment":{"distarch":"x86_64","target_arch":"x86_64"}}}}
{"t":{"$date":"2022-08-04T20:22:55.693+09:00"},"s":"I",  "c":"CONTROL",  "id":51765,   "ctx":"initandlisten","msg":"Operating System","attr":{"os":{"name":"Mac OS X","version":"21.6.0"}}}
{"t":{"$date":"2022-08-04T20:22:55.693+09:00"},"s":"I",  "c":"CONTROL",  "id":21951,   "ctx":"initandlisten","msg":"Options set by command line","attr":{"options":{}}}
❗️{"t":{"$date":"2022-08-04T20:22:55.696+09:00"},"s":"I",  "c":"NETWORK",  "id":5693100, "ctx":"initandlisten","msg":"Asio socket.set_option failed with std::system_error","attr":{"note":"acceptor TCP fast open","option":{"level":6,"name":261,"data":"00 04 00 00"},"error":{"what":"set_option: Invalid argument","message":"Invalid argument","category":"asio.system","value":22}}}
❗️{"t":{"$date":"2022-08-04T20:22:55.700+09:00"},"s":"E",  "c":"CONTROL",  "id":20557,   "ctx":"initandlisten","msg":"DBException in initAndListen, terminating","attr":{"error":"NonExistentPath: Data directory /data/db not found. Create the missing directory or specify another path using (1) the --dbpath command line option, or (2) by adding the 'storage.dbPath' option in the configuration file."}}
{"t":{"$date":"2022-08-04T20:22:55.700+09:00"},"s":"I",  "c":"REPL",     "id":4784900, "ctx":"initandlisten","msg":"Stepping down the ReplicationCoordinator for shutdown","attr":{"waitTimeMillis":15000}}
{"t":{"$date":"2022-08-04T20:22:55.702+09:00"},"s":"I",  "c":"COMMAND",  "id":4784901, "ctx":"initandlisten","msg":"Shutting down the MirrorMaestro"}
{"t":{"$date":"2022-08-04T20:22:55.703+09:00"},"s":"I",  "c":"SHARDING", "id":4784902, "ctx":"initandlisten","msg":"Shutting down the WaitForMajorityService"}
{"t":{"$date":"2022-08-04T20:22:55.703+09:00"},"s":"I",  "c":"NETWORK",  "id":20562,   "ctx":"initandlisten","msg":"Shutdown: going to close listening sockets"}
{"t":{"$date":"2022-08-04T20:22:55.703+09:00"},"s":"I",  "c":"NETWORK",  "id":4784905, "ctx":"initandlisten","msg":"Shutting down the global connection pool"}
{"t":{"$date":"2022-08-04T20:22:55.703+09:00"},"s":"I",  "c":"CONTROL",  "id":4784906, "ctx":"initandlisten","msg":"Shutting down the FlowControlTicketholder"}
{"t":{"$date":"2022-08-04T20:22:55.703+09:00"},"s":"I",  "c":"-",        "id":20520,   "ctx":"initandlisten","msg":"Stopping further Flow Control ticket acquisitions."}
{"t":{"$date":"2022-08-04T20:22:55.703+09:00"},"s":"I",  "c":"NETWORK",  "id":4784918, "ctx":"initandlisten","msg":"Shutting down the ReplicaSetMonitor"}
{"t":{"$date":"2022-08-04T20:22:55.703+09:00"},"s":"I",  "c":"SHARDING", "id":4784921, "ctx":"initandlisten","msg":"Shutting down the MigrationUtilExecutor"}
{"t":{"$date":"2022-08-04T20:22:55.703+09:00"},"s":"I",  "c":"ASIO",     "id":22582,   "ctx":"MigrationUtil-TaskExecutor","msg":"Killing all outstanding egress activity."}
{"t":{"$date":"2022-08-04T20:22:55.703+09:00"},"s":"I",  "c":"COMMAND",  "id":4784923, "ctx":"initandlisten","msg":"Shutting down the ServiceEntryPoint"}
{"t":{"$date":"2022-08-04T20:22:55.703+09:00"},"s":"I",  "c":"CONTROL",  "id":4784925, "ctx":"initandlisten","msg":"Shutting down free monitoring"}
{"t":{"$date":"2022-08-04T20:22:55.703+09:00"},"s":"I",  "c":"CONTROL",  "id":4784927, "ctx":"initandlisten","msg":"Shutting down the HealthLog"}
{"t":{"$date":"2022-08-04T20:22:55.703+09:00"},"s":"I",  "c":"CONTROL",  "id":4784928, "ctx":"initandlisten","msg":"Shutting down the TTL monitor"}
{"t":{"$date":"2022-08-04T20:22:55.703+09:00"},"s":"I",  "c":"CONTROL",  "id":4784929, "ctx":"initandlisten","msg":"Acquiring the global lock for shutdown"}
{"t":{"$date":"2022-08-04T20:22:55.703+09:00"},"s":"I",  "c":"-",        "id":4784931, "ctx":"initandlisten","msg":"Dropping the scope cache for shutdown"}
{"t":{"$date":"2022-08-04T20:22:55.703+09:00"},"s":"I",  "c":"FTDC",     "id":4784926, "ctx":"initandlisten","msg":"Shutting down full-time data capture"}
{"t":{"$date":"2022-08-04T20:22:55.703+09:00"},"s":"I",  "c":"CONTROL",  "id":20565,   "ctx":"initandlisten","msg":"Now exiting"}
{"t":{"$date":"2022-08-04T20:22:55.704+09:00"},"s":"I",  "c":"CONTROL",  "id":23138,   "ctx":"initandlisten","msg":"Shutting down","attr":{"exitCode":100}}

```
### 아니 분명 mongod.conf에서 바꿔줬는데..?

일단 모르면 헬프부터 눌러보자.. 간혹 헬프에서 유용한 정보를 찾을 수 있다.
```bash
> mongod -h #실행
>>>  

--dbpath arg                          Directory for datafiles - defaults to
                                        /data/db
                                        
# --dbPath 명령에 파일경로를 붙여주라고 한다..
```
그렇구나. mongodb를 실행할때 명령어 뒤에 --dbPath <파일경로>를 붙이고 해줬다.


```bash
❯ mongod --dbpath /Users/{username}/data/db
{"t":{"$date":"2022-08-04T20:50:24.077+09:00"},"s":"I",  "c":"CONTROL",  "id":23285,   "ctx":"-","msg":"Automatically disabling TLS 1.0, to force-enable TLS 1.0 specify --sslDisabledProtocols 'none'"}
{"t":{"$date":"2022-08-04T20:50:24.077+09:00"},"s":"I",  "c":"NETWORK",  "id":4915701, "ctx":"-","msg":"Initialized wire specification","attr":{"spec":{"incomingExternalClient":{"minWireVersion":0,"maxWireVersion":13},"incomingInternalClient":{"minWireVersion":0,"maxWireVersion":13},"outgoing":{"minWireVersion":0,"maxWireVersion":13},"isInternalClient":true}}}
{"t":{"$date":"2022-08-04T20:50:24.082+09:00"},"s":"W",  "c":"ASIO",     "id":22601,   "ctx":"main","msg":"No TransportLayer configured during NetworkInterface startup"}
{"t":{"$date":"2022-08-04T20:50:24.082+09:00"},"s":"I",  "c":"NETWORK",  "id":4648602, "ctx":"main","msg":"Implicit TCP FastOpen in use."}
{"t":{"$date":"2022-08-04T20:50:24.084+09:00"},"s":"W",  "c":"ASIO",     "id":22601,   "ctx":"main","msg":"No TransportLayer configured during NetworkInterface startup"}
{"t":{"$date":"2022-08-04T20:50:24.084+09:00"},"s":"I",  "c":"REPL",     "id":5123008, "ctx":"main","msg":"Successfully registered PrimaryOnlyService","attr":{"service":"TenantMigrationDonorService","ns":"config.tenantMigrationDonors"}}
{"t":{"$date":"2022-08-04T20:50:24.084+09:00"},"s":"I",  "c":"REPL",     "id":5123008, "ctx":"main","msg":"Successfully registered PrimaryOnlyService","attr":{"service":"TenantMigrationRecipientService","ns":"config.tenantMigrationRecipients"}}
{"t":{"$date":"2022-08-04T20:50:24.084+09:00"},"s":"I",  "c":"CONTROL",  "id":5945603, "ctx":"main","msg":"Multi threading initialized"}
✅{"t":{"$date":"2022-08-04T20:50:24.084+09:00"},"s":"I",  "c":"CONTROL",  "id":4615611, "ctx":"initandlisten","msg":"MongoDB starting","attr":{"pid":21576,"port":27017,"dbPath":"/Users/{username}/data/db","architecture":"64-bit","host":"{username}ui-MacBookPro.local"}}
{"t":{"$date":"2022-08-04T20:50:24.084+09:00"},"s":"I",  "c":"CONTROL",  "id":23403,   "ctx":"initandlisten","msg":"Build Info","attr":{"buildInfo":{"version":"5.0.10","gitVersion":"bbf5bc7e16d1713c9349a09adf4901ca37472e66","modules":[],"allocator":"system","environment":{"distarch":"x86_64","target_arch":"x86_64"}}}}
{"t":{"$date":"2022-08-04T20:50:24.084+09:00"},"s":"I",  "c":"CONTROL",  "id":51765,   "ctx":"initandlisten","msg":"Operating System","attr":{"os":{"name":"Mac OS X","version":"21.6.0"}}}
{"t":{"$date":"2022-08-04T20:50:24.084+09:00"},"s":"I",  "c":"CONTROL",  "id":21951,   "ctx":"initandlisten","msg":"Options set by command line","attr":{"options":{"storage":{"dbPath":"/Users/{username}/data/db"}}}}
❓{"t":{"$date":"2022-08-04T20:50:24.086+09:00"},"s":"I",  "c":"NETWORK",  "id":5693100, "ctx":"initandlisten","msg":"Asio socket.set_option failed with std::system_error","attr":{"note":"acceptor TCP fast open","option":{"level":6,"name":261,"data":"00 04 00 00"},"error":{"what":"set_option: Invalid argument","message":"Invalid argument","category":"asio.system","value":22}}}
✅{"t":{"$date":"2022-08-04T20:50:24.090+09:00"},"s":"I",  "c":"STORAGE",  "id":22270,   "ctx":"initandlisten","msg":"Storage engine to use detected by data files","attr":{"dbpath":"/Users/{username}/data/db","storageEngine":"wiredTiger"}}
{"t":{"$date":"2022-08-04T20:50:24.090+09:00"},"s":"I",  "c":"STORAGE",  "id":22315,   "ctx":"initandlisten","msg":"Opening WiredTiger","attr":{"config":"create,cache_size=7680M,session_max=33000,eviction=(threads_min=4,threads_max=4),config_base=false,statistics=(fast),log=(enabled=true,archive=true,path=journal,compressor=snappy),builtin_extension_config=(zstd=(compression_level=6)),file_manager=(close_idle_time=600,close_scan_interval=10,close_handle_minimum=250),statistics_log=(wait=0),verbose=[recovery_progress,checkpoint_progress,compact_progress],"}}
{"t":{"$date":"2022-08-04T20:50:24.306+09:00"},"s":"I",  "c":"STORAGE",  "id":22430,   "ctx":"initandlisten","msg":"WiredTiger message","attr":{"message":"[1659613824:306974][21576:0x205f24600], txn-recover: [WT_VERB_RECOVERY_PROGRESS] Recovering log 2 through 3"}}
{"t":{"$date":"2022-08-04T20:50:24.347+09:00"},"s":"I",  "c":"STORAGE",  "id":22430,   "ctx":"initandlisten","msg":"WiredTiger message","attr":{"message":"[1659613824:347317][21576:0x205f24600], txn-recover: [WT_VERB_RECOVERY_PROGRESS] Recovering log 3 through 3"}}
{"t":{"$date":"2022-08-04T20:50:24.392+09:00"},"s":"I",  "c":"STORAGE",  "id":22430,   "ctx":"initandlisten","msg":"WiredTiger message","attr":{"message":"[1659613824:392580][21576:0x205f24600], txn-recover: [WT_VERB_RECOVERY_ALL] Main recovery loop: starting at 2/20096 to 3/256"}}
{"t":{"$date":"2022-08-04T20:50:24.471+09:00"},"s":"I",  "c":"STORAGE",  "id":22430,   "ctx":"initandlisten","msg":"WiredTiger message","attr":{"message":"[1659613824:471455][21576:0x205f24600], txn-recover: [WT_VERB_RECOVERY_PROGRESS] Recovering log 2 through 3"}}
{"t":{"$date":"2022-08-04T20:50:24.533+09:00"},"s":"I",  "c":"STORAGE",  "id":22430,   "ctx":"initandlisten","msg":"WiredTiger message","attr":{"message":"[1659613824:533055][21576:0x205f24600], txn-recover: [WT_VERB_RECOVERY_PROGRESS] Recovering log 3 through 3"}}
{"t":{"$date":"2022-08-04T20:50:24.572+09:00"},"s":"I",  "c":"STORAGE",  "id":22430,   "ctx":"initandlisten","msg":"WiredTiger message","attr":{"message":"[1659613824:572228][21576:0x205f24600], txn-recover: [WT_VERB_RECOVERY_ALL] Set global recovery timestamp: (0, 0)"}}
{"t":{"$date":"2022-08-04T20:50:24.572+09:00"},"s":"I",  "c":"STORAGE",  "id":22430,   "ctx":"initandlisten","msg":"WiredTiger message","attr":{"message":"[1659613824:572285][21576:0x205f24600], txn-recover: [WT_VERB_RECOVERY_ALL] Set global oldest timestamp: (0, 0)"}}
{"t":{"$date":"2022-08-04T20:50:24.591+09:00"},"s":"I",  "c":"STORAGE",  "id":22430,   "ctx":"initandlisten","msg":"WiredTiger message","attr":{"message":"[1659613824:591510][21576:0x205f24600], WT_SESSION.checkpoint: [WT_VERB_CHECKPOINT_PROGRESS] saving checkpoint snapshot min: 1, snapshot max: 1 snapshot count: 0, oldest timestamp: (0, 0) , meta checkpoint timestamp: (0, 0) base write gen: 198"}}
{"t":{"$date":"2022-08-04T20:50:24.701+09:00"},"s":"I",  "c":"STORAGE",  "id":4795906, "ctx":"initandlisten","msg":"WiredTiger opened","attr":{"durationMillis":611}}
{"t":{"$date":"2022-08-04T20:50:24.701+09:00"},"s":"I",  "c":"RECOVERY", "id":23987,   "ctx":"initandlisten","msg":"WiredTiger recoveryTimestamp","attr":{"recoveryTimestamp":{"$timestamp":{"t":0,"i":0}}}}
{"t":{"$date":"2022-08-04T20:50:24.702+09:00"},"s":"I",  "c":"STORAGE",  "id":4366408, "ctx":"initandlisten","msg":"No table logging settings modifications are required for existing WiredTiger tables","attr":{"loggingEnabled":true}}
{"t":{"$date":"2022-08-04T20:50:24.721+09:00"},"s":"I",  "c":"STORAGE",  "id":22262,   "ctx":"initandlisten","msg":"Timestamp monitor starting"}
{"t":{"$date":"2022-08-04T20:50:24.738+09:00"},"s":"W",  "c":"CONTROL",  "id":22120,   "ctx":"initandlisten","msg":"Access control is not enabled for the database. Read and write access to data and configuration is unrestricted","tags":["startupWarnings"]}
{"t":{"$date":"2022-08-04T20:50:24.738+09:00"},"s":"W",  "c":"CONTROL",  "id":22140,   "ctx":"initandlisten","msg":"This server is bound to localhost. Remote systems will be unable to connect to this server. Start the server with --bind_ip <address> to specify which IP addresses it should serve responses from, or with --bind_ip_all to bind to all interfaces. If this behavior is desired, start the server with --bind_ip 127.0.0.1 to disable this warning","tags":["startupWarnings"]}
{"t":{"$date":"2022-08-04T20:50:24.738+09:00"},"s":"W",  "c":"CONTROL",  "id":22184,   "ctx":"initandlisten","msg":"Soft rlimits for open file descriptors too low","attr":{"currentValue":256,"recommendedMinimum":64000},"tags":["startupWarnings"]}
{"t":{"$date":"2022-08-04T20:50:24.757+09:00"},"s":"I",  "c":"NETWORK",  "id":4915702, "ctx":"initandlisten","msg":"Updated wire specification","attr":{"oldSpec":{"incomingExternalClient":{"minWireVersion":0,"maxWireVersion":13},"incomingInternalClient":{"minWireVersion":0,"maxWireVersion":13},"outgoing":{"minWireVersion":0,"maxWireVersion":13},"isInternalClient":true},"newSpec":{"incomingExternalClient":{"minWireVersion":0,"maxWireVersion":13},"incomingInternalClient":{"minWireVersion":13,"maxWireVersion":13},"outgoing":{"minWireVersion":13,"maxWireVersion":13},"isInternalClient":true}}}
{"t":{"$date":"2022-08-04T20:50:24.757+09:00"},"s":"I",  "c":"STORAGE",  "id":5071100, "ctx":"initandlisten","msg":"Clearing temp directory"}
{"t":{"$date":"2022-08-04T20:50:24.757+09:00"},"s":"I",  "c":"CONTROL",  "id":20536,   "ctx":"initandlisten","msg":"Flow Control is enabled on this deployment"}
{"t":{"$date":"2022-08-04T20:50:24.758+09:00"},"s":"I",  "c":"FTDC",     "id":20625,   "ctx":"initandlisten","msg":"Initializing full-time diagnostic data capture","attr":{"dataDirectory":"/Users/{username}/data/db/diagnostic.data"}}
{"t":{"$date":"2022-08-04T20:50:24.776+09:00"},"s":"I",  "c":"REPL",     "id":6015317, "ctx":"initandlisten","msg":"Setting new configuration state","attr":{"newState":"ConfigReplicationDisabled","oldState":"ConfigPreStart"}}
{"t":{"$date":"2022-08-04T20:50:24.777+09:00"},"s":"I",  "c":"NETWORK",  "id":23015,   "ctx":"listener","msg":"Listening on","attr":{"address":"/tmp/mongodb-27017.sock"}}
{"t":{"$date":"2022-08-04T20:50:24.777+09:00"},"s":"I",  "c":"NETWORK",  "id":23015,   "ctx":"listener","msg":"Listening on","attr":{"address":"127.0.0.1"}}
✅{"t":{"$date":"2022-08-04T20:50:24.777+09:00"},"s":"I",  "c":"NETWORK",  "id":23016,   "ctx":"listener","msg":"Waiting for connections","attr":{"port":27017,"ssl":"off"}}
```
### 연결 성공!!!
그런데..
어.. Asio 넌 뭐니....? socket..?
그래도 mongo shell도 연결이 잘되는 것을 확인하였다.
![](https://velog.velcdn.com/images/sicksong/post/18cb2aca-06b0-4e43-8b26-43b558077176/image.png)

하지만 역시나 mongod만 입력하여 실행하면 여전히 db폴더를 찾지 못한다.
항상 dpPath를 같이 입력 해줘야한다.

### 대체 왜...?

아직 그 이유를 찾지는 못했다.

그래서 혹시나 하는 마음에 모든 경로를 원복시킨 후 --dbPath에 원경로를 입력하여 실행시켜 보았다.

아... 되네..? admin권한이랑은 상관이 없는걸까..?🥲

그럼.. 수십번의 재설치와 버전변경을 한 나의 3일은 어디로..?

아직 완전한 해결책을 찾지 못한채 여전히 dpPath를 붙여서 사용중이다..🤬


## no.2 Already exist..? 언제 실행했었지?
간혹 작업하다가 모르고 mongodb를 종료시키지 않고 터미널을 꺼버린다면 프로세스가 이미 실행중이라 mongodb를 실행시킬수 없는 오류가 발생했었다.

그럴 경우에 가장 쉬운 방법은
```bash
> sudo killall mongodb # mongodb라는 이름으로 실행중인 모든 프로세스를 강제종료 시켜버린다
```
하지만 pid를 확인하여 하나씩 종료시켜야 할 경우에는
```bash
>  sudo lsof -i TCP -s TCP:LISTEN #사용중인 모든 프로세스를 보여준다
```
입력 하면
![](https://velog.velcdn.com/images/sicksong/post/4338eccf-c378-4736-a908-b2af894756a4/image.png)
패스워드(본인 컴퓨터 암호)를 입력하라고 나온다.
입력해주고 Enter!
![](https://velog.velcdn.com/images/sicksong/post/02d5f09a-81c5-4154-acb2-babb5ec71011/image.png)
사진처럼 mongod PID를 찾은 다음
```bash
> sudo kill -9 {PID}
```
명령어 실행시켜주면 해당 PID만 종료시켜 준다..!
