### **NTP 와 Chrony 란**

*네트워크로 구성된 환경에서 각 시스템 마다 현재 시간이 다를 수 있습니다.*

*이 경우 NTP 서버를 기반 시간을 동기화 시키는 프로토콜이 NTP(Network Time Protocol)입니다.*

**: NTP (Network Time Protocol)**

**: Chrony (a versatile implementation of the Network Time Protocol)**

*=> Chrony는 기존 NTP 단점을 보완하여 빠르고 정확하게 동기화 할 수 있도록 개선한 프로토콜입니다.*

*=> RHEL 7 이전에는 NTP을 기본 네트워크 시간 프로토콜로 사용하였고, RHEL 7 이후부터 Chrony로 대체되었습니다.*

시스템 환경이나 상황에 따라 NTP를 사용하는 경우와 Chrony를 사용하는 경우가 발생합니다.

*=> NTP는 영구적으로 유지되는 시스템 또는 브로드 캐스트이나 멀티캐스트 IP를 사용하거나 오토키 프로토콜로 패킷 인증을*

*수행해야하는 시스템에 사용합니다.*

*=> Chrony는 네트워크 (모바일 및 가상 서버 등)에서 자주 중단되거나 간헐적으로 연결이 끊어지는 시스템에 가장 적합합니다.*

### **NTP 설치 및 동기화**

1. chronyd 데몬이 실행되고 있다면 해당 데몬을 중지해야 합니다.

=> ntpd 와 chronyd는 동시에 실행될 수 없습니다.

![https://k.kakaocdn.net/dn/0ISZb/btqFMIZ7Zq6/v0xkOyU4se6toUmQRMQDx0/img.png](https://k.kakaocdn.net/dn/0ISZb/btqFMIZ7Zq6/v0xkOyU4se6toUmQRMQDx0/img.png)

2. ntp 패키지를 설치합니다.

![https://k.kakaocdn.net/dn/bnijBN/btqFLtPoFf9/KkFm0zdgyOUykcZ0ujCB11/img.png](https://k.kakaocdn.net/dn/bnijBN/btqFLtPoFf9/KkFm0zdgyOUykcZ0ujCB11/img.png)

3. 아래와 같이 NTP 포트에 관련된 방화벽을 설정을 변경합니다. 

(Network Time Protocol은 UDP 123 포트를 사용합니다.)

4. /etc/ntp.conf 파일에서 아래와 같이 기존 NTP 서버에 대해 주석 후 새로운 NTP 서버로 변경합니다.

![https://k.kakaocdn.net/dn/cJM26d/btqFNhA38gS/La6tsMbjAc3M3R3CX4xDx0/img.png](https://k.kakaocdn.net/dn/cJM26d/btqFNhA38gS/La6tsMbjAc3M3R3CX4xDx0/img.png)

5. ntpd 데몬 시작 및 부팅시에 활성화 될 수 있도록 설정합니다.

![https://k.kakaocdn.net/dn/zoYVi/btqFNaPCn86/EyLugoFgtGHesGfOngAfg0/img.png](https://k.kakaocdn.net/dn/zoYVi/btqFNaPCn86/EyLugoFgtGHesGfOngAfg0/img.png)

6. NTP 서버와의 싱크를 활성화합니다.

![https://k.kakaocdn.net/dn/bgSZue/btqFNgoxMfh/BLItidnrksDtnf2IR1mVTk/img.png](https://k.kakaocdn.net/dn/bgSZue/btqFNgoxMfh/BLItidnrksDtnf2IR1mVTk/img.png)

7. NTP 서버와의 싱크가 활성화 되었는지 확인합니다

![https://k.kakaocdn.net/dn/vKGZD/btqFM9DmcKz/InJjprmi0quuBSvEc0V61K/img.png](https://k.kakaocdn.net/dn/vKGZD/btqFM9DmcKz/InJjprmi0quuBSvEc0V61K/img.png)

8. NTP 서버와의 상태를 확인합니다.

![https://k.kakaocdn.net/dn/bVdOUL/btqFNgoxR4S/WX6VXprdONPR4kUG7oF7k1/img.png](https://k.kakaocdn.net/dn/bVdOUL/btqFNgoxR4S/WX6VXprdONPR4kUG7oF7k1/img.png)

### **Chrony 설치 및 동기화**

1. chronyd를 제외한 모든 ntpd 관련 데몬들을 중지해야 합니다.=> ntpd 와 chronyd는 동시에 실행될 수 없습니다.

![https://k.kakaocdn.net/dn/AMkrl/btqFMIMA6Yv/yb1TWX10Gp71T5hP1qbz31/img.png](https://k.kakaocdn.net/dn/AMkrl/btqFMIMA6Yv/yb1TWX10Gp71T5hP1qbz31/img.png)

2. chrony 패키지를 설치합니다.

![https://k.kakaocdn.net/dn/bD0CoZ/btqFNa9C0Rk/Yoli1UqqHU7rQ2rOdhrqR1/img.png](https://k.kakaocdn.net/dn/bD0CoZ/btqFNa9C0Rk/Yoli1UqqHU7rQ2rOdhrqR1/img.png)

3. 아래와 같이 NTP 포트에 관련된 방화벽을 설정을 변경합니다. (Network Time Protocol은 UDP 123 포트를 사용합니다.)

4. /etc/chrony.conf 파일에서 아래와 같이 기존 NTP 서버에 대해 주석 후 새로운 NTP 서버로 변경합니다.

![https://k.kakaocdn.net/dn/PNbDC/btqFKLKqoyU/KbhqcfFGRK6tsMq8FT2gJK/img.png](https://k.kakaocdn.net/dn/PNbDC/btqFKLKqoyU/KbhqcfFGRK6tsMq8FT2gJK/img.png)

5. chronyd 데몬 시작 및 부팅시에 활성화 될 수 있도록 설정합니다.

![https://k.kakaocdn.net/dn/nq6gz/btqFKCtfru9/t0yRw0PkY6ASUmoqW0Orn1/img.png](https://k.kakaocdn.net/dn/nq6gz/btqFKCtfru9/t0yRw0PkY6ASUmoqW0Orn1/img.png)

6. NTP 서버와의 싱크를 활성화합니다.

![https://k.kakaocdn.net/dn/lSi2w/btqFNshYmdg/RpXB4iVW9zJ9yiYLsLoduk/img.png](https://k.kakaocdn.net/dn/lSi2w/btqFNshYmdg/RpXB4iVW9zJ9yiYLsLoduk/img.png)

7. NTP 서버와의 싱크가 활성화 되었는지 확인합니다

![https://k.kakaocdn.net/dn/nlNfM/btqFK2yvkNZ/XBG8igLDkXHQGQekklvykK/img.png](https://k.kakaocdn.net/dn/nlNfM/btqFK2yvkNZ/XBG8igLDkXHQGQekklvykK/img.png)

8. chronyd 를 컨트롤하는 명령어인 chronyc 로 정상 연결 여부를 확인합니다
