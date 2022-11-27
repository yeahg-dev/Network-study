# Network Layer

---
<img width="605" alt="network layer" src="https://user-images.githubusercontent.com/81469717/197388071-c81499f8-eef2-41e2-95c4-0bea92210e66.png">

## 역할

- sending host로부터 `segment`를 receiving  host에게 전달
- 보내는 쪽에서 `segment` 를 `datagram` 으로 캡슐화함
- 받는 쪽은 `datagram` 을 다시 추출해서 `segment` 를 transport layer로 배달
- sending/receiving host, 네트워크 코어의 router에서 사용
    - host에는 routing 기능이 적음
    - router는 `datagram` 의 header field를 참조, 길을 기록해둔 table 참조해서 routing 수행

## 2가지 핵심 기능

<img width="605" alt="2 function" src="https://user-images.githubusercontent.com/81469717/197388090-b7349c6d-679e-4490-8db0-803ad8529e03.png">

1. routing : 출발지에서 목적지로의 경로를 계산하는 것 (라우팅 알고리즘) 
2. forwarding : header field의 목적지로 forwarding table을 참고해서 packet을 output port로 전달해주는 것

<br>

## Network service model

1. service for individual datagram
    - 배달을 보장
    - 지연 시간에 대한 보장 (예를들어, 40 msec delay이하로 지연)
2. service for a flow of datagrams
    - in-order datagram delivery
    - minimum bandwidth 보장

<br>

### 네트워크 코어 통식 방식

1. Circuit Switching network
2. Packet Switching network 
    - Virtual Circuit : network-layer의 connection service 제공
    - Datagram : network-layer의 connectionless service 제공 (인터넷✅)
- TCP/UDP의 connection-oriented/connectionless 서비스와 다른 점
    - service : host-to-host delievery
    - no choice : transport계층에서  virtual circuit과 datagram을 선택 불가, network layer가 사용하는 방식을 무조건적으로 따라야함
    - implementation : in network core

## 1. Virtual circuits

- call setup : 데이터를 흘려보내기 전
    - sender에서 네트워크 레이어에게 목적지와 함께 VC 요청 메시지 보냄
    - 경로 결정됨
- teardown : 데이터를 흘려보낸 후
- 각 packet은 VC identifier를 운반 (destination host address가 아닌)
- 경로상에 있는 각 router는 passing connection에 대한 state 정보를 유지
- link, router resources(bandwidth, buffers)는 VC에 할당

### VC implementation

<img width="605" alt="vc implementation" src="https://user-images.githubusercontent.com/81469717/197388167-dc263320-6ce9-4c0a-898a-72d2b93e60f4.png">

VC는 아래 3가지를 포함

1. **source에서 dest까지의 path**
2. **VC number** (VC identifier), one number for each link along path
3. router에 있는 **forwarding table에서의 entries**
- datagram에는 dest address대신 VC number를 기재
- VC number는 link마다 변경될 수 있음(새로운 VC number는 forwarding table에서 옴)
    - source에서 dest까지 VC번호가 동일하려면 더 긴 VC번호가 필요함
    - 왜냐하면 link마다 VC번호가 유니크해야하기 때문
    - 따라서 sending과 receiving router 간 link에 VC number를 할당함으로써 VC number에 대한 오버헤드를 줄일 수 있다.
    - router는 연결된 link들에 대한 VC번호를 유니크하게 유지하면 됨

### VC forwarding table

<img width="605" alt="vc forwarding table" src="https://user-images.githubusercontent.com/81469717/197388184-ba6c5e38-cf66-4ec2-9899-44465402d176.png">

- VC에 할당된 resource에 대한 정보(사용량 등, state)를 forwarding table에 유지해야함

### Signaling protocols

<img width="605" alt="signaling protocol" src="https://user-images.githubusercontent.com/81469717/197388217-5add1e17-8571-420c-b6e4-9fa5bbfed063.png">

- VC setup, maintain, teardown을 위해 사용되는 라우터간  프로토콜
- ATM, frame-delay, X.25 를 사용 (전화회사에서 만든 프로토콜들)
- Virtual cirucit은 인터넷에서는 사용되지 않고 있음

<br>

## 2. Datagram networks

- network 계층에서 call setup이 없음
- router는 end-to-end connection의 정보를 저장하지 않음
- router는 packet의 header에 있는 host address를 사용해서 포워딩함

### Datagram forwarding table

<img width="605" alt="datagram forwarding table" src="https://user-images.githubusercontent.com/81469717/197388300-f7f4cdee-adb2-43d4-9b36-c0bf83cf7071.png">
<img width="605" alt="forwarding table" src="https://user-images.githubusercontent.com/81469717/197388308-721b3541-8ea0-4664-be77-81680ff2214a.png">


- 라우팅 알고리즘으로 forwarding table을 작성
- address range별로 ouput link를 매칭
    - IP address에 대한 모든 정보를 저장한다면,  look up time + 메모리가 많이 필요함

<br>

## Datagram or VC
<img width="605" alt="datagram vs vc" src="https://user-images.githubusercontent.com/81469717/197388325-a910f0d8-0ba8-4f3f-b934-f61f2ffd2cf2.png">

### Internet - datagram

- 컴퓨터간에 데이터를 교환
- link type도 많음
    - link type마다 특성이 다름
    - 서비스를 단일화하기 어려움
- end system인 컴퓨터가 스마트함
    - 컴퓨터가 adapt, control, error recovery를 할 수 있음
    - 따라서 네트워크는 간단, 복잡한 작업은 엣지로 몰 수 있음

### ATM - Virtual Circuit

