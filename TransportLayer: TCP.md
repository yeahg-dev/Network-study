# 2) connection-oriented transport : TCP
- Reliable, in-order `byte stream`
    - no message boundaries
    - message를 받으면 먼저 buffer에 저장. buffer에 저장된 데이터를 byte stream으로 취급. 사용자는 데이터를 원하는 양 만큼 읽어갈 수 있음. congestion control, flow control에 의해 제외되는 속도에 따라 데이터를 뽑아냄.
    - UDP: 네트워크 링크에서 물리적으로 실어나를 수 있는 데이터 크기 제한 존재. 최대 frame size가 있기 때문에, message를 segment로 분할
- hanshacking을 해야함
- point to point
    - sender 1, receiver 1
- full duplex data
    - 동일 연결에서 양 방향 데이터 플로우가 발생
    - `MSS` : maximum segment size
- pipelining
    - pipe utiliazation, througput을 높이기 위해서.
    - parallel하게 보냄⭐️

## TCP segment 구조
<img width="642" alt="tcpsegmentstruc" src="https://user-images.githubusercontent.com/81469717/194868988-2590f214-32c7-470d-b321-ebd1dc74c41b.png">

<img width="658" alt="스크린샷 2022-09-25 오후 4 31 42" src="https://user-images.githubusercontent.com/81469717/194869058-d5a442f6-4be3-4cae-8154-0b38d5f57897.png">

- TCP Header : header 길이 가변적(header length 필요)
    - 고정 파트 (4byte * 5)
        - source port number / dest port number
        - seq# : payload에 실린 첫 번째 바이트 번호
        - acknoweldgement number : **다음 받을 data의 첫 byte number**. 즉 number이전까지의 데이터를 잘 받음을 인증(cumulative ACK)
        - header length / not used / UAPRSF / receive window
        - checksum / urg data pointer
    - 가변 파트

### 예시 (telnet)

<img width="658" alt="스크린샷 2022-09-25 오후 4 37 49" src="https://user-images.githubusercontent.com/81469717/194869180-3a5ccf34-5b31-4694-9b5b-ae9dc6ef8fac.png">

 

### Time out : TCP round trip time

- round trip time은 데이터가 올 때마다 걸리는 시간(RTT)을 가지고 평균내서 예측함
    - retransmission은 무시함
    - `estimatedRTT` = (1-a)*`estimatedRTT` + a * `sampleRTT` (바로 이전)
    - 오래된 샘플일수록 기하급수적으로(지수) 적은 비율 반영
<img width="658" alt="스크린샷 2022-09-25 오후 4 43 19" src="https://user-images.githubusercontent.com/81469717/194869381-08bc6b90-47b8-43c8-874f-c605f6630b27.png">

- a 는 0.125로 권장
- 그럼에도 `estimatedRTT` 는 실제 RTT와 갭이 큼
- timeout interval : `estimatedRTT` 에 `safety margin` 을 추가함
- `safety margin`은  네트워크의 동적인 상황을 반영

<img width="658" alt="rtt" src="https://user-images.githubusercontent.com/81469717/194869630-1a66d77c-c3ec-4152-bed3-b0df29cb0eb4.png">


- pipelined segement
- cumlative acks
- single retransmission timer - 하나의 연결에 하나의 타이머

## 가장 간단한 시나리오에서의 sender, reciever의 액션

- TCP sender, receiver - 클라이언트 일수도 서버일 수도
- 보내는 쪽이 sender, 받는 쪽이 receiver

### 📨 TCP sender events:

1. **App → `data` 받음 : `segment` 포장해서 내보냄**
    - `seq #` 붙여 헤더를 붙임
    - payload에 실린 첫 번째 데이터 바이트의 번호
    - `timer` 시작, ack을 못 받은 가장 오래된 `segment`에 대한 타이머임, timeout은 `TimeOutInterval` 로 셋팅
    - `checksum`
2. **Network → `ack` 받음 :** 
    - 가장 오래된 `segment`의 ack이 도착하면 타이머를 리셋함. 그 시점에 가장 오래된 `segment` 에 대해 타이머 시작
3. **timeout**
    - ack이 안들어와 timeout이 발생하면 retransmission & 타이머 시작

- `NextSeqNum` : 다음 번 내보낼 first byte 번호, seq # (= `initial Seq#`로 초기화)
    - `segment` 를 만들고 업데이트. `NextSeqNum` =. `NextSeqNum` + `length(data)`
- `SendBase` : 다음 번 ack을 기다리는 seq # (= `initial Seq#`로 초기화)

### 재전송 시나리오
<img width="653" alt="retrans" src="https://user-images.githubusercontent.com/81469717/194869956-254d3c48-c1f7-4ac5-98e9-73d2b3c13037.png">

<img width="653" alt="retrans2" src="https://user-images.githubusercontent.com/81469717/194870020-e8b271b7-2747-48c1-8966-d13286807699.png">

