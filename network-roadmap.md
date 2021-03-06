**๐์์ ๋ชฉํ**
- ์ปดํจํฐ ๋คํธ์ํฌ์ ๊ณ์ธต์ ์ธ ํ๋กํ ์ฝ ๊ตฌ์กฐ ์ดํด 
- ๋คํธ์ํฌ์ ์์ ํ์ฉ์จ์ ๋์ด๊ณ  ์ฌ์ฉ์์ ๋ํ ์๋น์ค๋ฅผ ํฅ์์ํฌ ์ ์๋ ๋คํธ์ํฌ ํ๋กํ ์ฝ ์ค๊ณ ๋ฐ ๋ถ์ ๋ฅ๋ ฅ์ ๋ฐฐ์

<br>

### Intro
**Top-Down**

์๋๋ก ๊ฐ์๋ก ๋ฌผ๋ฆฌ์ ์ธ ๋คํธ์ํฌ ์ปดํฌ๋ํธ์ ์ฐ๊ด,
์๋ก ๊ฐ์๋ก ์ฌ์ฉ์ ์ดํ๋ฆฌ์ผ์ด์์ ๊ฐ๊น์ด

5๊ฐ์ง ๊ณ์ธต์ด ์กด์ฌํจ

- application layer
- transport layer
- network layer
- link layer
- physical layer

<br>

# Network ๊ตฌ์กฐ
<img width="692" alt="แแณแแณแแตแซแแฃแบ 2022-06-16 แแฉแแฎ 10 47 03" src="https://user-images.githubusercontent.com/81469717/174100963-f59b4ba3-0370-489b-8c7f-76411356126d.png">

- **network edge (๊ฐ์ฅ์๋ฆฌ)**
    - PC, server, laptop, smartphone
    - `host` ๋๋ `end system` ์ด๋ผ๊ณ  ๋ถ๋ฆผ
    - running network apps
- **communication links**
    - fiber, copper, radio, satelite
    - transmisson rate: `bandwidth` (๋์ญํญ)
    - host์ router๋ฅผ ์ฐ๊ฒฐ์์ผ์ค
- **network core**
    - `routers` and `switches`(packet์ ๊ธธ์ ์๋ดํด์ฃผ๋ ํน์ํ ์ปดํจํฐ)
    - network of networks

ํ๋์์  ๋ค์ํ end system์ด ์๊ฒจ๋จ: ํต์ ์ด ๊ฐ๋ฅํ ๋์ฅ๊ณ , ํ ์คํธ๊ธฐ, ์ฌ๋ฌผ์ธํฐ๋ท ๋ฑ

- ์ธํฐ๋ท์ โnetwork of networksโ
    - ๋จ์ผ๋ก ๊ตฌ์ฑ๋์ด์์ง ์๊ณ  ๋ญํฐ๊ธฐ(์ฐ๊ฒฐ๋ network)๋ค๋ก ๊ตฌ์ฑ๋จ
- `protocol` : ๋ฉ์์ง๋ฅผ ์ฃผ๊ณ  ๋ฐ๋ ๊ท์น๋ค
- Internet standards
    - ํ์คํ๊ฐ ๋งค์ฐ ์ค์ํจ
    - IETF(๊ธฐ๊ด)์์ RFC(ํ์ค)์ ๊ท์น์ ์ผ๋ก ๋ฐํ

<br>

# Protocol์ ๋ฌด์์ธ๊ฐ?
- ์ฃผ๊ณ  ๋ฐ๋ ๋ฉ์์ง์ `format`, `order`(์์)๋ฅผ ์ ์
- ๋ฉ์์ง ์ ์ก์ ๋ฐ๋ผ ์ทจํด์ผํ๋ `action` ์ ์ ์

<br>

## network edge
- host์๋ `client` and `server` ๊ฐ ์กด์ฌ
- server๋ ๋ณดํต data center์ ์์
- `access network`๋ฅผ ํตํด endpoints๋ค์ด ์ฐ๊ฒฐ๋จ

<br>

## Access network
endsystem๊ณผ router๋ฅผ ์ฐ๊ฒฐํ๋ ๋ฐฉ๋ฒ

- ์ฑ๋ฅ ์ฒ๋: `bandwitdth` (`transmisson rate`) : ์ด๋น ์ ์ก๊ฐ๋ฅํ bit์
- ๊ณต์ ๋๋์ง์ ๋ฐ๋ผ ๋ถ๋ฅ: `shared` vs `dedicated`
    - ๋ณด์, ์๋์ ์ํฅ

