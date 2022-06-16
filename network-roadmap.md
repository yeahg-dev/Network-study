**🌟수업 목표**
- 컴퓨터 네트워크의 계층적인 프로토콜 구조 이해 
- 네트워크의 자원 활용율을 높이고 사용자에 대한 서비스를 향상시킬 수 있는 네트워크 프로토콜 설계 및 분석 능력을 배양

<br>

### Intro
**Top-Down**

아래로 갈수록 물리적인 네트워크 컴포넌트에 연관,
위로 갈수록 사용자 어플리케이션에 가까운

5가지 계층이 존재함

- application layer
- transport layer
- network layer
- link layer
- physical layer

<br>

# Network 구조
<img width="692" alt="스크린샷 2022-06-16 오후 10 47 03" src="https://user-images.githubusercontent.com/81469717/174100963-f59b4ba3-0370-489b-8c7f-76411356126d.png">

- **network edge (가장자리)**
    - PC, server, laptop, smartphone
    - `host` 또는 `end system` 이라고 불림
    - running network apps
- **communication links**
    - fiber, copper, radio, satelite
    - transmisson rate: `bandwidth` (대역폭)
    - host와 router를 연결시켜줌
- **network core**
    - `routers` and `switches`(packet의 길을 안내해주는 특수한 컴퓨터)
    - network of networks

현대에선 다양한 end system이 생겨남: 통신이 가능한 냉장고, 토스트기, 사물인터넷 등

- 인터넷은 “network of networks”
    - 단일로 구성되어있지 않고 뭉터기(연결된 network)들로 구성됨
- `protocol` : 메시지를 주고 받는 규칙들
- Internet standards
    - 표준화가 매우 중요함
    - IETF(기관)에서 RFC(표준)을 규칙적으로 발표

<br>

# Protocol은 무엇인가?
- 주고 받는 메시지의 `format`, `order`(순서)를 정의
- 메시지 전송에 따라 취해야하는 `action` 을 정의

<br>

## network edge
- host에는 `client` and `server` 가 존재
- server는 보통 data center에 있음
- `access network`를 통해 endpoints들이 연결됨

<br>

## Access network
endsystem과 router를 연결하는 방법

- 성능 척도: `bandwitdth` (`transmisson rate`) : 초당 전송가능한 bit수
- 공유되는지에 따라 분류: `shared` vs `dedicated`
    - 보안, 속도에 영향

**종류**
-  digital subscriber line (DSL)
-  cable network
-  home network
-  Ethrenet(Enterprise access networks)
-  Wireless access network

### 1) `digital subscriber line (DSL)`

<img width="703" alt="스크린샷 2022-06-16 오후 11 11 31" src="https://user-images.githubusercontent.com/81469717/174102174-d4815e06-dd41-4e1e-880e-5eac6c27a7f1.png">

- kt, sk가 여기에 해당
- 전화회사에서 제공해주는 access net을 `DSL`이라고 지칭
- 컴퓨터는 `DSL modem` 에 연결, 전화는 `splitter` 에 연결됨.
- `splitter` 로 부터 voice, data가 각 집마다의 `dedicated line` (DSL phone line)을 통해 중앙국으로 보내지게됨
- 중앙국의 `DSLAM(DSL access multiplexer)`를 통해 voice는 `telephone network`로, data는 `ISP`로 보내지게됨
- 보통 업로드보다 다운로드를 많이 함
    - upstream transmissoin rate (보통 < 1Mbps)
    - download transmissoin rate (보통 < 10Mbps)

### 2) `cable network`

<img width="703" alt="스크린샷 2022-06-16 오후 11 20 10" src="https://user-images.githubusercontent.com/81469717/174102244-7a6fb8fb-ef4f-4cf6-9a9a-f53d0323c890.png">


- 케이블 회사
- `cable modem` 에 데스크탑이 연결. `splitter` 엔 티비가 연결
- `shared cable` 을 통해 데이터가 `cable headend` 로 전달(중앙국과 같은 역할), `coax cable` 이 사용
- `CMTS(Cable Modem Termination System)` 에서 데이터를 `ISP(인터넷)` 으로 전달

**HFC(Hybrid Fiber Coax)**

- `fiber link` & `coax cable` 의 조합
- 여러 cable headend가 묶여져 있음. 더 큰 대역폭을 제공하는 `fiber` link에 연결
- download가 upload보다 bandwidth가 큼

### 3) `home network`

<img width="703" alt="스크린샷 2022-06-16 오후 11 36 19" src="https://user-images.githubusercontent.com/81469717/174102304-aeafc637-41d6-4f38-80fe-26ecd5cef4b6.png">

`router` : 코어에는 `router` 가 있음.

- `wifi access point` , 데스크탑이 연결되어 있음
- `modem` 에 연결되어 있어 `cable` 또는 `DSL`을 통해 인터넷으로 연결.
- 집에 있는 여러 endsystem을 연결시켜주는 역할.

### 4) `Ethrenet(Enterprise access networks)`

<img width="703" alt="스크린샷 2022-06-16 오후 11 47 44" src="https://user-images.githubusercontent.com/81469717/174102372-97d4af0b-7b64-44fc-8744-42e22167621a.png">

- 학교나 회사가 사용

> `end system` s ➡️  `Ethernet switch` s ➡️  대표`router` ➡️ `dedicated line` ➡️ `ISP`

- `Ethernet switch` 가 한 건물/층/방에 있는 end system들을 연결
- 여러개의 `Ethernet switch` 이 기관 전체를 연결해주는 `router` 로 연결
- 기관 대표 `router` 가  `ISP(Internet Service Provider)`에 `dedicated line` 으로 직접  연결함
- 전화회사나 케이블회사가 `ISP` 에 가 연결해주지 않음. `dedicated line` 은 `ISP` 가 제공

> 💡 DSL과 cable은 ISP에 전화회사/케이블회사를 통해 연결. 반면 이더넷은 ISP에 직접 연결


### 5) `Wireless access network`

<img width="703" alt="스크린샷 2022-06-17 오전 12 01 25" src="https://user-images.githubusercontent.com/81469717/174102535-5780a7e7-214a-4db8-adff-ea3dec3795e2.png">

- `wifi` `LTE` 가 여기에 해당
- `shared` wireless access network가 end system이 router로 연결

**wireless LANs(wifi)**

- 빌딩안에서만 사용가능 (사용 영역이 한정적)
- 802.1 b/g (프로토콜) : b는 11Mbps, g는 54Mbps transmission rate

**wide-area wireless access(cellular)**

- `3G` `4G` `LTE` 망 = 셀룰러 네트워크
