# TCP 와 HTTP

OSI 7계층 중 TCP는 4계층인 Transport Layer에서 동작, TCP와 묶여 하나의 인터넷 프로토콜처럼 불리는 IP는 3계층인 Network Layer에서 동작한다.

Http는 최상위 계층인 application 계층에서 동작한다. 7계층에서 http 프로토콜에 패킷이 점점 붙어 4계층인 tcp 패킷까지 점차 붙게 된다.

### TCP(Transmission Control Protocol) 특징

- 연결지향적 (계속 연결되어 있음)
- 연결할 때는 3-way-handshaking / 연결종료 시에는 4-way-handshaking 방식을 사용한다.
- 신뢰성이 보장된다
- byte array (binary)로 통신

### HTTP(HyperText Transfer Protocol) 특징

- HTTP 또한 tcp로 이루어져 있다. (tcp 보다 high 한 계층이므로)
- 비연결지향적이다. (3-way-handshaking 이후 클라이언트 측에서 연결을 종료한다) → 그렇기 때문에 request > response 식으로 작동한다. (keep-alive 도 가능하다)
- String으로 통신 → 사용자계층이니까 당연히!

→ 연결지향 / 동기식 통신이 필요하다면 4계층에서부터 패킷변환이 이루어지는 소켓 통신을 사용해야 한다.