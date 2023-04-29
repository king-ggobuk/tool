# Iptables

### 1. 포트 상태 확인

- 열려있는 모든 포트 표시

```html
netstat -nap
```

 -n : host명으로 표시 안함

 -a : 모든 소켓 표시 

 -p : 프로세스ID와 프로그램명 표시

- LISTEN중인 포트 표시

```bash
netstat -nap | grep LISTEN
```

- 확인하려는 포트번호 상태확인

```bash
netstat -anp | grep [포트번호]
```

- 특정 호스트 포트 상태 확인

 - 특정 호스트로 접속이 불가할 때, netcat(nc) 네트워크 유틸리티를 이용하여 포트가 막혀 있는지 확인할 수 있다.

 * netcat : TCP,UDP 프로토콜을 사용하는 네트워크에서 네트워크 연결 상태를 읽거나 쓸때 사용하는 유틸리티 프로그램이다.

- 특정 포트 상태 확인

```bash
nc -z [호스트주소] [포트]
nc -z www.google.com 80 
```

 Target Port가 열려있다면 아래의 결과를 확인할 수 있다.

```bash
Connetion to www.google.com 80 port [tcp/http] succeeded!
```

b) 특정 호스트의 포트 범위를 지정하여 열린 포트 확인

```bash
nc [호스트주소] -z [시작포트]-[끝포트]
nc 10.20.30.40 -z 19-20
```

```bash
Connection to 10.20.30.40 21 port [tcp/ftp] succeeded!
Connection to 10.20.30.40 22 port [tcp/ssh] succeeded!
Connection to 10.20.30.40 23 port [tcp/telnet] succeeded!
```

---

### 2. 포트 열기

 - 리눅스 방화벽 설정 명령어인 iptables 을 사용하여 포트를 오픈할 수 있다.

- 방화벽 설정 정보 확인

```bash
iptables -nL
```

- 특정포트 외부에서 접속할 수 있도록 열기 (외부에서 접속할 수 있도록 포트 OPEN)

  - 아래는 외부에서 들어오는(Inbound) TCP 포트 12345의 연결을 받아들인다는 규칙을 1번 방화벽 규칙으로 추가한다는 의미이다.

 

 - TCP PORT일 경우

```bash
iptables -I INPUT 1 -p tcp --dport 12345 -j ACCEPT 
```

 - UDP PORT 일 경우

```bash
iptables -I INPUT 1 -p udp --dport 12345 -j ACCEPT
```

 - 옵션 설명 

 -I : 새로운 규칙을 추가한다.

 -p : 패킷의 프로토콜을 명시한다.

 -j : 규칙에 해당되는 패킷을 어떻게 처리할지를 정한다.

- 내부에서 외부로 나갈 수 있도록 포트 열기

```bash
iptables -I OUTPUT 1 -p tcp -dport 9002 -j ACCEPT
iptables -I OUTPUT 1 -p udp -dport 9002 -j ACCEPT
```

 - 추가한 설정 조회 

```bash
iptables -L -v
```

 -L : 규칙을 출력

 -v : 자세히

- 추가한 설정 삭제

 - 추가한 규칙의 번호로 삭제하는 방법과

 - 추가했을 때의 명령어에서 ‘-I’를 ‘-D’로 바꾸어주는 방법이 있다.

- 규칙번호로 삭제

```bash
iptables -D INPUT 1
```

- 축하나 규칙으로 삭제

```bash
iptables -D INPUT -p tcp --dport 12345 -j ACCEPT
iptables -D INPUT -p udp --dport 12345 -j ACCEPT
```

- 변경사항 저장

```bash
service iptables save
/etc/init.d/iptables restart
```

### 3. 방화벽 활성화&비활성화

- 켜기

```
/etc/init.d/iptables start
```

- 끄기
```
/etc/init.d/iptables stop
```