**์ข๋ฅ**
-  digital subscriber line (DSL)
-  cable network
-  home network
-  Ethrenet(Enterprise access networks)
-  Wireless access network

### 1) `digital subscriber line (DSL)`

<img width="703" alt="แแณแแณแแตแซแแฃแบ 2022-06-16 แแฉแแฎ 11 11 31" src="https://user-images.githubusercontent.com/81469717/174102174-d4815e06-dd41-4e1e-880e-5eac6c27a7f1.png">

- kt, sk๊ฐ ์ฌ๊ธฐ์ ํด๋น
- ์ ํํ์ฌ์์ ์ ๊ณตํด์ฃผ๋ access net์ `DSL`์ด๋ผ๊ณ  ์ง์นญ
- ์ปดํจํฐ๋ `DSL modem` ์ ์ฐ๊ฒฐ, ์ ํ๋ `splitter` ์ ์ฐ๊ฒฐ๋จ.
- `splitter` ๋ก ๋ถํฐ voice, data๊ฐ ๊ฐ ์ง๋ง๋ค์ `dedicated line` (DSL phone line)์ ํตํด ์ค์๊ตญ์ผ๋ก ๋ณด๋ด์ง๊ฒ๋จ
- ์ค์๊ตญ์ `DSLAM(DSL access multiplexer)`๋ฅผ ํตํด voice๋ `telephone network`๋ก, data๋ `ISP`๋ก ๋ณด๋ด์ง๊ฒ๋จ
- ๋ณดํต ์๋ก๋๋ณด๋ค ๋ค์ด๋ก๋๋ฅผ ๋ง์ด ํจ
    - upstream transmissoin rate (๋ณดํต < 1Mbps)
    - download transmissoin rate (๋ณดํต < 10Mbps)

### 2) `cable network`

<img width="703" alt="แแณแแณแแตแซแแฃแบ 2022-06-16 แแฉแแฎ 11 20 10" src="https://user-images.githubusercontent.com/81469717/174102244-7a6fb8fb-ef4f-4cf6-9a9a-f53d0323c890.png">


- ์ผ์ด๋ธ ํ์ฌ
- `cable modem` ์ ๋ฐ์คํฌํ์ด ์ฐ๊ฒฐ. `splitter` ์ ํฐ๋น๊ฐ ์ฐ๊ฒฐ
- `shared cable` ์ ํตํด ๋ฐ์ดํฐ๊ฐ `cable headend` ๋ก ์ ๋ฌ(์ค์๊ตญ๊ณผ ๊ฐ์ ์ญํ ), `coax cable` ์ด ์ฌ์ฉ
- `CMTS(Cable Modem Termination System)` ์์ ๋ฐ์ดํฐ๋ฅผ `ISP(์ธํฐ๋ท)` ์ผ๋ก ์ ๋ฌ

**HFC(Hybrid Fiber Coax)**

- `fiber link` & `coax cable` ์ ์กฐํฉ
- ์ฌ๋ฌ cable headend๊ฐ ๋ฌถ์ฌ์ ธ ์์. ๋ ํฐ ๋์ญํญ์ ์ ๊ณตํ๋ `fiber` link์ ์ฐ๊ฒฐ
- download๊ฐ upload๋ณด๋ค bandwidth๊ฐ ํผ

### 3) `home network`

<img width="703" alt="แแณแแณแแตแซแแฃแบ 2022-06-16 แแฉแแฎ 11 36 19" src="https://user-images.githubusercontent.com/81469717/174102304-aeafc637-41d6-4f38-80fe-26ecd5cef4b6.png">

`router` : ์ฝ์ด์๋ `router` ๊ฐ ์์.

- `wifi access point` , ๋ฐ์คํฌํ์ด ์ฐ๊ฒฐ๋์ด ์์
- `modem` ์ ์ฐ๊ฒฐ๋์ด ์์ด `cable` ๋๋ `DSL`์ ํตํด ์ธํฐ๋ท์ผ๋ก ์ฐ๊ฒฐ.
- ์ง์ ์๋ ์ฌ๋ฌ endsystem์ ์ฐ๊ฒฐ์์ผ์ฃผ๋ ์ญํ .

### 4) `Ethrenet(Enterprise access networks)`