- 전화에서 발전
- 사람의 대화를 전달
    - 엄격한 타이밍, 신뢰성이 요구됨
- end system이 멍청함
    - 전화기
    - 네트워크가 복잡함

<br>

# Router architecture

Router 2가지 핵심 기능

- **라우팅** : run, routing algorithm/protocol (RIP, OSPF, BGP)
- **포워딩** : 다음 링크로 `datagram`을 전달

<img width="653" alt="routerarchitecture" src="https://user-images.githubusercontent.com/81469717/204135836-36526df3-adfa-4daa-8775-7d7c05bae6db.png">


## 구조

- **control plane** (software) : routing management, routing protocol 메시지를 주고 받는 곳,
    - routing processor: routing algorithm 실행, forwarding table을 계산, input port로 밀어넣어줌
- **data plane** (hardware) : 사용자의 데이터를 주고 받는 곳, 전달을 빨리 하기 위해 하드웨어로 구현
    - router input ports
    - high-seed switching fabric
    - router output ports

## Data plane - Input port functions
<img width="646" alt="input port" src="https://user-images.githubusercontent.com/81469717/204135886-9e6cb11b-cd9f-40e5-b70c-22bfcc684dd7.png">

- **line termination**
    - bit-level reception
    - bit를 모아서 frame을 형성
- **link layer protocol** (receive)
    - frame이 한 홉을 잘 건너왔는 지 확인, 링크 계층 프로토콜을 수행
- **lookup, forwarding** (queuing)
    - datagram dest를 참고하여 output port를 찾기위해 forwarding table을 lookup
- 목표
    - line speed, 데이가 미디엄을 통해 유입되는 속도와  동일하도록
- queuing
    
    - `datagram`을 switch fabric으로 forwarding하는 속도보다 빠르게 input port로 도착할 때, input queue에서  `datagram` queueing이 발생
    - input buffer overflow때문에 queueing delay, loss가 발생 가능
    - **Head-of-the-Line(HOL) blocking**
        - switch fabric은 `datagram`을 같은 output port로 동시에 포워딩하지 못함
        - 큐의 가장 앞에 같은 Outport를 가진 `datagram` 이 있으면 다른 `datagram` 을 포워딩하는 것이 막힘 (기다려야함)
        - 따라서 다음 `datagram` 포워딩이 one packet time만큼 지연됨
    

## Switching fabrics

<img width="646" alt="switchingfabric" src="https://user-images.githubusercontent.com/81469717/204135938-be0f079e-3206-4a2a-a355-d856a851db80.png">


input buffer에 있는 `datagram` 을 빠르게 output buffer로 포워딩하는 것이 핵심!

- **switching rate**
    - `packet` 을 input에서 output으로 전송하는 속도
    - 보통 input/output line rate의 배수로 결정됨
    - N inputs: switching rate * N times 가 가장 이상적
- 3가지 종류
    1. memory
    2. bus
    3. crossbar (interconnection network의 한 종류)

### 1) Switching - memory

<img width="646" alt="memory" src="https://user-images.githubusercontent.com/81469717/204135947-fedca684-437c-4d73-af26-ab9853d7b4b5.png">


- 1세대 라우터에서 사용
- CPU의 직접 제어에 의해 스위칭이 이루어짐
- `packet`이 시스템의 메모리로 복사됨
- 따라서 input port → memory → output port, system bus를 두번 거쳐야함
- memory bandwidth에 의해 속도가 결정됨

### 2) Switching - bus

<img width="646" alt="bus" src="https://user-images.githubusercontent.com/81469717/204135960-39e348de-5a2e-44a7-86ff-d8e39f2256ca.png">


- system bus를 두 번 거쳐야함, memory bandwidth에 의해 속도가 제한된다는 문제를 극복하기 위해 탄샌
- input port와 output port를 직접적으로 연결하는 버스를 switching fabric으로 사용
- bus에 한 번에 하나의 packet만 접근 가능
- 여러 input port에서 `packet` 이 유입될 경우, 경쟁 상태 발생
- bus bandwidth에 의해 switching speed가 제한됨
- 32 Gbps bus, Cisco 5600 : 기업 라우처에서 사용하기 충분한 스피드

### Switching - interconnection network

<img width="646" alt="ineterconnection" src="https://user-images.githubusercontent.com/81469717/204135967-b5afe509-82d9-464c-94bd-0cda6c4e3a0f.png">


- bus bandwidth limitation을 극복하기 위해 제안
- banyan networks, crossbar, other interconnection nets는 최초에 멀티 프로세서에서 프로세서를 연결하기 위해 개발됨
- `datagram` 을 고정 길이의 셀로 나눈 다음, fabric을 통해 셀을 스위칭

## Output ports

<img width="646" alt="output" src="https://user-images.githubusercontent.com/81469717/204135982-72155023-6600-4842-b625-66e9e4246e6b.png">


- switch fabric으로 받은 `datagram` 을 transmission medium으로 내보내는 역할
- **buffering**
    - transmission rate보다 fabric으로 `datagram` 이 빨리 도착할 때 필요
- **scheduling discipline**
    - 어떤 걸 먼저 transmission medium으로 보낼지 선택
    - 일반적인 경우는 FIFO
    - 프리미엄 서비스를 제공할 경우 선택 순위를 다르게 결정
- queueing   
    - output line speed보다 arrival rate가 빠를 때 버퍼가 일어남
    - output port buffer overflow때문에 queueing delay, loss가 발생함
    

### 버퍼 크기 어떻게 결정할까?
<img width="646" alt="buffer sizing" src="https://user-images.githubusercontent.com/81469717/204135791-c9548d68-714a-4dcc-8630-84cb8b30aa93.png">