<aside>
💡 `ack` 으로 sender에게 `segment` 가 완전히 전송되었는지 receiver가 확인
`ack` : reciever가 전송 완료 된 byte의 number + 1을 보냄
`ack`을 받지 못한 가장 오래전에 전송한 `seq#` 에 대해 `Timer` 를 가동.  `TimeInteraval` 안에 도착하지 않으면 Timeout으로 간주해 retransmission

</aside>

### 📩 TCP receiver events:

- Network로부터 `segment` 가 도착하면 `ACK` 보냄
- `NACK` 사용하지 않고 `ACK` 만 사용!

<img width="653" alt="ack" src="https://user-images.githubusercontent.com/81469717/194870167-3bcd2571-465f-42d5-91db-5f9d1becfd8a.png">

- **in-order segment 이벤트**
    - segment는 연속적으로 들어오므로 다음 segment를 500ms 기다림.
    - 다음 `segment`가 들어오는 즉시 `cumulative ACK` 내보냄
- **out-of-order segment 이벤트**
    - 즉시 `duplicated ACK` 내보냄. (다음에 받길 기대하는 byte 번호를 적음)
    - 만약 갭을 일부/전부 채우는 `segment`가 도착할시, 기다리지 않고 즉시 `ACK`을 내보냄

### TCP fast retransmit

<img width="653" alt="fast" src="https://user-images.githubusercontent.com/81469717/194870307-fc4d15ec-500c-48a8-bbc0-4bb101b746f6.png">

<img width="653" alt="fastretransmit" src="https://user-images.githubusercontent.com/81469717/194870443-115936a6-c316-4392-a6df-ec5cab7cb050.png">


- 일반적으로 time-out 시간은 길다.
    - 따라서 잃어버린 패킷을 재전송하기까지 딜레이가 발생
- `**duplicated ACKs` 을 통해 lost segment를 감지해서 timeout 전에 재전송 = fast retransmit**
- 만약 sender가 같은 데이터의 `ACK` 을 세번 받으면( `triple duplicate ACK` ) `ACK` 을 받지 못한 segment를 재전송
- 불필요한 retransmission을 피하기 위해 3번 기다림

<br>

# TCP flow control

<img width="653" alt="flowcontrol" src="https://user-images.githubusercontent.com/81469717/194870808-689153ce-dc66-4d96-b032-d3adbc13a870.png">


### TCP flow control이 필요한 이유

> buffer가 가득 차서 segment가 drop되는 것을 방지 하기 위해
> 
- segment가 buffer로 들어오는 속도가 buffer에서 App으로 내보내는 속도보다 빠를 경우, buffer가 가득차서 뒤에 들어오는 segment가 드롭됨

 
### How? : `rwnd` 에 free buffer size 명시

<img width="653" alt="tcpflow_rwnd" src="https://user-images.githubusercontent.com/81469717/194871055-96e8ca40-561a-47b6-a1f0-809d2aea000e.png">

- `socket`이 만들어질 때, `socket`을 위한 `buffer`를 할당 (4096byte)
- sender에게 내보내는 `TCP header`의 `rwnd`에 `free buffer size`를 적어서 내보냄
- **sender는 unacked data(in-flight)를 reciever의 `rwnd` 값보다 적게 유지**함으로써 buffer over flow를 방지!

# Connection Management

<img width="653" alt="connectionmanagement" src="https://user-images.githubusercontent.com/81469717/194871469-f7c6b1e5-4faa-4588-b4b2-39da34d9c604.png">


handshake 통해 sender/receiver connection 설정

handshake 동안 서로의 파라미터(`seq #`, `receive buffer size`)를 교환, 협상함

## 2-way handshake는 문제가 있음

<img width="653" alt="2-wayhanshake_problem" src="https://user-images.githubusercontent.com/81469717/194871567-7f3a2a40-a4d4-4541-8a22-d1f2ae7ae33e.png">

- 서버가 establish를 하고 보냈을 때, 클라이언트가 살아있다는 보장이 없음

## TCP: 3-way handshake
<img width="653" alt="3way-handshake" src="https://user-images.githubusercontent.com/81469717/194871685-ef34b8ee-3112-4fc7-ba32-8760afd1ffde.png">

- 각자가 alive한 것을 확인한 시점에 ESTablish
- 초록색이 서버 사이드
- 빨간색이 클라이언트 사이드

