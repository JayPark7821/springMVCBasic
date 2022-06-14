# springMVCBasic
스프링 MVC 1편

* ### HttpServletRequest - 개요
  - HttpServletRequest 역할
  - HTTP 요청 메시지를 개발자가 직접 파싱해서 사용해도 되지만, 매우 불편할 것이다. 서블릿은 개발자가
    HTTP 요청 메시지를 편리하게 사용할 수 있도록 개발자 대신에 HTTP 요청 메시지를 파싱한다. 그리고 그
    결과를 HttpServletRequest 객체에 담아서 제공한다.

![image](https://user-images.githubusercontent.com/60100532/173553172-33efbaac-da75-4ec6-96ed-70a3d07ceb12.png)

  - ### 임시 저장소 기능
  - 해당 HTTP 요청이 시작부터 끝날 때 까지 유지되는 임시 저장소 기능
  - 저장: request.setAttribute(name, value)
  - 조회: request.getAttribute(name)

  - ### 세션 관리 기능
  - request.getSession(create: true)

* ### HTTP 요청 데이터 - 개요
  - HTTP 요청 메시지를 통해 클라이언트에서 서버로 데이터를 전달하는 방법을 알아보자.
  - GET - 쿼리 파라미터
    - /url?username=hello&age=20
    - 메시지 바디 없이, URL의 쿼리 파라미터에 데이터를 포함해서 전달 예) 검색, 필터, 페이징등에서 많이 사용하는 방식
  - POST - HTML Form
    - content-type: application/x-www-form-urlencoded
    - 메시지 바디에 쿼리 파리미터 형식으로 전달 username=hello&age=20 예) 회원 가입, 상품 주문, HTML Form 사용
  - HTTP message body에 데이터를 직접 담아서 요청
    - HTTP API에서 주로 사용, JSON, XML, TEXT
    - 데이터 형식은 주로 JSON 사용
    - POST, PUT, PATCH

![image](https://user-images.githubusercontent.com/60100532/173557970-81709bc2-a6b4-4f01-8131-edc592b70521.png)


* ### HTTP 요청 데이터 - GET 쿼리 파라미터

  * 전달 데이터
    - username=hello
    - age=20
  * 메시지 바디 없이, URL의 쿼리 파라미터를 사용해서 데이터를 전달하자.
    예) 검색, 필터, 페이징등에서 많이 사용하는 방식
    
  * 쿼리 파라미터는 URL에 다음과 같이 ? 를 시작으로 보낼 수 있다. 추가 파라미터는 & 로 구분하면 된다.
    - http://localhost:8080/request-param?username=hello&age=2

* ### HTTP 요청 데이터 - POST HTML Form
  * 특징
    content-type: application/x-www-form-urlencoded
    메시지 바디에 쿼리 파리미터 형식으로 데이터를 전달한다. username=hello&age=20

> application/x-www-form-urlencoded 형식은 앞서 GET에서 살펴본 쿼리 파라미터 형식과 같다.
따라서 쿼리 파라미터 조회 메서드를 그대로 사용하면 된다.

> - 클라이언트(웹 브라우저) 입장에서는 두 방식에 차이가 있지만, 서버 입장에서는 둘의 형식이 동일하므로,
request.getParameter() 로 편리하게 구분없이 조회할 수 있다.
> - 정리하면 request.getParameter() 는 GET URL 쿼리 파라미터 형식도 지원하고, POST HTML Form
형식도 둘 다 지원한다.
>

* ### HTTP 요청 데이터 - API 메시지 바디 - 단순 텍스트
  - HTTP message body에 데이터를 직접 담아서 요청
    - HTTP API에서 주로 사용, JSON, XML, TEXT
    - 데이터 형식은 주로 JSON 사용
    - POST, PUT, PATCH

> inputStream은 byte 코드를 반환한다. byte 코드를 우리가 읽을 수 있는 문자(String)로 보려면 문자표
(Charset)를 지정해주어야 한다. 여기서는 UTF_8 Charset을 지정해주었다.
>

* ### HTTP 요청 데이터 - API 메시지 바디 - JSON 

> * 참고
> 
> JSON 결과를 파싱해서 사용할 수 있는 자바 객체로 변환하려면 Jackson, Gson 같은 JSON 변환
라이브러리를 추가해서 사용해야 한다. 스프링 부트로 Spring MVC를 선택하면 기본으로 Jackson
라이브러리( ObjectMapper )를 함께 제공한다.


> * 참고
> 
> HTML form 데이터도 메시지 바디를 통해 전송되므로 직접 읽을 수 있다. 하지만 편리한 파리미터 조회
기능( request.getParameter(...) )을 이미 제공하기 때문에 파라미터 조회 기능을 사용하면 된다