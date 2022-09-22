`socket` : application layer 와 transport layer사이의 interface

# Socket programming

2가지 tranansport 계층 프로토콜 사용 가능

- UDP : unreliable datagram
- TCP : reliable, byte stream-oriented

<br>

# 1️⃣ Socket programming with UDP

- client와 server간에 **연결이 없음!**
    - No handshaking before sending data
- 데이터가 독립적임
- 전송할 데이터를 잃어버릴 수도 있고, 순서가 바뀐 채로 받을 수 있음
- `datagram` (group of bytes)을 신뢰할 수 없는 방식으로 전송
- 클라이언트는 `IP 목적지 주소` , `port number` 를 packet에 부착해서 전송. `port number` 는 운영체제가 프로세스에 따라 임의로 배정, 패킷을 만들 때 명시
- 서버의 포트 번호는 알려져있음.
- receiver, 서버는 `클라이언트의 IP address`, `포트 번호`를 패킷에서 추출함

<br>

## Client/server socket interaction flow

<img width="679" alt="스크린샷 2022-09-19 오후 5 16 19" src="https://user-images.githubusercontent.com/81469717/191750896-41c64de0-04d8-4ca1-b746-e4cee5181d43.png">

- 서버가 먼저 running

<br>

### UDP Client

<img width="679" alt="스크린샷 2022-09-19 오후 5 33 12" src="https://user-images.githubusercontent.com/81469717/191751027-f7b7ea11-8c02-4520-bc9f-771ca3710861.png">

- `SOCK_DGRAM` 타입 → tansport 계층 UDP 프로토콜 사용
- `AF_INET` 사용 → IPv4 주소 체계
    
    ➡️ 클라이언트 주소와 사용 transport protocol 명시하기 위해 
    
- 운영체제는 port number를 관리, 운영체제만 클라이언트의 port number를 앎
- client socket으로 받은 메시지에서 modified message와 server address를 추출

<br>

### UDP Server
<img width="679" alt="udp server" src="https://user-images.githubusercontent.com/81469717/191751108-252b7320-f6ab-4023-abfe-0146f11bd487.png">

- 클라이언트와 달리 명시적으로 `서버 소켓`과 `포트 번호`를 바인딩함 → 운영체제가 그에 따라 소켓과 포트를 묶어줌 → 클라이언트도 알 수 있음
- 영원히 반복문을 실행함
    - 서버 소켓으로부터 메세지 받음
    - 데이터를 변환
    - 클라이언트에게 변환된 데이터를 전송, 목적지(client address) 명시해야함

<br>

# 2️⃣ Socket programming with TCP

- reliable, in-order byte-stream transfer(”`pipe`”) 제공

<br>

## Client/server socket interaction flow
<img width="679" alt="tcp flow" src="https://user-images.githubusercontent.com/81469717/191751582-29ca4164-fc2f-43e8-b588-ce80f7b12069.png">

- 서버는 먼저 running하고 있어야 함
- 서버는 `door socket`(welcome socket)을 만듦 (클라이언트의 연결을 받기 위함)

---

- 클라이언트는 서버의 IP주소와 port 번호를 명시한 `TCP client socket`을 만듦
- 소켓을 만들면 서버와 TCP 연결을 함

---

- 서버와 클라이언트가 연결 되면, 서버 TCP는 해당 클라이언트와의 소통을 위한  새로운 `connection socket`을 생성
    - 서버가 여러 클라이언트와 동시에 소통 가능함
- 따라서 handshaking후에는 클라이언트 주소가 필요 없음
- 믿을 수 있는, `byte stream` transfer : 임의의 비트를 연속적으로 전송 가능한 특징, 파이프 라인 구축

---

- 클라이언트는 `client socket` 을 통해 요청을 보냄
- 서버는 `connection socket` 으로 응답을 보냄
- 클라이언트는 `client socket` 으로 응답을 읽음

---

- `client socket` close
- `connection socket` close

<br>

### TCP Client

<img width="679" alt="tcp client" src="https://user-images.githubusercontent.com/81469717/191751667-c57fe4ba-1e45-4a34-96fb-1df5be2e06b3.png">

- `SOCK_STREAM` : transport 계층 TCP 사용
- 운영체제는 TCP port number 배정
- client socket에 `서버 host 주소` + `서버 port number` 연결
- client socket으로 메시지 전송
- client socket에 응답 도착
- UDP와 달리 address 추출하지 않음. client socket에 연결된 server address는 이미 알기 때문

<br>

### TCP server
<img width="619" alt="tcp server" src="https://user-images.githubusercontent.com/81469717/191751739-696970ac-2e07-4fcc-9532-bc5014617ba4.png">

- server port 번호 명시 (door socket 만 public, known port number)
- server socket 만듦 (주소 체계, Transport 계층 → TCP 사용 명시)
- server socket과 server port number 바인드
- serverSocket.listen : 요청 대기
- server socket이 TCP 연결 승인하면 connection socket 생성(client 주소 연결)
- **connection socket에는 데이터만 보내주면됨**
- connection socket의 port number는 몰라도 통신 가능 (은닉화)
- 보통 응답 메시지를 보내고 나서는 connection socket close
- 여러번 요청-응답을 받도록 connection socket 오픈해둘 수 있음

<aside>
💡 특정 클라이언트를 위한 connection socket을 생성해 데이터를 연속적으로 보낼 수 있는 파이프라인 구축

</aside>
