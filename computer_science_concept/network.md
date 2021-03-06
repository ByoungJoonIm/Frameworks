### OSI 7 Layer
- OSI(Open Systems Interconnection Reference Model)은 국제표준화기구(ISO, International Organization for Standardization)에서 개발한 통신 계층 모델
- 통신이 일어나는 과정을 단계별로 파악 가능
- 각 Layer별로 표준이 정해져 있기 때문에, 여러 회사가 만든 각기 다른 장비를 사용해도 호환이 가능 

계층 번호 | 계층 | 하는 일 | 대표 장비
----- | ---- | ---- | ----
7 | 응용 | HTTP/POP3/IMAP/FTP 등의 사용자가 사용하는 프로토콜 | .
6 | 표현 | 인코딩 / 암호화를 통해 이해할 수 있는 포맷으로 변환 | .
5 | 세션 | TCP / IP 연결 관리 | .
4 | 전송 | UDP/TCP로 전송 | 게이트웨이
3 | 네트워크 | 라우팅 | 라우터
2 | 데이터 링크 | 데이터 오류 검출/수정 | 브릿지, 스위치
1 | 물리 | 실제 데이터 전송 | 리피터, 허브


### IP Address
- 범위에 따른 클래스 구분

class | 시작 비트 | 십진수 범위 | 네트워크 범위 | 호스트 범위 | 한 네트워크 당 호스트 개수
---- | ---- | ---- | ---- | ---- | ----
A | 0 | 1 ~ 126 | 1~8 bit | 9~32 bit(24 bits) | 2^24 - 2
B | 1 | 128 ~ 191 | 1~16 bit | 17~32 bit(16 bits) | 2^16 - 2
C | 11 | 192 ~ 223 | 1~24 bit | 25~32 bit(8 bits) | 2^8 - 2
D | 111 | 224 ~ 239 | - | - | -
E | 1111 | 240 ~ 255 | - | - | -

  - 네트워크 127은 루프백 주소(localhost)로 사용된다
  - 호스트 0번은 네트워크 주소 자체를 나타낸다
  - 호스트 255번은 브로드캐스트 주소이다

### 연결 수립과 해제
- 3-way-handshake(연결)
  1. client는 서버에게 SYN(a)를 전송한다.
  2. server는 ACK(a+1)와 SYN(b)를 client에게 전송한다.
  3. client는 ACK(b+1)를 전송한다.
  
- 4-way-handshake(연결 해제)
  - server와 client의 초기 상태는 ESTABLISHED이다.
  1. client는 상태를 FIN_WAIT1로 바꾸고 server에게 FIN을 전송한다.
  2. FIN을 받은 서버는 ACK를 전송하고, 상태를 CLOSE_WAIT으로 변경한다.
  3. ACK를 받은 client는 상태를 FIN_WAIT2로 변경한다.
  4. 데이터 전송을 마친 server는 상태를 LASK_ACK로 바꾸고, client에게 FIN을 전송한다.
  5. FIN을 받은 client는 ACK를 전송하고, 상태를 TIME_WAIT으로 변경한다.
  6. ACK를 받은 server는 연결을 해제한다.
  7. client는 TIME_WAIT 상태에서 timeout되면 연결을 해제한다.
  