<img width="703" alt="แแณแแณแแตแซแแฃแบ 2022-06-16 แแฉแแฎ 11 47 44" src="https://user-images.githubusercontent.com/81469717/174102372-97d4af0b-7b64-44fc-8744-42e22167621a.png">

- ํ๊ต๋ ํ์ฌ๊ฐ ์ฌ์ฉ

> `end system` s โก๏ธย  `Ethernet switch` s โก๏ธย  ๋ํ`router` โก๏ธย `dedicated line` โก๏ธย `ISP`

- `Ethernet switch` ๊ฐ ํ ๊ฑด๋ฌผ/์ธต/๋ฐฉ์ ์๋ end system๋ค์ ์ฐ๊ฒฐ
- ์ฌ๋ฌ๊ฐ์ `Ethernet switch` ์ด ๊ธฐ๊ด ์ ์ฒด๋ฅผ ์ฐ๊ฒฐํด์ฃผ๋ `router` ๋ก ์ฐ๊ฒฐ
- ๊ธฐ๊ด ๋ํ `router` ๊ฐ  `ISP(Internet Service Provider)`์ `dedicated line` ์ผ๋ก ์ง์   ์ฐ๊ฒฐํจ
- ์ ํํ์ฌ๋ ์ผ์ด๋ธํ์ฌ๊ฐ `ISP` ์ ๊ฐ ์ฐ๊ฒฐํด์ฃผ์ง ์์. `dedicated line` ์ `ISP` ๊ฐ ์ ๊ณต

> ๐ก DSL๊ณผ cable์ ISP์ ์ ํํ์ฌ/์ผ์ด๋ธํ์ฌ๋ฅผ ํตํด ์ฐ๊ฒฐ. ๋ฐ๋ฉด ์ด๋๋ท์ ISP์ ์ง์  ์ฐ๊ฒฐ


### 5) `Wireless access network`

<img width="703" alt="แแณแแณแแตแซแแฃแบ 2022-06-17 แแฉแแฅแซ 12 01 25" src="https://user-images.githubusercontent.com/81469717/174102535-5780a7e7-214a-4db8-adff-ea3dec3795e2.png">

- `wifi` `LTE` ๊ฐ ์ฌ๊ธฐ์ ํด๋น
- `shared` wireless access network๊ฐ end system์ด router๋ก ์ฐ๊ฒฐ

**wireless LANs(wifi)**

- ๋น๋ฉ์์์๋ง ์ฌ์ฉ๊ฐ๋ฅ (์ฌ์ฉ ์์ญ์ด ํ์ ์ )
- 802.1 b/g (ํ๋กํ ์ฝ) : b๋ 11Mbps, g๋ 54Mbps transmission rate

**wide-area wireless access(cellular)**

- `3G` `4G` `LTE` ๋ง = ์๋ฃฐ๋ฌ ๋คํธ์ํฌ

<br>

## Hosts: send `packets` of data

<img width="685" alt="แแณแแณแแตแซแแฃแบ 2022-06-17 แแฉแแฎ 5 26 41" src="https://user-images.githubusercontent.com/81469717/174299791-853bf884-271e-44be-a96a-64c0386a8ad8.png">


**์ญํ **

- application program์ ํธ์คํธ(์คํ)
- host๋ ๋ฐ์๋ `application message` ๋ฅผ `packet` ์ผ๋ก ๋ง๋ค์ด  `access network` ๋ก ์ ์ก
- ์๋ = `L/R` (`L`: packet์ ๊ธธ์ด, bitํฌ๊ธฐ, `R` : transmission rate)

## link์ ์ข๋ฅ

<img width="685" alt="แแณแแณแแตแซแแฃแบ 2022-06-17 แแฉแแฎ 5 31 19" src="https://user-images.githubusercontent.com/81469717/174299834-834999f6-b96c-4deb-96f0-6eb86d485e1c.png">


ํฌ๊ฒ ๋๊ฐ์ง ์ข๋ฅ

- **guided media**
    - ๋ฌผ๋ฆฌ์  ์์ด์ด ์ฌ์ฉํ๋ link
    - cooper, fiber, coax
- **unguided media**
    - electronic magnetic ..
    - radio, wifi, cellular

### 1. guided media

<img width="685" alt="แแณแแณแแตแซแแฃแบ 2022-06-17 แแฉแแฎ 5 35 40" src="https://user-images.githubusercontent.com/81469717/174299885-8d33a1aa-54ee-4425-9786-3353585ef2db.png">


