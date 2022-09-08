# Web and HTTP

- web page는 object들로 구성
- object는 HTML 파일, JPEG image, Java applet, audio file 가 될 수 있다.
- 웹 페이지는 참조를 갖고 있는 object가 포함된 HTML file로 구성이됨
- 각 object는 `URL` 로 다루어짐

## HTTP (HyperText Transfer Protocol)

- 클라이언트/서버 모델
    - client : 브라우저. HTTP 프로토콜을 사용해 요청하고 응답을 받아서 보여줌
    - server : 요청에 대한 응답으로 HTTP를 사용해서 object를 보냄

- 클라이언트는 서버어게 `TCP connection` 시작, 80번 포트 사용
- 서버는 TCP 연결을 승인함
- 브라우저(client)와 서버간 HTTP 메시지를 주고 받음
- TCP 연결은 종료됨
- HTTP는 `stateless` 임(서버에서 클라이언트 요청에 대한 정보를 저장하지 않음)
    - 둘 사이에 크래시가 나서 상태의 일관성이 깨질 수 있기 때문

## HTTP connections

### 1.. persistent HTTP

- 한 번의 TCP 연결에 다수의 객체를 전송 가능

### 2. non-persistent HTTP

- 한 번의 TCP 연결에 하나의 객체만 전송 가능
- 한 차례의 요청과 응답이 오가면 TCP연결이 닫힘
- 요청마다 TCP연결을 해야함

## non-persistent HTTP

**동작 방식**

1. 사용자가 URL을 브라우저에 입력
2. HTTP Client가 서버(www.someSchool.edu)의 80번 포트에 TCP 연결 요청을 보냄
3. HTTP 서버의 80번 포트는 연결을 기다리다가, 연결을 승인하고, 클라이언트에게 알림
4. 클라이언트가 HTTP Request message를 TCP 연결 socket으로  보냄
5. 서버가 받고, 요청한 객체를 포함한 response message를 소켓을 통해 보냄
6. 서버는 TCP connection close
7. 클라이언트는 html파일을 포함한 응답을 받음
8. html 파일을 파싱해서 10개의 jpeg 객체를 받기 위해서 다시 TCP 연결시작해서 위 과정 반복

- TCP Connection : 클라이언트, 서버에 socket을 만드는 것

 

### response time


: 유저가 url을 요청하고, 디스플레이 되기 전까지 소요되는 시간

= 2RTT + file transmission time 

- `RTT`(Round Trip Time) : 작은 패킷이 클라이언트로부터 서버까지 갔다가, 다시 클라이언트로 돌아올 때 까지의 시간 (packet이 작아서 mdium으로 밀어넣는 시간이 무의미할 정도의 작은 패킷)
- non-persistent HTTP
    - 객체당 2RTT가 필요
    - 브라우저는 parrallel TCP Connection을 오픈하기도 함 (소켓이 동시에 5개 생김)
    - OS입장에선 하나의 애플리케이션 당 여러개의 TCP 연결을 관리하는 overhead
    - 예) socket 할당. 각 TCP연결마다 buffer가 필요

## Persistent HTTP

- 1.0 이후의 버전 지원
- 응답을 받은 뒤에도 연결을 오픈 해둠
- 같은 클라이언트, 서버에 대한 객체에 대한 요청을 연속적으로 소켓으로 밀어넣음
- 최소한의 RTT

## HTTP Request message

- 종류: request, response
- ASCii 로 작성됨
- `request line` + `header` + `body` 파트로 구성
    - `header` 끝엔 추가로 식별자가 추가됨 ( 몇 줄인지 구분할 수 없기 때문에)

## HTTP Method

- GET : 리소스 조회
- POST : 요청 데이터 처리
- HEAD : GET과 동일하지만 바디를 제외한 상태줄과 헤더만 전송
- PUT : 리소스를 대체, 해당 리소스가 없으면 생성
- PATCH : 리소스 일부만 변경
- DELETE : 리소스 삭제


## HTTP Response message

- `status line` + `header line` + `body` 파트로 구성

## HTTP status code

- 200 OK
    - 요청 성공, 객체는 메시지에 정상적으로 포함됨
