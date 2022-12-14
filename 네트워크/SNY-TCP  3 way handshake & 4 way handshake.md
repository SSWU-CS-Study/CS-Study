# [TCP] 3 way handshake & 4 way handshake

<br>

### ๐ Index

- [TCP(Transmission Control Protocol)๋](#tcptransmission-control-protocol๋)
- [TCP์ ์ฐ๊ฒฐ ์ค์  ๋ฐ ํด์  ๊ณผ์ ](#tcp์-์ฐ๊ฒฐ-์ค์ -๋ฐ-ํด์ -๊ณผ์ )
  - [3 way handshake๋?](#3-way-handshake๋)
  - [4 way handshake๋?](#4-way-handshake๋)

- [์ฐธ๊ณ ](#์ฐธ๊ณ )
  - [ํฌํธ(PORT) ์ํ ์ ๋ณด](#ํฌํธport-์ํ-์ ๋ณด)
  - [TCP ํค๋ ์์ ํ๋๊ทธ ์ ๋ณด](#tcp-ํค๋-์์-ํ๋๊ทธ-์ ๋ณด)

<br>

<br>

## TCP(Transmission Control Protocol)๋

TCP๋ ๋คํธ์ํฌ ๊ณ์ธต ์ค **์ ์ก ๊ณ์ธต(4๊ณ์ธต)์์ ์ฌ์ฉํ๋ ํ๋กํ ์ฝ**๋ก์, ์ฅ์น๋ค ์ฌ์ด์ ๋ผ๋ฆฌ์ ์ธ ์ ์์ ์ฑ๋ฆฝ(establish)ํ๊ธฐ ์ํ์ฌ ์ฐ๊ฒฐ์ ์ค์ ํ์ฌ **์ ๋ขฐ์ฑ์ ๋ณด์ฅํ๋ ์ฐ๊ฒฐํ ์๋น์ค**์ด๋ค.

#### TCP์ ํน์ง

- ์ธํฐ๋ท ์์์ ๋ฐ์ดํฐ๋ฅผ ๋ฉ์ธ์ง์ ํํ(**์ธ๊ทธ๋จผํธ**๋ผ๋ ๋ธ๋ก ๋จ์)๋ก ๋ณด๋ด๊ธฐ ์ํด IP์ ํจ๊ป ์ฌ์ฉํ๋ ํ๋กํ ์ฝ์ด๋ค.
- **์ฐ๊ฒฐํ ์๋น์ค**๋ก, ๊ฐ์ ํ์  ๋ฐฉ์์ ์ ๊ณตํ๋ค. `3-way handshaking` ๊ณผ์ ์ ํตํด ์ฐ๊ฒฐ์ ์ค์ ํ๊ณ , `4-way handshaking`์ ํตํด ์ฐ๊ฒฐ์ ํด์ ํ๋ค. (๋ฐ์ดํฐ๋ฅผ ์ ์กํ๊ธฐ ์ ์ ๋ผ๋ฆฌ์  ์ฐ๊ฒฐ์ด ์ค์ ๋๋๋ฐ, ์ด๋ฅผ ๊ฐ์ํ์ ์ด๋ผ๊ณ  ํ๋ค, UDP๋ ๋ฐ์ดํฐ๊ทธ๋จ ๊ตํ ๋ฐฉ์)

- ํ๋ฆ์ ์ด ๋ฐ ํผ์ก์ ์ด๋ฅผ ์ ๊ณตํ๋ค.
  - ํ๋ฆ ์ ์ด : ๋ฐ์ดํฐ๋ฅผ ์ก์ ํ๋ ๊ณณ๊ณผ ์์ ํ๋ ๊ณณ์ ๋ฐ์ดํฐ ์ฒ๋ฆฌ ์๋๋ฅผ ์กฐ์ ํ์ฌ ์์ ์์ ๋ฒํผ ์ค๋ฒํ๋ก์ฐ๋ฅผ ๋ฐฉ์งํ๋ ๊ฒ
  - ํผ์ก ์ ์ด : ๋คํธ์ํฌ ๋ด์ ํจํท ์๊ฐ ๋์น๊ฒ ์ฆ๊ฐํ์ง ์๋๋ก ๋ฐฉ์งํ๋ ๊ฒ
- ๋์ ์ ๋ขฐ์ฑ์ ๋ณด์ฅํ๋ค.
- UDP ๋ณด๋ค ์๋๊ฐ ๋๋ฆฌ๋ค.
- ์ ์ด์ค(Full-Duplex), ์ ๋์ (Point to Point) ๋ฐฉ์์ด๋ค.
  - ์ ์ด์ค : ์ ์ก์ด ์๋ฐฉํฅ์ผ๋ก ๋์์ ์ผ์ด๋  ์ ์๋ค.
  - ์ ๋์  : ๊ฐ ์ฐ๊ฒฐ์ด ์ ํํ 2๊ฐ์ ์ข๋จ์ ์ ๊ฐ์ง๊ณ  ์๋ค.
  - ๋ฉํฐ ์บ์คํ์ด๋ ๋ธ๋ก๋ ์บ์คํ์ ์ง์ํ์ง ์๋๋ค.
- ์ฐ์์ฑ๋ณด๋ค ์ ๋ขฐ์ฑ์๋ ์ ์ก์ด ์ค์ํ  ๋ ์ฌ์ฉํ๋ค.

<br>

## TCP์ ์ฐ๊ฒฐ ์ค์  ๋ฐ ํด์  ๊ณผ์ 

TCP๋ ๊ณ์ํด์ ํต์ ์ ํ์ธํ๋ฉด์ ๋ฐ์ดํฐ๋ฅผ ์ ์กํ๋ค. ์ด์  ํต์  ๊ณผ์ ์ ์ดํด๋ณด์.

์ด๋ฌํ ์ฐ๊ฒฐ์ ๋งบ๋ ๊ณผ์ ์ 3-Way Handshaking, ์ฐ๊ฒฐ์ ํด์ ํ๋ ๊ณผ์ ์ 4-Way Handshaking๋ผ ํ๊ณ , ์ด๋ฅผ ๋ง๊ทธ๋๋ก ๋ง๋์ ๋ฐ๊ฐ๋ค๊ณ  ์์ํ๋๊ณผ์ , ํค์ด์ง๊ธฐ ์  ์์๋ฅผ ํ๋ ๊ณผ์ ์ด๋ผ๊ณ  ์๊ฐํ๋ฉด ์ดํด๊ฐ ๋น ๋ฅด๋ค.

#### 3 way handshake๋?

TCP ํต์ ์ ์ด์ฉํ์ฌ ๋ฐ์ดํฐ๋ฅผ ์ ์กํ๊ธฐ ์ํด **๋คํธ์ํฌ ์ฐ๊ฒฐ์ ์ค์  (Connection Establish) ํ๋ ๊ณผ์ **์ผ๋ก, TCP๋ฅผ ์ด์ฉํ ๋ฐ์ดํฐ ํต์ ์ ํ  ๋ ํ๋ก์ธ์ค์ ํ๋ก์ธ์ค๋ฅผ ์ฐ๊ฒฐํ๊ธฐ ์ํด **๊ฐ์ฅ ๋จผ์  ์ํ๋๋ ๊ณผ์ **์ด๋ค.

์์ชฝ ๋ชจ๋ ๋ฐ์ดํฐ๋ฅผ ์ ์กํ  ์ค๋น๊ฐ ๋์๋ค๋ ๊ฒ์ ๋ณด์ฅํ๊ณ , ์ค์ ๋ก ๋ฐ์ดํฐ ์ ๋ฌ์ด ์์ํ๊ธฐ ์ ์ ํ ์ชฝ์ด ๋ค๋ฅธ ์ชฝ์ด ์ค๋น๋์๋ค๋ ๊ฒ์ ์ ์ ์๋๋ก ํ๋ค.  ์ฆ, TCP/IP ํ๋กํ ์ฝ์ ์ด์ฉํ์ฌ ํต์ ์ ํ๋ ์์ฉ ํ๋ก๊ทธ๋จ์ด ๋ฐ์ดํฐ๋ฅผ ์ ์กํ๊ธฐ ์ ์ ๋จผ์  ์ ํํ ์ ์ก์ ๋ณด์ฅํ๊ธฐ ์ํด ์๋๋ฐฉ ์ปดํจํฐ์ ์ฌ์ ์ ์ธ์์ ์๋ฆฝํ๋ ๊ณผ์ ์ ์๋ฏธํ๋ค.

<br>

<img src="https://k.kakaocdn.net/dn/U4p6B/btrmFMGAGRM/tYtqWSbBJpTCLb4lWNMVJk/img.png" width=300/>

1. ํด๋ผ์ด์ธํธ๊ฐ ์๋ฒ์๊ฒ ์์ฒญ ํจํท์ ๋ณด๋ธ๋ค : `์ฐ๊ฒฐ ์์ฒญ`
2. ์๋ฒ๊ฐ ํด๋ผ์ด์ธํธ์ ์์ฒญ์ ๋ฐ์๋ค์ด๋ ํจํท์ ๋ณด๋ธ๋ค : `ํ์ธ + ์ฐ๊ฒฐ ์์ฒญ`
3. ํด๋ผ์ด์ธํธ๋ ์ด๋ฅผ ์ต์ข์ ์ผ๋ก ์๋ฝํ๋ ํจํท์ ๋ณด๋ธ๋ค : `ํ์ธ`

<br>

๐ **์์** : A ํ๋ก์ธ์ค(Client)๊ฐ B ํ๋ก์ธ์ค(Server)์ ์ฐ๊ฒฐ์ ์์ฒญ

<img src= "https://gmlwjd9405.github.io/images/network/3-way-handshaking.png" width=700/>



1)A->B : `SYN`

- ์ ์ ์์ฒญ ํ๋ก์ธ์ค A๊ฐ ์ฐ๊ฒฐ ์์ฒญ ๋ฉ์ธ์ง ์ ์ก (SYN)
- ์ก์ ์๊ฐ ์ต์ด๋ก ๋ฐ์ดํฐ๋ฅผ ์ ์กํ  ๋ Sequence Number๋ฅผ ์์์ ๋๋ค ์ซ์๋ก ์ง์ ํ๊ณ , SYN ํ๋๊ทธ ๋นํธ๋ฅผ 1๋ก ์ค์ ํ ์ธ๊ทธ๋จผํธ๋ฅผ ์ ์กํ๋ค.
- ํฌํธ ์ํ : A = `CLOSED`, B = `LISTEN`

2)B->A : `SYN` + `ACK`

- ์ ์ ์์ฒญ์ ๋ฐ์ ํ๋ก์ธ์ค B๊ฐ ์์ฒญ์ ์๋ฝํ์ผ๋ฉฐ, ์ ์ ์์ฒญ์ ํ๋ก์ธ์ค์ธ A๋ ํฌํธ๋ฅผ ์ด์ด ๋ฌ๋ผ๋ ๋ฉ์ธ์ง ์ ์ก (SYN + ACK)
- ์์ ์๋ Acknowledgement Number ํ๋๋ฅผ `Sequence Number +1`๋ก ์ง์ ํ๊ณ , SYN๊ณผ ACK ํ๋๊ทธ ๋นํธ๋ฅผ 1๋ก ์ค์ ํ ์ธ๊ทธ๋จผํธ๋ฅผ ์ ์กํ๋ค.
- ํฌํธ ์ํ : A = `CLOSED`, B = `SYN_RCV`

3)A->B : ACK

- ํฌํธ ์ํ : A = `ESTABLISHED`, B = `SYN_RCv`
- ๋ง์ง๋ง์ผ๋ก ์ ์ ์์ฒญ ํ๋ก์ธ์ค A๊ฐ ์๋ฝ ํ์ธ์ ๋ณด๋ด ์ฐ๊ฒฐ์ ๋งบ์(ACK)
- ์ด๋, ์ ์กํ  ๋ฐ์ดํฐ๊ฐ ์์ผ๋ฉด ์ด ๋จ๊ณ์์ ๋ฐ์ดํฐ๋ฅผ ์ ์กํ  ์ ์๋ค.
- ํฌํธ ์ํ : A = `ESTABLISHED`, B = `ESTABLISHED`

<br>

#### 4 way handshake๋?

TCP์ **์ฐ๊ฒฐ์ ํด์  (Connection Termination) ํ๋ ๊ณผ์ **์ด๋ค.

<br>

๐ **์์** : A ํ๋ก์ธ์ค(Client)๊ฐ B ํ๋ก์ธ์ค(Server)์ ์ฐ๊ฒฐ ํด์ ๋ฅผ ์์ฒญ

<img src="https://gmlwjd9405.github.io/images/network/4-way-handshaking.png" width=700/>

1)A->B : `FIN`

- ํ๋ก์ธ์ค A๊ฐ ์ฐ๊ฒฐ์ ์ข๋ฃํ๊ฒ ๋ค๋ FIN ํ๋๊ทธ๋ฅผ ์ ์ก
- ํ๋ก์ธ์ค B๊ฐ FIN ํ๋๊ทธ๋ก ์๋ตํ๊ธฐ ์ ๊น์ง ์ฐ๊ฒฐ์ ๊ณ์ ์ ์ง

2)B->A : `ACK`

- ํ๋ก์ธ์ค B๋ ์ผ๋จ ํ์ธ ๋ฉ์ธ์ง๋ฅผ ๋ณด๋ด๊ณ , ์์ ์ ํต์ ์ด ๋๋  ๋๊น์ง ๊ธฐ๋ค๋ฆฐ๋ค. (์ด ์ํ๊ฐ TIME_WAIT ์ํ)
- ์์ ์๋ Ackowledgement Number ํ๋๋ฅผ `Sequence Number + 1`๋ก ์ง์ ํ๊ณ , ACK ํ๋๊ทธ ๋นํธ๋ฅผ 1๋ก ์ค์ ํ ์ธ๊ทธ๋จผํธ๋ฅผ ์ ์กํ๋ค.
- ๊ทธ๋ฆฌ๊ณ  ์์ ์ด ์ ์กํ  ๋ฐ์ดํฐ๊ฐ ๋จ์์๋ค๋ฉด ์ด์ด์ ๊ณ์ ์ ์กํ๋ค.

3)B->A : `FIN`

- ํ๋ก์ธ์ค B๊ฐ ํต์ ์ด ๋๋ฌ์ผ๋ฉด ์ฐ๊ฒฐ ์ข๋ฃ ์์ฒญ์ ํฉ์ํ๋ค๋ ์๋ฏธ๋ก ํ๋ก์ธ์ค A์๊ฒ FIN ํ๋๊ทธ๋ฅผ ์ ์ก

4)A->B : `ACK`

