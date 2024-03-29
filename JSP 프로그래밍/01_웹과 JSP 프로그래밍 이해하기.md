![](https://i.imgur.com/AcMiLCW.png)

본 게시글은 "JSP 웹 프로그래밍"을 학습하며, 내용 요약 또는 몰랐던 부분을 정리하는 글 입니다.

## 웹과 JSP 프로그래밍 이해하기

### 인터넷과 웹의 개요

인터넷

- 컴퓨터가 서로 연결되어 TCP/IP라는 통신 프로토콜을 이용하여 정보를 주고 받는 전 세계의 컴퓨터 네트워크

웹

- 인터넷에 연결된 컴퓨터들을 통해 사람들이 정보를 공유할 수 있는 정보 공간
- 월드 와이드 웹(world wide web)의 줄임말

웹의 동작 원리

- 웹은 기본적으로 클라이언트/서버 방식으로 동작
- 기본적으로 http 통신 (요청/응답)

가장 널리 쓰이는 웹 서버

- 아파치
- 톰캣 (아파치 재단에서 배포)
  - 아파치 소프트웨어재단(Apache Software Foundation)에서 개발한 웹 어플리케이션 서버
  - 자바로 만들어진 웹 페이지를 구동하기 위한 엔진
- IIS (윈도우에서만 동작)

### 정적 웹 페이지와 동적 웹 페이지

정적 웹 페이지

- 컴퓨터에 저장된 텍스트 파일을 그대로 보는 것
- 서버: 이미 준비된 HTML 문서를 그대로 전달
  ![](https://i.imgur.com/Efryckf.png)

동적 웹 페이지

- 저장된 내용을 다른 변수로 가공 처리하여 보는 것
- PHP(Personal Home Page), ASP(Active Server Page), JSP(Java Server Page)
- 서버: 사용자 요청에 맞게 정제된 HTML 문서를 전달
  ![](https://i.imgur.com/iMmEeYf.png)

### 웹 프로그래밍과 JSP(JavaServer Pages)

웹 프로그래밍 언어

- 클라이언트 측 실행 언어와 서버 측 실행 언어로 구분
- 자바를 기반으로 하는 **JSP는 서버 측 웹 프로그래밍 언어**
  - JSP: HTML내에 자바 코드를 삽입하여 웹 서버에서 동적으로 웹 페이지를 생성하여 웹 브라우저에 돌려주는 **서버 사이드 스크립트 언어**

JSP 특징

- JSP는 서블릿 기술의 확장(서블릿의 모든 기능을 사용할 수 있다.)
- JSP는 유지 관리가 용이
- JSP는 빠른 개발이 가능
- JSP로 개발하면 코드 길이를 줄일 수 있음

### JSP 페이지의 처리 과정

![](https://i.imgur.com/NhGLduG.png)

JSP 컨테이너에서 아래와 같은 동작들이 수행된다.

1. JSP 요청 (Hello.jsp)
2. 번역: 서블릿 프로그램 (Hello_jsp.java)
3. 컴파일: 서블릿 클래스 (Hello_jsp.class 바이트 코드)

### JSP 생명주기

각 단계별 함수 중요.

![](https://i.imgur.com/aQgWill.png)
