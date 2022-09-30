
# UDP (User Datagrma Protocol)  [RFC 768]
"connectionless Transport Protocol"

### 특징
- “best effort” service : 네트워크에서 데이터를 유실할 수 있음, 순서가 바뀔 수 있음
- UDP segment를 주고 받기전, handshacking이 없음
- 각 UDP segment는 독립적

<br>
 
### UDP 사용하는 앱 / 프로토콜

- streaming multimedia app (loss tolerant, rate sensitive = minimum througput gaurantee)
- DNS (transaction type, connection이 overhead)
- SNMP (network 관리 protocol)
- reliable transfer over UDP
    - 데이터의 무결성이 중요한 앱 (앱 자체적으로 신뢰성을 체크하는 함수를 갖추고 있기 때문에, 에러가 발생했을 때 회복하는 로직이 구성)

<br>

## UDP : segment header

<img width="642" alt="udp header" src="https://user-images.githubusercontent.com/81469717/193210240-bca3b9d3-22b2-4d8e-8d3c-366f1ff550fb.png">

**장점**

- connection overhead가 없기 때문에 user message를 빨리 보낼수 있음
- 간단함 : connection state에 대한 정보 필요 없음
- header size가 작음 = traffic의 overhead가 작다
- congestion control이 없음 = 앱이 데이터를 생성하는 속도에 맞춰 데이터를 네트워크로 내보낼 수 있음

<br>

### UDP checksum
> segment 전송 오류 감지, Header의 checksum field에 기재

<img width="642" alt="checksum" src="https://user-images.githubusercontent.com/81469717/193211249-b9df59e1-51d6-4178-9722-4000ba4bc78e.png">

**sender**
- UDP segment header field를 16bit integer로 취급
- 두개의 16bit integer를 더함

**receiver**
- sender가 보낸 checksum과 receiver checksum이 같은지 확인을 통해서 비트 loss를 체크, 하지만 완벽하게 오류를 체크할 수 없음
- UDP 계층에서 오류 발생 진단을 위해 사용
- UDP는 이를 통해 오류가 발생 여부만 애플리케이션에게 전달, 에러 핸들링을 하지 않음
  
