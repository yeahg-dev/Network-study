# Network layer

<img width="646" alt="networklayer overview" src="https://user-images.githubusercontent.com/81469717/208239735-046bd31c-6d40-4107-b0e0-62a4f78b2f34.png">


3개의 프로토콜

- routing protocol (control plane)
    - 경로 선택
    - RIP, OSPF, BGP
- IP protocol (data plane)
    - 데이터 전송 관련된 사항 정의
    - addressing convention, datagram format, packet handling convention
- ICMP protocol
    - error reporting
    - router signaling

<br>

# IP Protocol

---

## IP datagram format

<img width="646" alt="IPdatagramformat" src="https://user-images.githubusercontent.com/81469717/208239755-f35d5728-f908-42d9-a7fc-78a6de152f61.png">


- header : 고정 20bytes field + option field (가변 길이)
- `ver`
    - IP Protocol version number
- `header length`
    - header의 길이, receiver에서 header를 파악하는데 사용
- `type of service`
    - 서비스에 따라 경로 선택, FiFO이므로 실제로 이 필드는 잘 사용하지 않음
- `length`
    - header 포함 전체 길이 (bytes)
- `time to live`
    - maximum number of remaing hops
    - source에서 네트워크 크기에 따라 값을 결정
    - 각 라우터를 건널때마다 1씩 감소됨, 0이되면 드롭시킴
    - ghost datagram 생성을 방지
- `upper layer`
    - payload에 실려있는 datagram이 TCP/UDP 인지 표시
    - 네트워크 계층에서 TCP/UDP 중 어떤 프로세스로 올려보내야할지 결정
- `header checksum`
- `32 bit source IP address`
- `32 bit destination IP address`
- `16-bit identifier`
    - 같은 fragment인지 식별
- `flags`
    - 다음 fragment가 있으면 1, 없으면 0
- `fragment offset`
    - fragment 순서 식별
    - data크기를 8byte를 나눈 값
- `optional` : record route taken(datagram의 경로 정보), timestamp, list of router to visit

<br>

## IP fragmentation, reassembly

<img width="644" alt="IPfragmentation" src="https://user-images.githubusercontent.com/81469717/208239822-8b4b1a31-f327-4f45-8039-94baf7ea133c.png">


- **fragmentation : 라우터에서 큰 datagram을 분할 하는 것**
- 네트워크 링크는 물리적 특성으로 인해 MTU(max transfer size)를 가짐 = 링크 계층의 최대 frame 사이즈 결정
- 링크 타입마다 MTU가 다름. 따라서 큰 IP datagram을 작은 datagram으로 쪼개짐
- 각 작은 datagram은 독립적으로 목적지에 전달되고, 최종 목적지에서 reassemble됨
    - 네트워크에서의 복잡성을 최대한 엣지로 몰아내기 위함
- IP header bits는 datagram을 식별하고, 관련 datagram의 순서를 맞추기 위해 사용됨
- `16-bit identifier` , `flags` , `fragment offset` fragmentation마다 해당 필드 내용이 수정됨, reassemble 위해 사용됨
    

## IPv4 addresssing

<img width="644" alt="IPv4" src="https://user-images.githubusercontent.com/81469717/208239842-16c9ee12-b834-4f41-a09c-7720db120103.png">


- IP address : 32-bit identifier for host, router interface
- 8bit씩 .으로 4파트로 나누어 표현, subnet part + host part
- interface : host/router와 physical link간 연결
    - router는 보통 multiple interface를 가짐
    - host는 하나 또는 2개의 인터페이스를 가짐 (와이어 이더넷, 와이파이)
- host interface와 router interface가 직접 연결된 것으로 가정하고 설명
    - 실제로는 유선 이더넷은 이더넷 스위치를 통해 host interface와 router interface와 연결
    - 실제로는 무선 와이파이는 WfFi base station을 통해 host interface와 router interface와연결됨

<br>

## Subnets

라우터의 개입없이 서로 통신할 수 있는 (연결되어 있는) 디바이스 인터페이스

<img width="644" alt="subnets" src="https://user-images.githubusercontent.com/81469717/208239900-9fce2792-1dfb-407c-b09d-96c4b2e6d83c.png">

