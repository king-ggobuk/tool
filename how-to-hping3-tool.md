# how to hping3 tool

- **hping3 툴의 용도**

Scanning ( Port Scan && Host Sweep )

Flooding Attack

IP Spoofing

**[ 형태 ]**

# hping3 <Mode> <options> <source> <destination> <port> <count>

# hping3 -S 192.168.20.200 -p 80 -c 3

# hping3 -S -a 192.168.20.203 192.168.20.200 -p 80 -i u1000

# hping3 -S --rand-source 192.168.20.200 -p 80 -i u1000

# hping3 -h ( # hping3 --hlep )

......중략 .....

Mode

default mode     TCP

-0  --rawip      RAW IP mode

-1  --icmp       ICMP mode

-2  --udp        UDP mode

-8  --scan       SCAN mode.

IP

-a  --spoof      spoof source address

..... 중략 .....

ICMP

-p  --destport   [+][+]<port> destination port(default 0) ctrl+z inc/dec

-F  --fin        set FIN flag

-S  --syn        set SYN flag

-R  --rst        set RST flag

-P  --push       set PUSH flag

-A  --ack        set ACK flag

-U  --urg        set URG flag

-X  --xmas       set X unused flag (0x40)

..... 중략 ....

Common

-d  --data       data size                    (default is 0)

.... 중략 ....

**[ 테스트 ]**

**( kali )**

# hping3 -S 192.168.27.200 -p 80 -c 5

Mode : 생략하면 tcp

SorceIP : 생략하면 자신의 IP 192.168.27.50

- S : syn 192.168.27.200 ( 타겟 )
- p : port 80
- c : 5번 보냄

HPING 192.168.27.200 (eth1 192.168.27.200): S set, 40 headers + 0 data bytes

len=46 ip=192.168.27.200 ttl=64 DF id=0 sport=80 flags=SA seq=0 win=5840 rtt=7.6 ms

len=46 ip=192.168.27.200 ttl=64 DF id=0 sport=80 flags=SA seq=1 win=5840 rtt=7.0 ms

len=46 ip=192.168.27.200 ttl=64 DF id=0 sport=80 flags=SA seq=2 win=5840 rtt=6.4 ms

len=46 ip=192.168.27.200 ttl=64 DF id=0 sport=80 flags=SA seq=3 win=5840 rtt=6.1 ms

len=46 ip=192.168.27.200 ttl=64 DF id=0 sport=80 flags=SA seq=4 win=5840 rtt=0.7 ms

- -- 192.168.27.200 hping statistic ---

5 packets transmitted, 5 packets received, 0% packet loss

round-trip min/avg/max = 0.7/5.6/7.6 ms

# hping3 -S 192.168.27.200 -p ++50 -c 5    /*  ++50 은 잘 사용하지 않는다. */

- p ++50 : 포트 번호 50부터 하나씩 늘려가며 hping3 수행
- c 5 : 5번 보냄 -> 50부터 5번이니 50번 포트부터 54번 포트까지 점검

HPING 192.168.27.200 (eth1 192.168.27.200): S set, 40 headers + 0 data bytes

len=46 ip=192.168.27.200 ttl=64 DF id=0 sport=50 flags=RA seq=0 win=0 rtt=7.9 ms

len=46 ip=192.168.27.200 ttl=64 DF id=0 sport=51 flags=RA seq=1 win=0 rtt=7.0 ms

len=46 ip=192.168.27.200 ttl=64 DF id=0 sport=52 flags=RA seq=2 win=0 rtt=3.1 ms

len=46 ip=192.168.27.200 ttl=64 DF id=0 sport=53 flags=SA seq=3 win=5840 rtt=2.4 ms

len=46 ip=192.168.27.200 ttl=64 DF id=0 sport=54 flags=RA seq=4 win=0 rtt=2.1 ms

- -- 192.168.27.200 hping statistic ---

5 packets transmitted, 5 packets received, 0% packet loss

round-trip min/avg/max = 2.1/4.5/7.9 ms

# hping3 -S 192.168.27.200 -8 50-56

- 8 50-56 : 50번 포트부터 56번 포트까지 SCAN 열려있는것만 출력

Scanning 192.168.27.200 (192.168.27.200), port 50-56

7 ports to scan, use -V to see all the replies

+----+-----------+---------+---+-----+-----+-----+

|port| serv name |  flags  |ttl| id  | win | len |

+----+-----------+---------+---+-----+-----+-----+

53 domain     : .S..A...  64     0  5840    46

All replies received. Done.

Not responding ports:

# hping3 -1 192.168.27.200 --icmp-ts -c 3

- 1 : ICMP
- -icmp-ts : request time 출력

HPING 192.168.27.200 (eth1 192.168.27.200): icmp mode set, 28 headers + 0 data bytes

len=46 ip=192.168.27.200 ttl=64 id=3779 icmp_seq=0 rtt=4.5 ms

ICMP timestamp: Originate=41295068 Receive=73682443 Transmit=73682443

ICMP timestamp RTT tsrtt=4

len=46 ip=192.168.27.200 ttl=64 id=3780 icmp_seq=1 rtt=8.0 ms

ICMP timestamp: Originate=41296068 Receive=73683444 Transmit=73683444

ICMP timestamp RTT tsrtt=8

len=46 ip=192.168.27.200 ttl=64 id=3781 icmp_seq=2 rtt=3.3 ms

ICMP timestamp: Originate=41297069 Receive=73684444 Transmit=73684444

ICMP timestamp RTT tsrtt=3

- -- 192.168.27.200 hping statistic ---

3 packets transmitted, 3 packets received, 0% packet loss

round-trip min/avg/max = 3.3/5.3/8.0 ms

# hping3 -2 192.168.27.200 -p 53 -c 5

- 2 : UDP

HPING 192.168.27.200 (eth1 192.168.27.200): udp mode set, 28 headers + 0 data bytes

- -- 192.168.27.200 hping statistic ---

5 packets transmitted, 0 packets received, 100% packet loss

round-trip min/avg/max = 0.0/0.0/0.0 ms

> udp는 SYN ACK 패킷을 받지 않는다.

# hping3 -1 192.168.27.200 -a 192.168.27.204 -c 3

- 1 : icmp
- a : spoof

> 192.168.27.200 번 서버에 Source IP 가 192.168.27.204로 ICMP 패킷을 전송한다.

HPING 192.168.27.200 (eth1 192.168.27.200): icmp mode set, 28 headers + 0 data bytes

- -- 192.168.27.200 hping statistic ---

3 packets transmitted, 0 packets received, 100% packet loss

round-trip min/avg/max = 0.0/0.0/0.0 ms

[https://t1.daumcdn.net/cfile/tistory/9950B33359EB30F607](https://t1.daumcdn.net/cfile/tistory/9950B33359EB30F607)

> 200서버는 SYN ACK 패킷을 204 서버로 전송하므로 50서버로 들어오는 패킷은 없다.

> wireshark로 확인해보면 204번으로 200 에 보낸것을 확인 할 수있다.

# hping3 -1 192.168.27.200 -a 192.168.27.201 -c 3

HPING 192.168.27.200 (eth1 192.168.27.200): icmp mode set, 28 headers + 0 data bytes

- -- 192.168.27.200 hping statistic ---

3 packets transmitted, 0 packets received, 100% packet loss

round-trip min/avg/max = 0.0/0.0/0.0 ms