- ํ๋ก์ธ์ค A๋ ํ์ธํ๋ค๋ ๋ฉ์ธ์ง๋ฅผ ์ ์ก

<br>

## ์ฐธ๊ณ 

#### ํฌํธ(PORT) ์ํ ์ ๋ณด

- `CLOSED` : ํฌํธ๊ฐ ๋ซํ ์ํ
- `LISTEN` : ํฌํธ๊ฐ ์ด๋ฆฐ ์ํ๋ก ์ฐ๊ฒฐ ์์ฒญ ๋๊ธฐ ์ค
- `SYN_RCV` : SYNC ์์ฒญ์ ๋ฐ๊ณ  ์๋๋ฐฉ์ ์๋ต์ ๊ธฐ๋ค๋ฆฌ๋ ์ค
- `ESTABLISHED` : ํฌํธ ์ฐ๊ฒฐ ์ํ

<br>

#### TCP ํค๋ ์์ ํ๋๊ทธ ์ ๋ณด

TCP Header์๋ CONTROL BIT(ํ๋๊ทธ ๋นํธ, 6 bit)๊ฐ ์กด์ฌํ๋ฉฐ, ๊ฐ๊ฐ์ bit๋ `URG-ACK-PSH-RST-SYN-FIN`์ ์๋ฏธ๋ฅผ ๊ฐ์ง๋ค.

