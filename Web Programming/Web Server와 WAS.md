# Web Server와 WAS

## 웹 서버(Web Server)

### HTTP 프로토콜을 기반으로 클라이언트의 요청을 서비스

- 웹 서버란 클라이언트가 웹 브라우저에 어떠한 페이지 요청을 하면 웹 서버에서 그 요청을 받아 정적 컨텐츠를 제공하는 서버
  - 여기서 정적 컨텐츠는 `html, css, js 이미지` 등과 같은 즉시 응답 가능한 컨텐츠들이다.
  - 아파치, nginx 등이 있다.

- 그렇다면 웹 서버는 정적 컨텐츠만 제공한다고 생각할 수 있는데 아니다.
  - 웹 서버가 동적 컨텐츠를 받으면 WAS(Web Application Server)에게 요청을 넘겨주고 WAS에서 처리한 결과를 클라이언트에게 전달한다.

### CGI(Common Gateway Interface)

- 웹 서버에서 어플리케이션을 작동시키기 위한 인터페이스 입니다.
- 정적인 웹서버를 동적으로 기능하게 하기 위해서 등장하였습니다.
- 서버 프로그램과 외부 프로그램 간의 인터페이스가 바로 CGI입니다.



### WAS(Web Application Server)

- WAS는 웹서버가 동적으로 기능하면 WAS입니다.

- 간단하게 말해 웹서버 위에 서버 어플리케이션을 얹은 것이 WAS 입니다.
- 방식) 요청 -> 웹서버 -> 웹 어플리키이션 서버(톰캣, JBoss)

### WSGI(Web Server Gateway Interface)

- WSGI는 웹 서버 소프트웨어와 파이썬으로 작성된 웹 응용프로그램(Python application) 간의 표준 인터페이스입니다.
- WSGI 자체는 서버나 프레임워크 자체가 아니라 서버가 어플리케이션과 통신하는 명세를 다루고 있는 것이기 때문에 우리가 이 기능들을 매번 개발해야 한다.(쿠키, 세션, 인증, 라우팅 등)
  - 그러나 이러한 기능 등을 구현해 확장할 수 있게 한 것이 WSGI Middleware다.
  - WSGI Middleware는 Web Application의 실행 전과 후에 이러한 기능들을 추가해주고, 그 자체로도 WSGI Application이다.
  - 대표적인 WSGI 서버로는 Gunicorn, uWSGI, Werkzeug, mod_wsgie등이 있다.
- 하지만 WSGI는 단일 호출 가능 인터페이스여서 Websocket과 같은 웹 프로토콜에 적합하지 않습니다.
  - HTTP는 Connection이 짧게 유지되는 특성을 지니고 있었기 때문에, Long-Polling HTTP와 WebSocket 같이 상대적으로 **connection이 긴 특성을 지닌 Protocol 과는 맞지 않았다.**
  - HTTP Request는 Application 내부에서 오직 하나의 path를 가질 수 있기 때문에, **여러개의 path를 통해 이벤트를 수신하는 Websocket**의 이벤트를 처리할 수 없었다.

### ASGI(Asynchronous Server Gateway Interface)

- 비동기 가능 python 웹 서버, 프레임워크 및 어플리케이션 간의 표준 인터페이스를 제공하기 위한 WSGI의 영적 후속 제품

- WSGI가 동기 Python 앱에 대한 표준을 제공 했다면, ASGI는 WSGI 이전 버전과의 호환성 구현과 여러 서버 및 애플리케이션 프레임워크를 통해 비동기 및 동기 앱 모두에 대한 표준을 제공합니다.

- ASGI Application은 단일, 비동기 callable 속성을 지니고 있다. 수신 요청에 대한 정보를 포함하는 scope를 accept하고, 클라이언트에 이벤트를 보내고 받을 수 있는 awaitable를 보내고 받을 수 있다.

  - 이 덕분에 ASGI는 WSGI의 한계점을 뛰어넘는 수신/발신 event를 허영할 수 있다.
  - 그 뿐만 아니라 ASGI는 background coroutine을 허용하기 때문에 application은 요청을 처리하면서 background에서 다른 작업도 수행할 수 있다.(ex. 외부 event를 listening 하고 있는 redis queue등)
  - ASGI를 통해서 보내거나 받는 **모든 event는 Python Dictionary Type이다.**


### Daphne

- Daphne는 Django Channels 프로젝트의 공식 ASGI HTTP/WebSocket 서버입니다.
- HTTP 및 WebSocket과 같은 모든 요청에 대해 Daphne를 사용할 수 있다.(우리 프로젝트에 그렇게 적용)
  - 안정성에 대해 보수적이라면 WSGI를 통해 HTTP요청을 처리하고, WSGI가 할 수 없는 HTTP long-polling과 WebSocket만 Daphne로 처리하도록 할 수 있다.
