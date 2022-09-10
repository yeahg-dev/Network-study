# 전자 메일의 3가지 구성요소
- user agents
- mail server
- mail protocol

# User agents

- mail reader
- composing, editing, reading mail message
- ex) Outloook, iPhone Mail

# Mail Server

- 서버는 ougoing(보내는), incoming(받는) message를 저장
- mailbox : 사용자에게 배송되는 (incoming) 메시지를 저장, 사용자별로 mailbox가 존재
- message queue : 사용자가 발송하는(outgoing) 메시지를 담고 있음

# Mail Protocol
<img width="549" alt="스크린샷 2022-09-08 오후 12 02 05" src="https://user-images.githubusercontent.com/81469717/189486151-88fb7306-3240-41aa-81ef-2c7728a7d8cc.png">

다른 Mail agent를 사용하는 사용자간에 메일을 주고 받을 수 있음. (naver와 gmail이 주고받는 것 처럼)

1. 메일 송신인 앨리스는 Gmail, 수신인 밥은 Outlook 사용한다고 가정
2. 앨리스는 Gmail을 사용해서 메시지를 작성
3. Gmail은 메시지를 `SMTP`를 이용해 Gmail Server로 보냄
4. 앨리스의 메시지는 Gmail 서버의 메시지 큐에 쌓임
5. 밥의 mail server(Outlook)과 TCP 연결을 시작
6. Gmail 서버는 메시지 큐에 있는 메시지를 정기적으로 뽑아서 `SMTP`사용해 Receiver측 서버(밥네 Outlook 서버)로 보냄,  이는 `TCP` 연결 위에서 동작
7. Outlook server는 메시지를 밥의 mailbox에 저장
8. 밥은 이제 Outlook을 열어서 메시지 확인(POP3, IMAP,  HTTP) 

<img width="549" alt="스크린샷 2022-09-08 오후 12 27 28" src="https://user-images.githubusercontent.com/81469717/189486170-4a0511c3-f49e-449e-9d22-7f2bb27158da.png">

- mail server는 클라이언트 프로세스, 서버 프로세스 모두 처리함

# SMTP


- 믿을 수 있는 메시지 전송을 위해 TCP 사용. 포트 번호 25 사용
- 전송의 3단계
    1. handshaking (greeting)
    2. transfer of message (HTTP와 달리, 한 번 handshaking한 뒤 여러개의 메시지를 전송할 수 있음)
    3. closure 
- command, respsonse interaction (like HTTP, FTP)
    - **`command`** : ASCII text (클라이언트 메시지)
    - **`response`**: status code and phrase (서버 메시지)
- message는 7-bit ASCII 만 포함해야한다고 함 (지금은 새로운 프로토콜이 정의 되어서 영상, 사진을 보낼 수 있음)

## 정리

- SMTP는 persistent connection 사용
- SMTP는 header와 body의 메시지가 7-bit ASCII 요구
- SMTP 서버는 `.` 을 메시지 종료를 위해 사용

### HTTP 🆚 SMTP

- HTTP : pull
- SMTP : push (메일을 상대에게 보내는 과정에서만 사용)

## Mail access protocols

<img width="642" alt="스크린샷 2022-09-10 오후 9 40 58" src="https://user-images.githubusercontent.com/81469717/189486119-e775a4fe-f610-409f-af82-b5395282676f.png">

### POP3 protocol

- 먼저 만들어짐
- 단순한 프로토콜
- 세션간 stateless, 즉 서버나 클라이언트가 어떤 메시지를 어디에서 retreive했는지 저장하지 않음

- Client commands, server responses 로 통신

<download and delete mode>

- 과거에 보편적으로 사용함
1. 인증 단계
    - Client commands:
        - `user` : username
        - `pass` : user password
    - Server response
        - `+OK` : 성공
        - `-ERR`  : 실패
2. 트랜잭션 단계
    - `list` : message list 요청
    - `retr` : number로 메시지를 요청
    - `dele` : 삭제 요청
    - `quit` : 끝!

<download and keep mode>

- 위 과정에서 delete가 생략됨

### IMAP protocol

- POP3가 불편해서 만들어진 프로토콜
- 모든 메시지를 한 곳, 메일 서버에 저장
- 메일을 메일 서버의 메시지 박스에서 관리
- 메시지를 폴더로 분류하고 조직할 수 있음
- 세션간 메시지의 상태, 폴더가 유지됨
- POP3보다 복잡하고 무거운 프로토콜