ํด๋น ์์น์ bit๊ฐ 1์ด๋ฉด ํด๋น ํจํท์ด ์ด๋ ํ ๋ด์ฉ์ ๋ด๊ณ  ์๋ ํจํท์ธ์ง๋ฅผ ๋ํ๋ธ๋ค.

![img](https://mblogthumb-phinf.pstatic.net/MjAxOTA1MTVfMTkz/MDAxNTU3ODk4MjkyNzMw.ne0R09ZKfbH0hn5w3wvEcf-m469wWfBu8kSAnHdFI2sg.qT5Gl2BL0cBW_Soo2si9xWMaetDowy-Q5yT8OvWLNzMg.JPEG.ak0402/1.jpg?type=w800)

- `SYN` (Synchronize Sequence Number)
  - ์ฐ๊ฒฐ ์ค์  / `000010`
  - Sequence Number๋ฅผ ๋๋ค์ผ๋ก ์ค์ ํ์ฌ ์ธ์์ ์ฐ๊ฒฐํ๋ ๋ฐ ์ฌ์ฉํ๋ฉฐ, ์ด๊ธฐ์ Sequence Number๋ฅผ ์ ์กํ๋ค.
- `ACK` (Acknowledgement)
  - ์๋ต ํ์ธ / `010000`
  - ํจํท์ ๋ฐ์๋ค๋ ๊ฒ์ ์๋ฏธํ๋ค.
  - Acknowledgement Number ํ๋๊ฐ ์ ํจํ ์ง๋ฅผ ๋ํ๋ธ๋ค.
  - ์๋จ ํ๋ก์ธ์ค๊ฐ ์ฌ์ง ์๊ณ , ๋ฐ์ดํฐ๋ฅผ ์ ์กํ๋ค๊ณ  ๊ฐ์ ํ๋ฉด  ์ต์ด ์ฐ๊ฒฐ ์ค์  ๊ณผ์ ์์ ์ ์ก๋๋ ์ฒซ ๋ฒ์งธ ์ธ๊ทธ๋จผํธ๋ฅผ ์ ์ธํ ๋ชจ๋  ์ธ๊ทธ๋จผํธ์ ACK ๋นํธ๋ 1๋ก ์ง์ ๋๋ค๊ณ  ์๊ฐํ  ์ ์๋ค.
- `FIN` (Finish)
  - ์ฐ๊ฒฐ ํด์  / `000001`
  - ์ธ์ ์ฐ๊ฒฐ์ ์ข๋ฃ์ํฌ ๋ ์ฌ์ฉ๋๋ฉฐ, ๋ ์ด์ ์ ์กํ  ๋ฐ์ดํฐ๊ฐ ์์์ ์๋ฏธํ๋ค.

<br>

## ๊ธฐ์  ์ง๋ฌธ/์์ ์ง๋ฌธ

<details>
<summary>TCP์ UDP์ ์ฐจ์ด์ ๋ํด์ ์ค๋ชํด ์ฃผ์ธ์.</summary>




<br>

**TCP**

- ์ ๋ขฐ์ฑ ์๋ ๋ฐ์ดํฐ ์ ์ก์ ์ง์ํ๋ ์ฐ๊ฒฐ ์งํฅํ ํ๋กํ ์ฝ

- ํ๋ฆ์ ์ด, ํผ์ก์ ์ด, ์ค๋ฅ์ ์ด ์ง์

- ์ฐ๊ฒฐ ์ค์ ์ 3 way handshake๋ฅผ, ์ฐ๊ฒฐ ํด์ ์ 4 way handshake ์งํ

- UDP๋ณด๋ค ์๋๊ฐ ๋๋ฆฌ๋ค

- **EX)** ์น http ํต์ , ์ด๋ฉ์ผ, ํ์ผ ์ ์ก