- `twisted pair(TP)` = `Cooper`
    - ์ด๋๋ท์ ์ฌ์ฉ๋จ
    - category5 (100Mbps, 1Gbps), category6 (10Gbps)
- `coax cable`
- `fiber optic cable`
    - `light pulse` ๋ฅผ `glass fiber` ๋ก ์ ๋ฌ (coax์ cooper๋ ์ ๊ธฐ ์ ํธ)
    - bandwidth๊ฐ ๋งค์ฐ ๋์
    - ์ฃผ๋ณ์ electromagnetic noise์ ์ํฅ ๋ฐ์ง ์์. ๋ฐ๋ผ์ transmission bit error(์์ค)์์ด ์์ ์ ์ผ๋ก ๋ฉ๋ฆฌ ๊ฐ
    

### 2. unguided media

<img width="687" alt="แแณแแณแแตแซแแฃแบ 2022-06-17 แแฉแแฎ 5 41 16" src="https://user-images.githubusercontent.com/81469717/174300028-84b809d5-3055-47ba-be6b-4a574a09cf3d.png">


**์ฅ์ **

- ๋ฌผ๋ฆฌ์  link๋ฅผ ์ฌ์ฉํ์ง ์๊ณ ๋ ์ฌ์ฉ ๊ฐ๋ฅ
- ์๋ฐฉํฅ

**๋จ์ **

- ์ฅ์ ๋ฌผ์ ๋ถ๋ชํ๋ฉด ์๊ทธ๋์ด ๋ถ์ฐ, ๋ฐ์ฌ, ๋งํ์ง ์ ์์
- ์ฃผ๋ณ noise์ ๋ฏผ๊ฐ (radio signal๋ผ๋ฆฌ๋ ๊ฐ์ญ์ด ์์)

**์ข๋ฅ**

- terrestrial microwave
- LAN (wifi)
    - wifi 300m, cellular ์์ญkm ์ปค๋ฒ๋ฆฌ์ง area๊ฐ ๋ค๋ฆ
- wide-area (cellular)
- satelited
    - `geostationary` ์ `low earth orbiting` ์ ๋น๊ตํ์ ๋ `geostationary` ๊ฐ ์ปค๋ฒ๋ฆฌ์ง area๊ฐ ๋์ (geo๊ฐ ๋ ๋์ด ๋์ฐ๊ธฐ ๋๋ฌธ)
    - end-end delay๊ฐ ํผ 270msec

<br>

## Core์์ ๋ฐ์ดํฐ ํต์  ๋ฐฉ๋ฒ 2๊ฐ์ง

### 1. Circuit switching

<img width="687" alt="แแณแแณแแตแซแแฃแบ 2022-06-17 แแฉแแฎ 5 56 04" src="https://user-images.githubusercontent.com/81469717/174300246-dc55e5fe-3ca5-4096-b237-22bca0cbad36.png">


- ์ฃผ๋ก ์ ํ ๋คํธ์ํฌ์์ ์ฌ์ฉ (์ ํ์ฐ๊ฒฐ์์ด ๋์ค๊ธฐ์  ์ง์ฐ๋๋ ์๊ฐ = ํ์ ์ ๊ฒฐ์ ํ๋ ์๊ฐ)
- `call` ์ด ์ค๋ฉด ์์์ ํ ๋นํ๊ณ  ์์ฝํ๋ ์์คํ
- ๊ฐ `link`๋ `circuit(ํ๋ก)`๋ฅผ ๊ฐ๊ณ  ์์
- `call setup` : ๊ฒฝ๋ก ๊ฒฐ์  + ๊ฒฝ๋ก์ ์์ ์์ฝ
- ๋ฐ์ดํฐ๊ฐ ํ์ดํ ๋ผ์ธ์ ๋ฐ๋ผ ์ ์ก๋จ
- **๋จ์ ** :
    - ์์์ ๋ฏธ๋ฆฌ ๋ถํ ํด๋๊ณ (cirtuit์ผ๋ก), ๋ถํ ๋ ์์ ์ค ํ ๋ฉ์ด๋ฆฌ๋ฅผ callํ ์ฌ์ฉ์์๊ฒ ์ค. ๋ฐ๋ผ์ call์ด ์ค์ง์์ผ๋ฉด ๊ฐ๋๋์ง ์๋ circuit์ด ๋ฐ์
    - ๋ง์ฝ ์ ์ ํ๊ณ  ์๋ circuit์์ ์ ๋ณด๋ฅผ ๋ณด๋ด์ง ์๋ ์๊ฐ๋์์ ํ์ ์ด ๋ญ๋น๋๊ฒ ๋๋ค.ย โก๏ธย ๋นํจ์จ์  ์์ ์ฌ์ฉ
    - โก๏ธย data network์์๋ ๋ถ๊ท์น์  request๊ฐ ๋ฐ์ํ๋๋ฐ ๋นํจ์จ์ ์ผ๋ก ์์์ ์ฌ์ฉํ๊ฒ ๋จ