![3way-handshakeFSM](https://user-images.githubusercontent.com/81469717/194871893-2961687c-73ed-4e18-bdac-65e51ec4a564.png)

<br>

### TCP : Closing Connection
- 양방향 스트림이 형성
- 연결 종료는 각 스트림별로 독립적으로 이루어짐
- `final segment` 를 내보내며(FINBit = 1), 스트림을 종료
- `final segment` 를 받은 쪽은 `ACk` 으로 응답
    - 이때 `ACK` 에 `final segment` 를 실어보낼 수 있음
- 클라이언트, 서버가 동시에 `final segment` 를 내보낼 수 도 있음
- 서버가 연결을 종료하면 소켓, 프로세스 모두 종료시킴

<br>

# TCP congestion control

: 네트워크에서 발생, 너무 많은 소스가 너무 많은 데이터를 너무 빨리 전송해서 처리하지 못해서 발생

- packet loss : 라우터에서 버퍼가 오버 플로우
- long delay : 라우터 버퍼에 오래동안 저장되어서

## Congestion control의 원인과 비용

- 소스(HostA)가 데이터를 전송하는 속도가 `a` 라면
- 실제 데이터를 전송하는 속도는 `a` + `재전송되는 데이터(packet loss, long delay)` = `a``임
- `C/2` : bottle neck link에서의 bandwidth
- `a`` 가 `C/2` 에 가까워질 수록 goodput은 감소하고, `C/2` 를 넘으면 0에 가까워짐

**원인**

- 재전송하는데 네트워크를 더 많이 사용함
- 불필요한 재전송 : 지연이 길어져서 타임아웃 발생에 따라 유발
- 버퍼에서 패킷이 드롭됨, 패킷을 위해 사용된 “upstream transmission capacity” 낭비

**비용**

- 불필요한 재전송을 위해 네트워크를 낭비하게되어, goodput, 네트워크 효율 떨어짐

<br>

## Approach towards Congestion Control

congestion control을 해결하기 위한 두가지 해결 방향

1. **end-to-end congestion control**
    - 네트워크는 직접적으로 피드백을 주지 않음, TCP가 스스로 핸들링
    - endsystem이 loss, delay를 관찰해 congestion control을 판단하고, 데이터 내보내는 속도를 낮춤
2. **network-assisted congestion control**
    - 라우터가 endsystem에게 피드백을 제공
    - congstion이 발생함을 **single bit로 알림  → SNA, DECbit, TCP/IP ECN, ATm**

<br>

# TCP Congestion Control 방법

> cwnd 값을 조절해 congestion을 컨트롤
> 
> - `cwnd(congestion window)`
>     - **sender가 ack을 받기 전에 보낼 수 있는 패킷양**

## 1. AIMD (합 증가 곱 감소,  Additive Increase & Multiplicative Decrease)

- sender는 transmission rate(cwnd)를 Loss가 발행하기전까지 점진적으로 증가 시킨다.
    - loss가 발생하기 전까지, 매 RTT마다 cwnd를 1MSS(maximum segement size)씩 증가시킴
- loss가 감지되면 cwnd를 반으로 줄임
    - 따라서 아래 그림과 같이 톱니 모양, tooth behavior를 보임
- sender의 전송 제한량 <= min(cwnd, rwnd)
    - 일반적인 경우, rwnd >> cwnd 이므로 보통 sender의 window size는 cwnd에 의해 결정된다
- 따라서 단위시간당 TCP가 보낼 수 있는 데이터의 양 = **cwnd / RTT (bytes/sec)**
- AIMD 방법 사용시 cwnd를 선형적으로 증가시키므로, 많은 데이터를 보내려면 시간이 걸린다. 속도가 느리다는 문제가 있다.  → Slow Start 등

<br>

## TCP가 cwnd를 조절하는 방법

### TCP Slow Start

- inital cwnd = 1MSS
- loss 발생 전까지, **매 RTT마다 cwnd를 2배 증가**
- ACK을 받을 때마다 cwnd를 증가 시킴
- 처음에 속도는 느리지만, 점차 폭발적으로 빨라짐
- 증가 시키다가 TCP Congestion Avodiance로 전환

### TCP Congestin Avodiance

- cwnd가 `ssthresh` 와 같아지면, 매 RTT마다 cwnd를 1씩 증가 시킴
    - ACK을 받으면 cwnd를 1씩 증가 시킴
- `ssthresh`
    - cwnd 증가 속도를 늦추기 시작하는 시점
    - 연결이 시작되면 `ssthresh` 는 OS에 의해 default value가 정해짐
    - loss가 발생하면 `ssthresh` = cwnd / 2

☝ - cwnd < ssthrseh : slow start ( 매 RTT마다 cwnd 2배 증가)
- cwnd ≥ ssthresh: congestion avoidance (매 RTT마다 cwnd 1씩 증가)

<br>

## TCP loss를 감지하고 대처하는 방법

TCP Tahoe, TCP Reno 최근에는 잘 사용하지 않는다고 한다. 

- loss 발생 유형에 따라 컨트롤 방법이 다름

### TCP RENO

- timeout : cwnd = 1MSS & slow start
- 3 duplicate ACK : cwnd = cwnd / 2 & linear gowth (1MSS씩 증가)

### TCP Tahoe

- timeout : cwnd = 1MSS & slow start
- 3 duplicate ACK : cwnd = 1MSS & slow start

<br>

# TCP performance mertics

## 1. TCP throughput

- slow start는 무시, 데이터를 항상 송신하고 있다고 가정
- W(window size) : loss 발생시 윈도 크기 라고 가정
    - 평균 window size = 3/4 W

## 2. TCP Fairness