- subnet part: high order bits (27bits)
- host part : low order bits (8bits)
- 동일한 subnet에 속한 interface를 식별하기 위한 방법
    - 인터페이스를 host와 router로 부터 분리?
    - subnet part에 할당된 비트 수 표시 (예. /24)

<br>

# IP addressing

---

## : CIDR (Classless Inter Domain Routing)

- 기존 IP 주소는 클래스 기반이었지만 비효율적이어서 제안됨
- subnet portion을 가변적으로 쓸 수 있도록함

## host, subnet이 IP 주소가 할당되는 방법

### 1. file의 system admin에  hard-coded로 저장

- Windows: contorl-panel > network > configuration > tcp/ip > properties
- UNIX: /etc/rc.config
- 고정적, 직접 입력
- IP 충돌의 문제

### 2. DHCP: Dynamic Host Configuration Protocol (동적 호스트 구성 프로토콜)’

> DHCP서버가 클라이언트에게 IP주소를 일정 시간(임대 기간) 임대해주는 시스템

- DHCP서버는 ISP로 부터 할당받은 일정한 IP 대역을 가짐
- DHCP서버로 부터 다이나믹하게 주소를 받음
- 라우터 장비가 DHCP 서버역할을 하는 경우도 있음
- “plug-and-play” 방식 (연결 후 작동)
- host가 네트워크에 접속할 때 동안만 IP주소 유효, 재사용 가능
- 네트워크에 참가하고 싶은 참여자가 많고, 이동이 잦고, 주소가 제한된 시간에만 필요한 경우 IP주소를 효율적으로 사용 가능
- 임대기간 이후에도 계속 할당IP주소를 사용하려면 IP Address Renewal을 DHCP서버에 요청
- 단말이 임대 받은 IP주소가 더 이상 필요하지 않으면 IP Address Release를 수행
- 가정 인터넷 접속 네트워크 및 무선랜(LAN), 모바일 폰의 사용자는 상시 네트워크에 접속해있는 것이 아니므로 이 방법 사용

<img width="630" alt="DHCP" src="https://user-images.githubusercontent.com/81469717/208239998-2bea4f25-9afd-4ecf-aa45-120ce8f42abe.png">

- application layer protocol로 구현 (transportLayer : UDP)
    1. `DHCP discover` msg : 호스트가 DHCP 서버를 발견하기 위해 서브넷에 broadcast (optional)
    2. `DHCP offer` msg : DHCP 서버가 서브넷에게 자신을 알기위해 하는 broadcast (optional)
    3. `DHCP request` msg : 호스트가 자신이 사용할 ip주소 요청
    4. `DHCP ack` msg : DHCP 서버가 응답으로 ip주소를 전달
- 인터넷에 접속하기 전에 DHCP 클라이언트 프로그램이 자동으로 시작
    - `0.0.0.0.68` default IP 주소
    - `255.255.255.255.67` 서브넷 broadcast 주소, 67번 포트 사용
    - yiaddrr : 클라이언트를 위해 할당하는 IP주소
    - transactionID: 서버는 여러 DHCP msg가 들어오기 때문에 어떤 메시지의 응답인지 식별가능, 클라이언트도 여러 서버로부터 offer를 받을 수 있기 때문에 식별에 사용
    - lifetime: 클라이언트가 IP주소를 사용할 수 있는 유효시간(초 단위)
- DHCP는 IP 주소 외 추가적인 정보를 제공
    - default gateway IP 주소, first hop router의 주소 (여기로 패킷을 포워딩 해라)
    - local DNS server의 이름과 IP주소
    - network mask(indicating network 🆚 host 비율)


### Subnet에서 사용할 IP주소는 어떻게 얻을까?

<img width="630" alt="subnetIP" src="https://user-images.githubusercontent.com/81469717/208240097-e1363ea0-1f65-45e1-8f82-638ddedc0c33.png">

- 서브넷을 연결해주는 ISP로부터 특정 주소 블럭 할당
- 앞의 23비트를 subnet IP로 할당
- 추가의 3개의 비틀(초록색)는 기관을 식별하는데 사용

 

### 계층적 IP주소는 router에 어떤 영향을 미치는가

<img width="630" alt="heirachialIP" src="https://user-images.githubusercontent.com/81469717/208240111-4975ad4f-c485-457f-af7e-e03fbfc0b99e.png">

