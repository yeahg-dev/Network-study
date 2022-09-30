# Transport layer services

> ⭐️ 핵심 역할 : 
> 
> 다른 host에서 실행중인 **application program 간** logical communication
>
> = **process 간** logical communication
> 
- end system에서 실행됨
    - 송신 쪽 : app message를 `segment` 로 분할해서 network layer로 전달
    - 수신 쪽 : `segment` 를 message로 다시 조합해서 app layer에게 전달
- Internet: TCP and UDP

<br>

## Transport layer 🆚 Network layer

### network layer

> **host간** logical communication
> 

### transport layer

> **process간** logical communication + network layer 서비스 신뢰도 개선 (TCP만 해당)
> 

- **reliable, in-order delivery** **(TCP)**
    - connection setup
    - congestion control - network 과부하 방지
    - flow control - receiver buffer 과부하 방지
- **unreliable, unorder delivery (UDP)**
    - no-frills extension of “best effort” IP
- delay, bandwidth 보장 불가

---

# Transport layer 핵심 기능 : Multiplexing / Demultiplexing

- 보내는 쪽 : Multiplexing
    - 여러 process의 **요청을 하나**로 multiplexing (여러개의 신호를 하나의 연결을 사용해서 보내는 것)
    - message마다 segment header에 목적지 명시 ➡️ network layer
- 받는 쪽 : demultiplexing
    - network layer를 통해 다른 클라이언트에서 온 메시지들을 하나로 받음
    - header로 구분해서 **메시지 분할**하는 것
- 따라서 ⭐️ dest/source port Number ⭐️ 필요, `Segment`에 부착 - socket 구분

<img width="619" alt="demulti" src="https://user-images.githubusercontent.com/81469717/193206179-13940483-af3d-4008-95e3-efd9d83e13fb.png">


- `datagram` : `source IP address` , `dest IP address` 포함 (Network Layer)
- `segment` : transport layer에서 만드는 data unit
    - `header` + `header fields` +  `payload` 구성
    - `header` : `source port number`, `dest port number` (network 계층에서 host address 부착해서 보냄)

<br>

## Connectionless, UDP demultiplexing 

<img width="619" alt="connectionless-demulit" src="https://user-images.githubusercontent.com/81469717/193207437-ec338476-5145-4bda-b832-1c1a56c84d88.png">ㄱㄷ

**reciever**
- 생성된 소켓은 host local port number을 할당 받음
- `UDP socket`으로 보내는 `datagram을` 만들기 위해서는
    - `dest IP address`, `dest port number` 명시해야함
- host가 `UDP segment`을 받으면 segment에 적힌 `dest port number의 socket`에게 전달
- 만약 같은 dest port number, 다른 source IP주소의 message가 서버에 오면 동일한 서버 소켓으로 모임

<br>

## Connection-oriented, TCP demultiplexing 

<img width="642" alt="connection demulti" src="https://user-images.githubusercontent.com/81469717/193209088-198ef8bd-feeb-4be7-93cf-f39cb9231185.png">


- TCP socket을 분별하는데 필요로하는 4가지 정보
    - `source IP address` (기반으로 connection 소켓 구분)
    - `source port number` (기반으로 connection 소켓 구분)
    - `dest IP address`
    - `dest port number` (도어 소켓 확인)
- connection socket을 만들면, application의 process를 fork 함 = 자식 프로세스를 만듦
    - threaded server
