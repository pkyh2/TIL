# HTTP(Hyper-Text-Transfer-Protocol)

## HTTP

- HTML로 이루어진 텍스트를 네트워크를 통해서 송/수신 하는 방법
  - HTML로 이루어진 소스파일은 서버에 존재를 하고
  - 브라우저는 서버로부터 HTML 소스파일을 전송받아서 화면에 표현

## 소켓 프로그래밍

### 소켓이란?

- 네트워크상에서 동작하는 프로그램 간 통신의 종착점(Endpoint)이다. 즉, 프로그램이 네트워크에서 데이터를 통신할 수 있도록 연결해주는 연결부라고 할 수 있다.
- 연결된 네트워크의 양 끝단을 추상화 시킨 개념, 컴퓨터의 관점에서는 네트워크로 통하는 컴퓨터의 외부와 컴퓨터 내부의 프로그램을 이어주는 인터페이스이다.

```python
import socket

# 네트워크 통신을 하기 위한 소켓 객체를 생성
# IPv4를 이용한 TCP 통신용 소켓(파일 입/출력을 하기 위해서 객체를 생성하는 것과 비슷)
# 이렇게 생성된 소켓 객체를 통해서 서버와 통신(입/출력)을 할 수 있다.
sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
'''
socket.socket() 함수는 패밀리, 타입 두 가지 인자를 받는다.
패밀리는 소켓의 주소체계 IPv4(default) or IPv6를 많이 쓴다.
타입은 소켓 타입 raw소켓, 스트림소켓, 데이터그램 소켓등이 있는데 socket.SOCK_STREAM(default) or socket.SOCK_DGRAW를 많이 쓴다.
'''

print('not error')

'''
파일 입/출력 할 때 open을 통해서 입/출력 하기 위한 파일 객체를 얻어온 것 처럼
통신(입/출력)하기 위한 서버 객체를 생성
소켓 프로그래밍에서는 connect()를 통해서 통신 하기위한 서버의 객체를 얻어올 수 있다.
생성된 객체를 통해서 입/출력(통신)
'''

# 읽거나 쓰기 위한 파일의 경로와 유사
# 통신하기 위한 네트워크 상의 경로 정도로 이해
serverAddress = socket.gethostbyname('info.cern.ch')
serverPort = 80

# 오픈하기 위한 서버의 경로는 튜플로 전달
sock.connect((serverAddress, serverPort))
print('DEBUG:::connect 완료')

# 서버에 HTML 코드를 요청 => Request Header(요청헤더)
# 대/소문자 띄어쓰기 잘 확인해야 한다.
request_header = 'GET /index.html HTTP/1.1\r\n'     # 여기서 \r\n은 구분자 라인과 라인, 필드와 필드를 구분해주는 구분자의 역할
request_header += 'Host: info.cern.ch\r\n'
request_header += '\r\n'

# 요청대로 처리가 되어서 HTML코드가 잘 다운로드 되는지 확인
sock.send(request_header.encode())

# sock.recv(bufsize)를 통해 소켓으로부터 bufsize만큼의 데이터를 읽어온다.
response = sock.recv(1024)

# 문자열을 주고받고 싶다면 .incode() or .decode()을 해야한다.
print(response.decode())			

sock.close()
```

#### Request Header

`GET /index.html HTTP/1.1\r\n` : request(start) - line

- `GET` : methon
- `/index.html` : URL(URI)
  - URL(Uniform Resource Locator) - 네트워크상에서 접근하려는 리소스(파일)의 경로 정도로 이해
  - URI(Uniform Resouece Identifier) 통합자원식별
    - 경로 표현을 식별자로 하여 요청을 보내면 서버에서 그에 맞는 정보를 반환
- `HTTP/1.1` : protocol ver
- `\r\n` : separator



