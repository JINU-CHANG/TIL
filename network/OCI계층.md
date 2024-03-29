## OSI 계층

OSI 7계층는 네트워크를 구성하는 7가지 계층을 의미한다. 7가지의 계층을 표준화함으로써 통신이 일어나는 과정을 단계별로 파악할 수 있으며 문제가 발생하면 해당 문제를 해결하기 용이해진다.

### Encapsulation

데이터를 보낼 때 아래 계층으로 내려갈수록 payload에 header를 붙여서 데이터를 전달한다.

### TCP(Transmission Control Protocol) (전송 계층)

TCP는 다음과 같은 특징으로 바탕으로 신뢰할 수 있는(reliable) 프로토콜이라고 한다.

- 연결지향(TCP 3 way handshake)
- 데이터 전달 보증
- 순서 보장

**연결지향(TCP 3 way handshake)**

데이터를 보내기 전 연결이 가능한지 확인한다. 목적지가 데이터를 받을 준비가 안 된 상황에서 일방적으로 데이터를 전송하면 목적지에서 데이터를 정상적으로 처리할 수 없기 때문이다. SYN, ACK 이라는 패킷을 주고받음으로써 연결을 확인한다.

1. 클라이언트에서 통신을 시도할 때 **SYN(접속요청) 패킷**을 보낸다.
2. SYN을 받은 서버는 **SYN, ACK(요청 수락)** 으로 응답한다.
3. 클라이언트는 이에 대한 응답을 **ACK 패킷**으로 보낸다.

**TCP 4 way handshake**

TCP 연결을 해제하는 단계로 클라이언트는 서버에게 연결해제를 통지하고 서버가 이를 확인하고 클라이언트에게 이를 받았음을 전송해주고 최종적으로 연결이 해제된다.

### TCP 패킷 정보

- 출발지 PORT
- 목적지 PORT
- 전송 제어
- 순서
- 검증 정보

### UDP(User Datagram Protocol)

TCP와 같은 계층의 프로토콜이다. 하지만 UDP는 3 way handshake, 데이터 전달 보증, 순서 보장이 없는 프로토콜이다. IP와 거의 같은 기능을 한다. 따라서 TCP는 파일전송과 같이 신뢰성이 중요한 서비스에 사용되고, UDP는 스트리밍과 같이 빠르고 연속성이 더 중요한 서비스에 사용된다.


## IP

IP 패킷 정보

- 출발지 IP
- 목적지 IP