- ์์ ํ ๋น ๋ฐฉ๋ฒ
    - `FDM (Frequency Division Multiplexing)` : frequency๋ฅผ ๋ถํ 
    - `TDM(Time Division Multiplexing)` : ์๋ถํ 
    - `multiplexing` : ์ฌ๋ฌ ์ ํธ๊ฐ ๋จ์ผ ๋ฐ์ดํฐ ๋งํฌ๋ฅผ ํตํด ๋์์ ์ ์ก๋๋ ๊ธฐ์ 
<img width="687" alt="แแณแแณแแตแซแแฃแบ 2022-06-17 แแฉแแฎ 6 00 43" src="https://user-images.githubusercontent.com/81469717/174300294-67505610-fd89-44ae-9f23-7975347494e9.png">


### 2. Packet switching
<img width="687" alt="แแณแแณแแตแซแแฃแบ 2022-06-17 แแฉแแฎ 6 05 19" src="https://user-images.githubusercontent.com/81469717/174300347-0de53806-1e66-4866-ba37-296d42792480.png">


- ๋ฐ์ดํฐ๋ฅผ ์ ์กํ๋ ๋์๋ง ๋คํธ์ํฌ ์์์ ์ฌ์ฉ
- `application message` ๋ฅผ ์ผ์ ํ ์ฌ์ด์ฆ๋ก ์๋ฅธ `packet` ์ ์ ์ก
- ๊ฐ packet์ `full link capacity` ๋ฅผ ์ฌ์ฉํด์ ์ ์ก๋จ (ํ์  ํจ์จ์ฑ์ด ๋์)
- ๊ฐ packet์๋ ๋ชฉ์ ์ง ์ฃผ์๊ฐ ๋ช์๋์ด ์์
- `router` ๋ ๋ชจ๋  packet์ด ๋์ฐฉํ ๋ ๊น์ง packet์ ์ผ์์ ์ผ๋ก `store` & ๋ชจ๋ ๋์ฐฉํ๋ฉด ์ฃผ์๋ฅผ ๋ณด๊ณ  `forward`
- ๋จผ์  ๋ค์ด์จ data๊ฐ ๋ชจ๋ ์ ์ก๋  ๋๊น์ง ๊ธฐ๋ค๋ฆผ
    

>๐ก `๋น์ฐ๊ฒฐํ ํจํท ๊ตํ` : ๊ฐ ํจํท์ ํค๋์ ์์ค์ฃผ์, ๋ชฉ์ ์ง ์ฃผ์, ์ด ์กฐ๊ฐ์, ์ํ์ค ๋ฒํธ๊ฐ ์ ์ฅ๋์ด์๊ธฐ๋๋ฌธ์ ๊ฐ ํจํท์ ๋ค๋ฅธ ๊ฒฝ๋ก๋ฅผ ํตํด ๋ฐ๋ก ์ ์ก ๊ฐ๋ฅ


 <img width="687" alt="แแณแแณแแตแซแแฃแบ 2022-06-17 แแฉแแฎ 6 13 36" src="https://user-images.githubusercontent.com/81469717/174300404-0416e674-3420-485e-838b-ae85a398260c.png">

- **๋ฌธ์ ์ ** 
    - data๊ฐ ๋ชจ๋ ์ ์ก๋  ๋๊น์ง link๋ฅผ ๋์ 
    - `queing delay`. `loss` ๋ฐ์๊ฐ๋ฅ
    - ๋ฝ์์ฃผ๋ ์๋๋ณด๋ค ํ๋ฌ๋ค์ด์ค๋ ์๋๊ฐ ๋น ๋ฅผ ๋ ๊ฐ๋ฅ
    - packet์ ๋ค๋ฅธ ๋งํฌ๋ก ์ก์ ๋  ๋๊น์ง `queue` ์ ์ ์ฅ๋จ
    - packet์ด ์์ด๋ฉด buffer๊ฐ ๊ฝ์ฐจ๊ฒ๋  ์ ์์ , ์ด๋ loss ๋ฐ์ ๊ฐ๋ฅ
    - `congestion` : `queing delay` , `loss` ๊ฐ ์ฌํ ํ์ฐ

