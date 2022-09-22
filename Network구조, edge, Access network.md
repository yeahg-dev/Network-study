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

<br>

## Hosts: send `packets` of data

<img width="685" alt="스크린샷 2022-06-17 오후 5 26 41" src="https://user-images.githubusercontent.com/81469717/174299791-853bf884-271e-44be-a96a-64c0386a8ad8.png">


**역할**

- application program을 호스트(실행)
- host는 발생된 `application message` 를 `packet` 으로 만들어  `access network` 로 전송
- 속도 = `L/R` (`L`: packet의 길이, bit크기, `R` : transmission rate)

## link의 종류

<img width="685" alt="스크린샷 2022-06-17 오후 5 31 19" src="https://user-images.githubusercontent.com/81469717/174299834-834999f6-b96c-4deb-96f0-6eb86d485e1c.png">


크게 두가지 종류

- **guided media**
    - 물리적 와이어 사용하는 link
    - cooper, fiber, coax
- **unguided media**
    - electronic magnetic ..
    - radio, wifi, cellular

### 1. guided media

<img width="685" alt="스크린샷 2022-06-17 오후 5 35 40" src="https://user-images.githubusercontent.com/81469717/174299885-8d33a1aa-54ee-4425-9786-3353585ef2db.png">


- `twisted pair(TP)` = `Cooper`
    - 이더넷에 사용됨
    - category5 (100Mbps, 1Gbps), category6 (10Gbps)
- `coax cable`
- `fiber optic cable`
    - `light pulse` 를 `glass fiber` 로 전달 (coax와 cooper는 전기 신호)
    - bandwidth가 매우 높음
    - 주변의 electromagnetic noise에 영향 받지 않음. 따라서 transmission bit error(손실)없이 안정적으로 멀리 감
    

### 2. unguided media

<img width="687" alt="스크린샷 2022-06-17 오후 5 41 16" src="https://user-images.githubusercontent.com/81469717/174300028-84b809d5-3055-47ba-be6b-4a574a09cf3d.png">


**장점**

- 물리적 link를 사용하지 않고도 사용 가능
- 양방향

**단점**

- 장애물에 부딪히면 시그널이 분산, 반사, 막혀질 수 있음
- 주변 noise에 민감 (radio signal끼리도 간섭이 있음)

**종류**

- terrestrial microwave
- LAN (wifi)
    - wifi 300m, cellular 수십km 커버리지 area가 다름
- wide-area (cellular)
- satelited
    - `geostationary` 와 `low earth orbiting` 을 비교했을 때 `geostationary` 가 커버리지 area가 넓음 (geo가 더 높이 띄우기 때문)
    - end-end delay가 큼 270msec

<br>

## Core에서 데이터 통신 방법 2가지

### 1. Circuit switching

<img width="687" alt="스크린샷 2022-06-17 오후 5 56 04" src="https://user-images.githubusercontent.com/81469717/174300246-dc55e5fe-3ca5-4096-b237-22bca0cbad36.png">


- 주로 전화 네트워크에서 사용 (전화연결음이 나오기전 지연되는 시간 = 회선을 결정하는 시간)
- `call` 이 오면 자원을 할당하고 예약하는 시스템
- 각 `link`는 `circuit(회로)`를 갖고 있음
- `call setup` : 경로 결정 + 경로상 자원 예약
- 데이터가 파이프 라인을 따라 전송됨
- **단점** :
    - 자원을 미리 분할해두고(cirtuit으로), 분할된 자원 중 한 덩어리를 call한 사용자에게 줌. 따라서 call이 오지않으면 가동되지 않는 circuit이 발생
    - 만약 점유하고 있는 circuit에서 정보를 보내지 않는 시간동안은 회선이 낭비되게 된다. ➡️ 비효율적 자원 사용
    - ➡️ data network에서는 불규칙적 request가 발생하는데 비효율적으로 자원을 사용하게 됨
- 자원 할당 방법
    - `FDM (Frequency Division Multiplexing)` : frequency를 분할
    - `TDM(Time Division Multiplexing)` : 시분할
    - `multiplexing` : 여러 신호가 단일 데이터 링크를 통해 동시에 전송되는 기술
<img width="687" alt="스크린샷 2022-06-17 오후 6 00 43" src="https://user-images.githubusercontent.com/81469717/174300294-67505610-fd89-44ae-9f23-7975347494e9.png">


### 2. Packet switching
<img width="687" alt="스크린샷 2022-06-17 오후 6 05 19" src="https://user-images.githubusercontent.com/81469717/174300347-0de53806-1e66-4866-ba37-296d42792480.png">


- 데이터를 전송하는 동안만 네트워크 자원을 사용
- `application message` 를 일정한 사이즈로 자른 `packet` 을 전송
- 각 packet은 `full link capacity` 를 사용해서 전송됨 (회선 효율성이 높음)
- 각 packet에는 목적지 주소가 명시되어 있음
- `router` 는 모든 packet이 도착할때 까지 packet을 일시적으로 `store` & 모두 도착하면 주소를 보고 `forward`
- 먼저 들어온 data가 모두 전송될 때까지 기다림
    

>💡 `비연결형 패킷 교환` : 각 패킷의 헤더에 소스주소, 목적지 주소, 총 조각수, 시퀀스 번호가 저장되어있기때문에 각 패킷을 다른 경로를 통해 따로 전송 가능


 <img width="687" alt="스크린샷 2022-06-17 오후 6 13 36" src="https://user-images.githubusercontent.com/81469717/174300404-0416e674-3420-485e-838b-ae85a398260c.png">