- 301 Moved Permanently
    - 요청한 객체가 이동됨, 새로운 위치가 메시지에 포함되어 전달
- 400 Bad Request
    - 잘못된 요청 양식
- 404 Not Found
    - 데이터가 서버에 없음
    - 505 HTPP Version Not Supported

<br>

## User-server state:  Cookies🍪

🍪: 클라이언트에대한 정보를 저장. 

### 목적
서버에서 클라이언트와의 트랜잭션 스토리를 유지하기 위해 사용

### 쿠키 매커니즘
1. HTTP Response 메시지에 쿠키 헤더 라인을 함께 보냄
2. 다음 HTTP Request 메시지에 쿠키 헤더 라인을 보냄
3. 쿠키 파일은 사용자의 호스트, browser에 의해 저장되고 관리
4. web site의 back-end database에도 저장 

### 예시
<img width="565" alt="스크린샷 2022-09-05 오전 12 35 39" src="https://user-images.githubusercontent.com/81469717/189075120-31e85cbd-d7b9-4d3a-bedf-74e9070d0b62.png">

1. 수잔이 인터넷에 접속
2. 어떠한 커머스 사이트를 처음으로 방문
3. 첫 HTTP 요청이 들어오면, 서버는 **유니크한 ID**를 생성해서 **쿠키 헤더라인(set-cookie: 1678)**에 실어서 보냄. 
4. 또한 backend database에서 ID를 위한 저장공간인 **entry**를 만듦, 여기엔 수잔과 주고 받은 **상호작용들이 기록됨**
5. 브라우저는 쿠키 파일에 ID를 저장
6. 커머스 사이트에 요청을 보낼 때마다 **쿠키 헤더 라인 (cookie: 1678)**을 함께 보냄
7. 서버에서 해당 ID에 대한 정보를 db에서 읽고, **cookie specific action**을 취함 

### Cookie의 기능

- user session state 사용 (Web e-mail, 장바구니 기능)
- authorization (인증)
- recommendations (웹 서버에서는 사용자의 데이터를 수집한 데이터 바탕으로 추천 또는 광고 제공)

### Cookie and Privacy
- 사이트가 사용자에 대한 정보를 학습할 수 있음
- 사이트에게 이름과 email을 제공

<br>

## Web Caches (proxy server)

### 목적
웹 캐시(proxy server)를 사용해 origin 서버 없이 object를 얻기 위함. 응답 시간 단축, 트래픽 줄여 비용 감축
<img width="565" alt="스크린샷 2022-09-05 오전 12 57 48" src="https://user-images.githubusercontent.com/81469717/189076507-9d338f41-0df9-4dd5-8c89-56b668f1229a.png">

- 사용자는 브라우저를 설정: 캐시를 통해 Web에 접근하도록
- 브라우저는 HTTP request를 로컬에 있는 캐시로 보냄
  - 캐시에 Object가 존재하면 해당 Object를 사용자에게 전송
  - 캐시에 Object없으면 orgin server에 요청
- 웹 캐시는 클라이언트, 서버 역할 모두 수행


- 응답 시간을 단축 가능
- 보통 기관, 회사, residentail ISP에서 사용
- 트래픽을 줄여서 비용을 줄 일 수 있음
- poor Content provider에서 유용
- P2P file sharing에서 유용

### Cache와 서버 데이터간 동기화 : Conditional GET

웹 캐시와 서버간 데이터의 동기화를 헤더를 사용해서 확인

<img width="549" alt="스크린샷 2022-09-08 오전 11 47 16" src="https://user-images.githubusercontent.com/81469717/189076794-a94088e1-9529-4407-aa66-953cae82ea3c.png">

- `last-modified-since <date>` 이후에 데이터가 바뀌었으면 서버는 새로운 데이터와 함께 `200 OK` 응답
- `last-modified-since <date>` 이후에 데이터가 바뀌지 않았으면 서버는 `304 Not Modified` 응답
- 매번 웹 캐시가 서버에게 확인을 요청하면 캐시를 사용하지 않을 때와 똑같이 트래픽을 사용할테니~