<img width="687" alt="แแณแแณแแตแซแแฃแบ 2022-06-17 แแฉแแฎ 6 18 25" src="https://user-images.githubusercontent.com/81469717/174300516-9f37d301-d70a-4e6b-a4df-82c44950eb88.png">


### Packet switching ๐ย Circuit Switching

- packet switching์ด ๋ ๋ง์ ์ ์ ๊ฐ ๋์ ์ฌ์ฉํ  ์ ์์ด์ resource sharing ์ธก๋ฉด์์ ํจ์จ์ 
- ๋จ, packet์ delay์๊ฐ์ ๋ณด์ฅ ๋ถ๊ฐ. ์คํธ๋ฆฌ๋ฐ ์๋น์ค์์๋ circuit๊ฐ์ ๋ฐฉ๋ฒ์ ์ฌ์ฉํด์ผํจ(์์ง ํด๊ฒฐ๋์ง ์์ ๋ฌธ์ )

<br>

## ์ธํฐ๋ท ๊ตฌ์กฐ : network of networks
- end system์ `access ISPs` ๋ฅผ ํตํด ์ธํฐ๋ท์ ์ฐ๊ฒฐ๋จ
- `access ISPs` ๋ผ๋ฆฌ๋ ๊ฒฐ๊ตญ ๋ชจ๋ ์ฐ๊ฒฐ๋์ด ์์
- ๋คํธ์ํฌ์ ๋คํธ์ํฌ. ๊ตฌ์กฐ๊ฐ ์๊ณ  ๋ณต์ก
- ๊ฒฝ์ ์ , ๊ตญ๊ฐ์  ์ ์ฑ์ ์ํด ์งํํด์ด

- ์ง์ ์ฐ๊ฒฐํ๋ค๋ฉด O(N^2)์ ๋งค์ฐ ๋ณต์กํ ๋ณต์ก๋, ํ์ฅ์ฑ์ด ๋ฎ์ ๋ฐฉ๋ฒ
- ๋ฐ๋ผ์ ๋คํธ์ํฌ๋ค์ด ์ฐ๊ฒฐ๋ ๋คํธ์ํฌ๋ฅผ ํ์ฑํด์ ์ ์ฐํ๊ฒ ์ฌ์ฉ

### global ISP
<img width="687" alt="แแณแแณแแตแซแแฃแบ 2022-06-17 แแฉแแฎ 6 36 44" src="https://user-images.githubusercontent.com/81469717/174300829-7c40fa56-ac2f-4c9f-9b2f-37ac3f0bfc9c.png">

- ์ฌ๋ฌ๊ฐ์ gloabal ISP๋ค์ด ์กด์ฌํจ.
- ๊ฐ ISP๋ผ๋ฆฌ๋ ๋ชจ๋ ์ฐ๊ฒฐ๋์ด ์์


### `IXP` & `peering link`

- `IXP(Internet Exchange Point)` : ISP๋ผ๋ฆฌ ์ฐ๊ฒฐํ๋ ๊ฑธ ๋ด๋นํ๋ ์ฌ์์
    - ๋ณดํต ํ ๋น๋ฉ์ ์์ฌ์ switche๋ค์ ๋๊ณ  ์์
    - 300์ฌ๊ฐ์ IXP ์กด์ฌ
- `peering link`
    - ๊ณต๊ธ ISP์๊ฒ ์์ฃผ ๋ง์ ํธ๋ํฝ ๋น์ฉ์ ์ง๋ถ
    - ๋น์ฉ์ ์ค์ด๊ธฐ ์ํด ๊ฐ๊น์ด ์๋ ๋์ผ ๋ ๋ฒจ์ `Regional ISP` ๋ผ๋ฆฌ ์ง์  ์ฐ๊ฒฐ, upstream์ ํต๊ณผํ์ง ์์
    - `settlement-free` : ๊ทธ๋ฆฌ๊ณ  ๋ ISP๊ฐ์๋ ๋น์ฉ์ ์ง๋ถํ์ง ์์