- **문제점** 
    - data가 모두 전송될 때까지 link를 독점
    - `queing delay`. `loss` 발생가능
    - 뽑아주는 속도보다 흘러들어오는 속도가 빠를 때 가능
    - packet은 다른 링크로 송신될 때까지 `queue` 에 저장됨
    - packet이 쌓이면 buffer가 꽉차게될 수 있음 , 이때 loss 발생 가능
    - `congestion` : `queing delay` , `loss` 가 심한 현산

<img width="687" alt="스크린샷 2022-06-17 오후 6 18 25" src="https://user-images.githubusercontent.com/81469717/174300516-9f37d301-d70a-4e6b-a4df-82c44950eb88.png">


### Packet switching 🆚 Circuit Switching

- packet switching이 더 많은 유저가 동시 사용할 수 있어서 resource sharing 측면에서 효율적
- 단, packet은 delay시간을 보장 불가. 스트리밍 서비스에서는 circuit같은 방법을 사용해야함(아직 해결되지 않은 문제)

<br>

## 인터넷 구조 : network of networks
- end system은 `access ISPs` 를 통해 인터넷에 연결됨
- `access ISPs` 끼리는 결국 모두 연결되어 있음
- 네트워크의 네트워크. 구조가 있고 복잡
- 경제적, 국가적 정책에 의해 진화해옴

- 직접연결한다면 O(N^2)의 매우 복잡한 복잡도, 확장성이 낮은 방법
- 따라서 네트워크들이 연결된 네트워크를 형성해서 유연하게 사용

### global ISP
<img width="687" alt="스크린샷 2022-06-17 오후 6 36 44" src="https://user-images.githubusercontent.com/81469717/174300829-7c40fa56-ac2f-4c9f-9b2f-37ac3f0bfc9c.png">

- 여러개의 gloabal ISP들이 존재함.
- 각 ISP끼리는 모두 연결되어 있음


### `IXP` & `peering link`

- `IXP(Internet Exchange Point)` : ISP끼리 연결하는 걸 담당하는 사업자
    - 보통 한 빌딩에 자사의 switche들을 두고 있음
    - 300여개의 IXP 존재
- `peering link`
    - 공급 ISP에게 아주 많은 트래픽 비용을 지불
    - 비용을 줄이기 위해 가까이 있는 동일 레벨의 `Regional ISP` 끼리 직접 연결, upstream을 통과하지 않음
    - `settlement-free` : 그리고 두 ISP간에는 비용을 지불하지 않음

### `regional ISP`
<img width="687" alt="스크린샷 2022-06-17 오후 6 39 02" src="https://user-images.githubusercontent.com/81469717/174300894-edf58e93-9db7-43a1-87d2-5f216a582a8d.png">
<img width="687" alt="스크린샷 2022-06-17 오후 6 41 01" src="https://user-images.githubusercontent.com/81469717/174300943-bfaea946-5bb5-4da2-85d7-1ced956afa82.png">

- `Access ISP` 들은 지역의 `Reginol ISP` 에 연결됨
- `Reginol ISP` 는 다시 `global ISP` 에게 연결
- multi-tire hierachy: 작은 지역의  `Reginol ISP` 는 또 다시 더 큰 지역의 `Reginol ISP` 에 연결
- `peering link` 로 cost, delay 감소 효과

### `Tier-1 ISP`
<img width="687" alt="스크린샷 2022-06-17 오후 6 45 24" src="https://user-images.githubusercontent.com/81469717/174301574-a0b075f1-ed3f-4643-81a9-306ce5827eba.png">


- 전세계를 묶는 건 `1st tier ISP`
- Sprint, AT&T, Orange등 15개 존재
- `POP(point of presence)` : 접속점

### `Multi-home`
<img width="687" alt="스크린샷 2022-06-17 오후 6 45 24" src="https://user-images.githubusercontent.com/81469717/174301000-d23af93b-bff0-402b-9877-7cd76e956708.png">


- Regional ISP가 여러개의 higher level ISP를 가지는 것
- 어떠한 공급자 ISP에 데이터 전송이 실패하더라도 다른 ISP를 통해 전송할 수 있기 때문에 안전성을 높여줌

### Content provider network
<img width="687" alt="스크린샷 2022-06-17 오후 7 07 06" src="https://user-images.githubusercontent.com/81469717/174301075-a9de3a0e-605d-477c-9866-3733afaaaec5.png">
<img width="687" alt="스크린샷 2022-06-17 오후 7 12 36" src="https://user-images.githubusercontent.com/81469717/174301092-cb86a1e3-fe77-4c9b-ae14-829637d4c0c7.png">


- 최근에 추가된 Content provider network
- Google, Microsoft, Akami등이 해당
- 많은 양의 데이터를 전송하기 때문에 ISP에게 막대한 비용을 지불해야함
- 따라서 30~50 데이터 센터끼리 연결한 network생성
- upper-tier ISP에게 지불하는 비용 절감효과
- 서비스 컨트롤 가능
- 1st tier-ISP, 일부 Reginoal ISP에 연결

총정리

<img width="687" alt="스크린샷 2022-06-17 오후 7 15 53" src="https://user-images.githubusercontent.com/81469717/174301179-c016034e-aad8-45b5-ad8e-5159fc3a9b33.png">