- forwarding table을 만들기위해 라우터는 각 IP주소 위치를 알아야함
- ISP는 자신에게 속한 기관들의 IP주소를 advertising함
    - 목적지 IP주소가 ~(subnet IP)로 시작하는 주소는 ~ISP에게 보내라라고 advertising
- 기관이 ISP를 변경한다면?
    - ISP가 전입받은 기관의 IP주소를 추가로 advertising
    - 인터넷은 가장 긴 prefix에 대한 advertising을 이용해서 forwarding을 함
    

### Longest prefix matching

<img width="606" alt="longestprefix" src="https://user-images.githubusercontent.com/81469717/208240154-15972eed-5b40-4667-9617-f810a130be36.png">

- 개별 IP주소 별로하지 않고 **서브넷 prefix별**로 advertising
- 라우터에서는 **subnet prefix**가 동일한 IP에 대해서는 forwarding table의 entry를 하나로 관리, aggregate함
- 라우터는 지리적으로 먼 목적지일 수록 더 크게 aggregation을 수행
    - 가까울 수록 긴 prefix
    - 멀수록 짧은 prefix
- 따라서 목적지 IP주소를 forwarding table과 비교해서 longest prefix matching이 되는 인터페이스로 패킷을 보냄


### ISP는 어떻게, 누구로부터 prefix를 할당 받는가?

- 한국 인터넷 정보 센터
    - 국내 ISP에게 prefix를 할당하는 역할
    - DNS 관리
    - domain name할당
    - 중복 해결


## NAT: network address translation (네트워크 주소 변환)

> IP주소를 효율적으로 사용하기 위함
사설 네트워크에 속한 여러개의 호스트가 하나의 공인 IP주소를 사용 가능하다록 변환하는 시스템
> 

<img width="606" alt="nat" src="https://user-images.githubusercontent.com/81469717/208240163-e3b0d0ba-4d3e-4150-9f8c-44798ae54f42.png">

- 공인 IP : 인터넷에서 사용되는 IP
- 사설 IP: subnet 내부에서만 유통되는 IP
1. 인터넷의 공인IP 주소를 절약 가능
    - 외부와 연결될 때, local network는 하나의 공인 IP 주소만 사용
    - 따라서 여러 대의 호스트가 하나의 공인 IP 주소 사용 가능
2. 인터넷이란 공공망과 연결되는 사설망을 보호 가능
    - 내부 네트워크에서 사용하는 IP 주소와 외부에 드러나는 주소를 다르게 유지할 수 있기 때문에 내부 네트워크의 보안에 기여
    - 인터넷망과 연결하는 장비인 라우터에 NAT를 설치하면 라우터는 자신에게 할당된 공인 IP주소만 외부로 알려지게 할 수 있고, 내부에서는 사설IP주소만 사용하도록 하여 이를 서로 변환 시켜줌
    - 외부 침입자가 공격하기 위해서는 사설망의 내부 사설 IP주소를 알아야 하기 때문에 공격이 불가능해짐, 따라서 내부 네트워크 보호 가능
- 호스트가 datagram을 라우터로 보낼 때 source IP, port#에 사설 IP를 적어서 보내면 NAT는 공인 IP, 새로운 port#로 변환해서 보냄
- NAT translation table : (사설 IP, port#) - (공인IP, 새로운 port#)의 매핑 정보 기록
    <img width="606" alt="nat table" src="https://user-images.githubusercontent.com/81469717/208240180-1b8b4a31-d657-4739-9050-fbe4052e9134.png">

- 16-bit port-number field를 가짐
    - 따라서 동시에 60,000여개의 호스트를 연결 가능
- 단점
    - 라우터는 Network layer에서 사용하기 위해 설계
    - 라우터에서 transport layer의 segment를 조작하기 때문에 overhead 발생
    - application design에 문제가 발생 가능
    - 서버는 사용 불가
        - 클라이언트의 컨택 없이는 매핑 테이블이 생길 수 없고, 클라이언트는 서버의 IP를 모르면 컨택할 수 없기 때문에 결국 NAT 사용 불가
        - 해결: static NAT(수동적으로 테이블을 작성), UPNP
- IP주소 부족 문제는 IPv6을 사용함으로써 해결 가능