**UDP**

- ๋ฐ์ดํฐ๋ฅผ ๋ฐ์ดํฐ๊ทธ๋จ ๋จ์๋ก ์ฒ๋ฆฌํ๋ ํ๋กํ ์ฝ

- ์ ๋ขฐ์ฑ ๋ฎ์

- ์๋๊ฐ ๋น ๋ฅด๊ณ  ๋ถํ๊ฐ ์ ๋ค

- **EX)** Real Time Protocol(RTP), Multicast, DNS

</details>

<br>

<details>
<summary>3-way hand shake, 4-way hand shake ํ๋ฆ์ ๋ํด์ ์ค๋ชํด์ฃผ์ธ์.</summary>




<br>

**3 way handshake**

- TCP/IP ํ๋กํ ์ฝ์ ์ฌ์ฉํด ํต์ ์ ์งํํ  ๋, ๋ ์ข๋จ๊ฐ ์ ํํ ๋ฐ์ดํฐ ์ ์ก ๋ณด์ฅํ๊ธฐ ์ํด ์ฐ๊ฒฐ์ ์ค์ 

- SYN(Synchronize Sequence Number)

- ACK(Acknowledgement)

1. **ํด๋ผ์ด์ธํธ** โ **์๋ฒ** : ์๋ฒ ์ ์ ์์ฒญ **SYN ํจํท** ๋ณด๋

