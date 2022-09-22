# Protocol

- 방대하고 복잡해서 layering 되어있음
- layering의 장점
    - 각 요소를 식별하고, 각 부분의 관계를 정의하기 용의
    - 모듈화를 하면 유지, 업데이트가 쉬움
- 단점은?
    - 레이어간 기능 중복 발생 가능
    - 한 레이어가 다른 레이어의 정보를 필요로 한 경우가 있음.  레이어로 구분되어있기 때문에 optimal하게 사용하지 못하는 경우가 발생 (오버헤드 발생)

## Internet Protocol stack

<img width="729" alt="스크린샷 2022-06-18 오후 3 11 58" src="https://user-images.githubusercontent.com/81469717/178401583-f4e37f53-0333-4f61-adbe-173077ba1370.png">

각 계층은 하위 계층에게 일을 시킴

- `application` : 애플리케이션 데이터를 캡슐화해서 메시지 생성
    - 네트워크 애플리케이션 프로그램을 지원
    - FTP(파일 전송), SMTP(메일), HTTP(web)
    - `transport` 계층에게 process to process delivery 요청
- `transport` : source process → destination process 배달 역할
    - TCP, UDP
        - `network` 계층에게 host to host delivery 요청
- `network` : source host → destination host로 배달 역할, 길 안내(routing),
    - `link` 계층에게 하나의 홉을 이동하도록 요청. 다음 홉으로 패킷 포워딩
- `link` : 하나의 홉을 패스하는 역할
- `physical` : transmission medium통해 데이터를 실어나르는 역할

> **홉**
(hop)은 [컴퓨터 네트워크](https://ko.wikipedia.org/wiki/%EC%BB%B4%ED%93%A8%ED%84%B0_%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC)에서 출발지와 목적지 사이에 위치한 경로의 한 부분이다. 데이터 패킷은 [브리지](https://ko.wikipedia.org/wiki/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC_%EB%B8%8C%EB%A6%AC%EC%A7%80)
, [라우터](https://ko.wikipedia.org/wiki/%EB%9D%BC%EC%9A%B0%ED%84%B0), [게이트웨이](https://ko.wikipedia.org/wiki/%EA%B2%8C%EC%9D%B4%ED%8A%B8%EC%9B%A8%EC%9D%B4)를 거치면서 출발지에서 목적지로 경유한다. 패킷이 다음 [네트워크 장비](https://ko.wikipedia.org/w/index.php?title=%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC_%ED%95%98%EB%93%9C%EC%9B%A8%EC%96%B4&action=edit&redlink=1)로 이동할 때마다 홉이 하나 발생한다. **홉 카운트**(hop count)는 데이터가 출발지와 목적지 사이에서 통과해야 하는 중간 장치들의 개수를 가리킨다.
> 

<img width="729" alt="스크린샷 2022-06-18 오후 3 26 29" src="https://user-images.githubusercontent.com/81469717/178401617-105660c1-e821-406a-98b7-096d9fdffbdd.png">

데이터 전송 과정

- peer간 header를 통해 소통, 체크
- `PDU(protocol data unit)` : 데이터 통신에서 상위 [계층](https://ko.wikipedia.org/wiki/OSI_%EB%AA%A8%ED%98%95)이 전달한 데이터에 붙이는 제어정보
- `message` - `segment` - `datagram` - `frame`
    - transport : `message`에 트랜스포트 계층의   `header` 를 붙여서 `segment` 생성
    - network: `segment` 에 네트워크 계층의 `header`를 붙여서 `datagram` 생성
    - link : physical에게 하나의 홉을 건널 것을 부탁, `datagram` 에 링크 계층의 `header`를 붙인 `frame` 생성
    - physical: 비트를 모아서 `frame` 으로 다시 복구해서 link로 올려보냄
        - 링크마다 frame의 종류가 다르기 때문에, 다음 홉에 맞는 frame으로 만들어냄
- switch는 길찾기 기능 없음, network 계층을 가지지 않음
