# Network performance metrics
네트워크 성능을 판단하는 척도에는 3가지가 있다. 

1. packet delay
2. packet loss
3. throughput


## 1. packet delay

packet이 라우터에서 라우터로 전달되기까지 여러 단계를 거치게되는데, 4가지 종류의 delay가 발생할 수 있다

`nodal processing` ➡️ `queing` ➡️ `transmission` ➡️ `propagation`

<img width="729" alt="스크린샷 2022-06-18 오후 2 24 12" src="https://user-images.githubusercontent.com/81469717/175309323-f6cefb04-9656-46eb-85ae-f9c6be30b27a.png">
<img width="729" alt="스크린샷 2022-06-18 오후 2 27 11" src="https://user-images.githubusercontent.com/81469717/175309451-78240a05-4445-41c9-81cf-fde4d7a5b9fb.png">


### nodal processing
- bit error를 체크
- output link를 결정
- 평균적으로 msec 미만으로 발생

### queing delay 
- output link로 전송되기전 queue(buffer)에서 대기하는 시간
- 노드의 congestion에 따라 복잡도가 결정되기 때문에 delay시간이 가변적이다

### transmissioin delay
- 패킷을 queue에서 링크로 전달하는데 걸린 시간
- `L` : packet length (bits)
- `R` : link bandwidth (bps)
- delay 시간 = `L/R`

### propagation(전파) delay
- 링크에 실린 후 다음 라우터까지 이동하는데 걸린 시간
- `d` : length of physical link
- `s` : propagation spped in medium (m/sec)
- 전파 시간: `d/s`


## queing delay
<img width="729" alt="스크린샷 2022-06-18 오후 2 30 53" src="https://user-images.githubusercontent.com/81469717/175310034-d4480d8d-be47-41e9-8011-ad19d39992af.png">
<img width="729" alt="스크린샷 2022-06-18 오후 2 34 51" src="https://user-images.githubusercontent.com/81469717/175310071-efab7ade-c8bb-444a-91ba-9bdc34952205.png">

- 패킷은 라우터의 버퍼에 쌓인다
- output link로 뽑아내는 속도보다 빠른 속도로 패킷이 유입되면 패킷이 들어오면 delay 발생
- 그리고 패킷이 큐로부터 디큐될 때 delay 발생
- `R` : link bandwidth (bps)
- `L` : packet length (bits)
- `a` : average packet arrival rate
- `L * a` = 단위시간당 유입되는 트래픽의 양
- traffic intensity(트래픽 밀도): `L*a/R`
    - 0.7~0.8만되어도 무한정 delay가 발생한다


## 2. packet loss
<img width="729" alt="스크린샷 2022-06-18 오후 2 41 18" src="https://user-images.githubusercontent.com/81469717/175310478-9739fa40-18f4-4c4f-bf79-9b5b3bb6ac76.png">

- queue(buffer)는 유한한 용량을 가짐
- 큐가 꽉찼는데 패킷이 도착하면 그 패킷은 버려짐, loss가 발생
- loss가 발생하면 재전송 요청을 할 수 있지만, network 리소스가 낭비됨


## 3. throughput
<img width="729" alt="스크린샷 2022-06-18 오후 2 41 10" src="https://user-images.githubusercontent.com/81469717/175311114-0818b30f-f896-46b6-bd92-ab3dd68dd263.png">

- sender와 receiver 사이의 단위시간당 bit 전송량 (bits/time unit)
- band width가 작은 link에 의해 결정됨
- `bottleneck link` : 가장 대역폭이 작은 link, throughput을 결정
    - 일반적으로 edge쪽 link가 bottleneck link에 해당함