2. **์๋ฒ** โ **ํด๋ผ์ด์ธํธ** : ์์ฒญ ์๋ฝ ์๋ต **ACK ํจํท**๊ณผ ํฌํธ ์ด์ด๋ฌ๋ผ๋ **SYN ํจํท** ๋ณด๋

3. **ํด๋ผ์ด์ธํธ** โ **์๋ฒ** : ํ์ธ ์๋ต์ผ๋ก **ACK ํจํท** ๋ณด๋

**4 way handshake**

- ์ฐ๊ฒฐ ์ค์  ํด์ ํจ

1. **ํด๋ผ์ด์ธํธ**โ **์๋ฒ** : ์ฐ๊ฒฐ ํด์ ํ๊ฒ ๋ค๋ **FIN ํจํท** ๋ณด๋

2. **์๋ฒ** โ **ํด๋ผ์ด์ธํธ** : ์๋ต์ผ๋ก **ACK ํจํท** ๋ณด๋

3. **์๋ฒ** โ **ํด๋ผ์ด์ธํธ**: ์ฒ๋ฆฌํด์ผํ  ๋ชจ๋  ํต์  ๋๋ด๊ณ  ์ฐ๊ฒฐ ์ข๋ฃํ๊ฒ ๋ค๋ **FIN ํจํท** ๋ณด๋

4. **ํด๋ผ์ด์ธํธ**โ **์๋ฒ** : ํ์ธ ์๋ต์ผ๋ก **ACK ํจํท** ๋ณด๋

</details>

<br>

<br>

<br>

Reference

- https://gmlwjd9405.github.io/2018/09/19/tcp-connection.html

- https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=ak0402&logNo=221539958656

- https://daengsik.tistory.com/m/30