### `regional ISP`
<img width="687" alt="แแณแแณแแตแซแแฃแบ 2022-06-17 แแฉแแฎ 6 39 02" src="https://user-images.githubusercontent.com/81469717/174300894-edf58e93-9db7-43a1-87d2-5f216a582a8d.png">
<img width="687" alt="แแณแแณแแตแซแแฃแบ 2022-06-17 แแฉแแฎ 6 41 01" src="https://user-images.githubusercontent.com/81469717/174300943-bfaea946-5bb5-4da2-85d7-1ced956afa82.png">

- `Access ISP` ๋ค์ ์ง์ญ์ `Reginol ISP` ์ ์ฐ๊ฒฐ๋จ
- `Reginol ISP` ๋ ๋ค์ `global ISP` ์๊ฒ ์ฐ๊ฒฐ
- multi-tire hierachy: ์์ ์ง์ญ์  `Reginol ISP` ๋ ๋ ๋ค์ ๋ ํฐ ์ง์ญ์ `Reginol ISP` ์ ์ฐ๊ฒฐ
- `peering link` ๋ก cost, delay ๊ฐ์ ํจ๊ณผ

### `Tier-1 ISP`
<img width="687" alt="แแณแแณแแตแซแแฃแบ 2022-06-17 แแฉแแฎ 6 45 24" src="https://user-images.githubusercontent.com/81469717/174301574-a0b075f1-ed3f-4643-81a9-306ce5827eba.png">


- ์ ์ธ๊ณ๋ฅผ ๋ฌถ๋ ๊ฑด `1st tier ISP`
- Sprint, AT&T, Orange๋ฑ 15๊ฐ ์กด์ฌ
- `POP(point of presence)` : ์ ์์ 

### `Multi-home`
<img width="687" alt="แแณแแณแแตแซแแฃแบ 2022-06-17 แแฉแแฎ 6 45 24" src="https://user-images.githubusercontent.com/81469717/174301000-d23af93b-bff0-402b-9877-7cd76e956708.png">


- Regional ISP๊ฐ ์ฌ๋ฌ๊ฐ์ higher level ISP๋ฅผ ๊ฐ์ง๋ ๊ฒ
- ์ด๋ ํ ๊ณต๊ธ์ ISP์ ๋ฐ์ดํฐ ์ ์ก์ด ์คํจํ๋๋ผ๋ ๋ค๋ฅธ ISP๋ฅผ ํตํด ์ ์กํ  ์ ์๊ธฐ ๋๋ฌธ์ ์์ ์ฑ์ ๋์ฌ์ค

### Content provider network
<img width="687" alt="แแณแแณแแตแซแแฃแบ 2022-06-17 แแฉแแฎ 7 07 06" src="https://user-images.githubusercontent.com/81469717/174301075-a9de3a0e-605d-477c-9866-3733afaaaec5.png">
<img width="687" alt="แแณแแณแแตแซแแฃแบ 2022-06-17 แแฉแแฎ 7 12 36" src="https://user-images.githubusercontent.com/81469717/174301092-cb86a1e3-fe77-4c9b-ae14-829637d4c0c7.png">


- ์ต๊ทผ์ ์ถ๊ฐ๋ Content provider network
- Google, Microsoft, Akami๋ฑ์ด ํด๋น
- ๋ง์ ์์ ๋ฐ์ดํฐ๋ฅผ ์ ์กํ๊ธฐ ๋๋ฌธ์ ISP์๊ฒ ๋ง๋ํ ๋น์ฉ์ ์ง๋ถํด์ผํจ
- ๋ฐ๋ผ์ 30~50 ๋ฐ์ดํฐ ์ผํฐ๋ผ๋ฆฌ ์ฐ๊ฒฐํ network์์ฑ
- upper-tier ISP์๊ฒ ์ง๋ถํ๋ ๋น์ฉ ์ ๊ฐํจ๊ณผ
- ์๋น์ค ์ปจํธ๋กค ๊ฐ๋ฅ
- 1st tier-ISP, ์ผ๋ถ Reginoal ISP์ ์ฐ๊ฒฐ

์ด์ ๋ฆฌ

<img width="687" alt="แแณแแณแแตแซแแฃแบ 2022-06-17 แแฉแแฎ 7 15 53" src="https://user-images.githubusercontent.com/81469717/174301179-c016034e-aad8-45b5-ad8e-5159fc3a9b33.png">
