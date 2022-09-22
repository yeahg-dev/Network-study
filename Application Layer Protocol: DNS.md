# DNS: domain name system

- `IP주소`와 `domain name(domain name)`으로 변환, 매핑해주는 시스템
- distributed database
- application layer protocol
    - 애플리케이션에서 사용되는 중요 기능이기 때문에 application layer protocol로 구현
    - 복잡성은 edge로 몰아내자는 원칙에 따라

[www.yahoo.com](http://www.yahoo.com) : host name = domain name

<br>

## DNS : Services, structure

- 주 기능:  `host name` ↔️ `IP address` 변환
- host aliasing(별명) 서비스
    - 서버의 host name을 외부에 다른 이름으로 대치해줌
    - 서버를 식별하기 위해 서버의 이름(canonical name)이 복잡할 수 있음. 하지만 외부에 알려줄 땐 회사의 서비스를 대표하는 이름(alias name)을 사용하는 경우가 있음
- mail server
- load distribution (부하 분산)
    - 웹 서버의 인기가 많을 때, 서버의 복사본을 가짐.
    - 모든 복사 서버를 하나의 alias로 통칭시켜 이를 통해 접근할 수 있도록함
- DNS를 중앙화하지 않는 이유는?
    - 애플리케이션에서 매우 중요한 서비스
    - single point of failure가 발생 가능
    - traffic volume이 너무 커짐
    - distant centralized database, 응답 시간이 너무 길어짐
    - maintenance

<br>

## DNS: 분산, 계층적 데이터베이스

<img width="639" alt="스크린샷 2022-09-10 오후 10 30 39" src="https://user-images.githubusercontent.com/81469717/189583850-7ae34e1d-c195-453b-9234-7c00b934160f.png">

3개 레벨로 분류

1. Root DNS Server
2. Top level DNS server (TLD server) : root DNS server 하위에 기관별(`.com` `.org`), 국가별(`.kr` `.fr`) DNS server가 분류
3. Autorithtive DNS server : 기관별 DNS server (`yahoo.com` DNS Server)
- Local DNS name server

### Local DNS name server

= default name server = proxy name server

- 계층 구조에 속하지 않음
- 각 ISP가 소유함 (residential ISP, company, university)
- web cache(proxy server)와 비슷한 개념
- 호스트가 만드는 DNS query가 Local DNS server로 보내짐
- Local DNS server가 정보를 가지고 있으면 그 정보를 응답으로 보내고, 없으면 heirachy DNS system으로 쿼리 전달

### 1. DNS : root name server

- 서버 계층구조에서 가장 높음
- 도메인을 담당하는 TLD 를 알고 있음
- 로컬 네임 서버가 가장 먼저 물어보는 서버

### 2. TLD : top-level domain (TLD) servers

- 기관 도메인에 속한 authroitiative DNS server를 알고 있음
- responsible for .com, .org, .net, .edu, .aero, .jobs, .museums, 모든 top-level country domian
- Network soultions: .com TLD
- Educause : .edu TLD
- 한국 인터넷 정보센터 : .kr

### 3. Authoritative DNS servers

- 기관은 자신만의 DNS server를 소유. 실제 hostname과 IP주소를 매핑하는 서버
- 기관 또는 서비스 제공자에 의해 유지됨

<br>

## DNS name resolution example : 2가지 방법

### 1. iterated query

- 질문을 받은 서버가 정보를 모를 경우, 매핑 정보를 알고 있는 **서버**를 알려줌

<img width="556" alt="스크린샷 2022-09-11 오후 2 29 39" src="https://user-images.githubusercontent.com/81469717/189583990-381fc521-dbee-46c1-94f6-09198c3302e3.png">


### 2. recursive query

- 질문 받은 서버가 매핑 정보를 찾아서 **IP주소**를 줌
- root server의 부하가 큼

<img width="556" alt="스크린샷 2022-09-11 오후 2 30 31" src="https://user-images.githubusercontent.com/81469717/189583958-dbbe3763-2949-4922-afc9-781e2d8b1413.png">

<br>

## DNS: caching, updating records

- DNS 서버들은 일시적으로 캐싱. 통해 응답 시간을 줄이고, 트래픽 양을 줄 일 수 있음
- 로컬은 보통 TLD 서버의 IP를 캐싱하고 있음. 따라서 root server를 거치지 않아도 됨
- 데이터의 최신성을 위해 매핑정보에 `TTL(time to leave, 데이터가 유효한 시간)`을 함께 보냄. 그럼에도 TTL중에 IP주소가 변경되면 잘못된 IP주소를 제공하게됨
- 따라서 IETF에서는 최신성 유지를 위해 IP주소가 변경이되면 update/notify mechanism이 발안되었음 (RFC 2136)

<br>

## DNS protocol, message

<img width="556" alt="스크린샷 2022-09-11 오후 2 43 03" src="https://user-images.githubusercontent.com/81469717/189584162-1e5c36cc-3b34-459e-a898-8281f39b7a4b.png">

- query와 reply 메시지는 동일한 형식, field들로 구성
- `identification` : 16bit, 숫자로 query와 Reply가 같은 번호를 사용. 번호 대조를 통해 알맞은 쿼리인지, 응답인지 판단
- `flags`
    - qeury or reply
    - recursion desired : recursion query를 원하는지
    - recursion available : recursion이 가능한지
    - reply is authoritative : 응답이 authoritative server인지

---

가변 길이 field

- `questions`
    - name, type filed for query
        - name: host name, type: A → host의 IP주소 알고 싶다는 쿼리
    - 길이에 대한 정보
- `answers`
    - RRs in response to query
    - 길이에 대한 정보
- `authority`
    - records of authoritative servers
    - 개수에 대한 정보
- `additional info`
    - 추가적 정보 (예, 도메인 네임 + MX 타입일 경우, 메일 서버의 IP주소를 추가적으로 제공할 수 있음)

### DNS Records

분산된 db에 resource records(RR)를 저장

RR format : (name, value, type, ttl)

name, value가 매핑 정보를 실고 있음.


- **type = A**
    - name : host name
    - value : IP 주소
- **type = NS**
    - name : domain (예. foo.com)
    - value : foo.com의 authoritative name server의 host name
    - 도메인의 기관 server(authoritative server)를 관리하는 TLD가 사용
- **type = CNAME**
    - name : canonical name(real)의 alis (예. www.ibm.com)
    - value: canonical name (예. servereast.backup2.ibm.com)
- **type = MX**
    - name : 도메인 네임
    - value: 도메인 네임의 mail server name


<br>

## 예시) 타트업이 DNS 서버에 IP주소를 등록하는 과정


1. 새로운 스타트업 “네트워크 유토피아”가 있다.
2. 회사 내의 authoritative DNS server 구매 
3. authoritative DNS Server의 이름과 IP주소를 DNS Registar(ex. Network Solutions, 한국 인터넷 정보 센터)에 등록
4. registarsms 2개의 RRs를 .com TLD server에 저장
    - (networkutopia.com, dns1.networkutopia.com, NS)
    - (dns1.networkutopia.com, 212.212.212.1, A)

### 처음으로 네트워크 유토피아의 IP주소를 얻는 과정
<img width="701" alt="스크린샷 2022-09-12 오후 2 36 44" src="https://user-images.githubusercontent.com/81469717/189584012-31dbf52f-d1cd-41cb-b14a-82cede1a68ff.png">
