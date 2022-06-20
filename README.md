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
> 

* ### HttpServletResponse - 기본 사용법
  * HttpServletResponse 역할

  - HTTP 응답 메시지 생성
    - HTTP 응답코드 지정
    - 헤더 생성
    - 바디 생성
  - 편의 기능 제공
    - Content-Type, 쿠키, Redirec

* ### HTTP 응답 데이터 - 단순 텍스트, HTML
  * 단순 텍스트 응답
  * HTML 응답
  * HTTP API - MessageBody JSON 응답
 
* ### HTTP 응답 데이터 - API JSON
  > application/json 은 스펙상 utf-8 형식을 사용하도록 정의되어 있다. 그래서 스펙에서 charset=utf-8
  > 과 같은 추가 파라미터를 지원하지 않는다. 따라서 application/json 이라고만 사용해야지 application/json;charset=utf-8 이라고 전달하는 것은 의미 없는 파라미터를 추가한 것이 된다.
  > 
  > response.getWriter()를 사용하면 추가 파라미터를 자동으로 추가해버린다. 이때는
response.getOutputStream()으로 출력하면 그런 문제가 없다.
  >

* ## 서블릿, JSP, MVC 패턴
  * ## 서블릿과 JSP의 한계

서블릿으로 개발할 때는 뷰(View)화면을 위한 HTML을 만드는 작업이 자바 코드에 섞여서 지저분하고
복잡했다.

JSP를 사용한 덕분에 뷰를 생성하는 HTML 작업을 깔끔하게 가져가고, 중간중간 동적으로 변경이 필요한
부분에만 자바 코드를 적용했다.

그런데 이렇게 해도 해결되지 않는 몇가지 고민이 남는다.

회원 저장 JSP를 보자. 코드의 상위 절반은 회원을 저장하기 위한 비즈니스 로직이고, 나머지 하위 절반만
결과를 HTML로 보여주기 위한 뷰 영역이다. 회원 목록의 경우에도 마찬가지다.

코드를 잘 보면, JAVA 코드, 데이터를 조회하는 리포지토리 등등 다양한 코드가 모두 JSP에 노출되어 있다.
JSP가 너무 많은 역할을 한다. 

이렇게 작은 프로젝트도 벌써 머리가 아파오는데, 수백 수천줄이 넘어가는
JSP를 떠올려보면 정말 지옥과 같을 것이다. (유지보수 지옥 썰)

* ### MVC 패턴의 등장
> 비즈니스 로직은 서블릿 처럼 다른곳에서 처리하고, JSP는 목적에 맞게 HTML로 화면(View)을 그리는
일에 집중하도록 하자. 과거 개발자들도 모두 비슷한 고민이 있었고, 그래서 MVC 패턴이 등장했다. 우리도
직접 MVC 패턴을 적용해서 프로젝트를 리팩터링 해보자.
> 

* ### MVC 패턴 - 개요
  * 너무 많은 역할
    * 하나의 서블릿이나 JSP만으로 비즈니스 로직과 뷰 렌더링까지 모두 처리하게 되면, 너무 많은 역할을
      하게되고, 결과적으로 유지보수가 어려워진다. 비즈니스 로직을 호출하는 부분에 변경이 발생해도 해당
      코드를 손대야 하고, UI를 변경할 일이 있어도 비즈니스 로직이 함께 있는 해당 파일을 수정해야 한다.
      HTML 코드 하나 수정해야 하는데, 수백줄의 자바 코드가 함께 있다고 상상해보라! 또는 비즈니스 로직을
      하나 수정해야 하는데 수백 수천줄의 HTML 코드가 함께 있다고 상상해보라.

  * 변경의 라이프 사이클 
    * 사실 이게 정말 중요한데, 진짜 문제는 둘 사이에 변경의 라이프 사이클이 다르다는 점이다.
    > 변경 주기가 다르면 서로 분리한다.
    * 예를 들어서 UI
      를 일부 수정하는 일과 비즈니스 로직을 수정하는 일은 각각 다르게 발생할 가능성이 매우 높고 대부분
      서로에게 영향을 주지 않는다. 이렇게 변경의 라이프 사이클이 다른 부분을 하나의 코드로 관리하는 것은
      유지보수하기 좋지 않다. (물론 UI가 많이 변하면 함께 변경될 가능성도 있다.)
  
  * 기능 특화
    * 특히 JSP 같은 뷰 템플릿은 화면을 렌더링 하는데 최적화 되어 있기 때문에 이 부분의 업무만 담당하는 것이
     가장 효과적이다.


  * ### Model View Controller
   * MVC 패턴은 지금까지 학습한 것 처럼 하나의 서블릿이나, JSP로 처리하던 것을 컨트롤러(Controller)와
  뷰(View)라는 영역으로 서로 역할을 나눈 것을 말한다. 웹 애플리케이션은 보통 이 MVC 패턴을 사용한다.


   * 컨트롤러: HTTP 요청을 받아서 파라미터를 검증하고, 비즈니스 로직을 실행한다. 그리고 뷰에 전달할 결과
  데이터를 조회해서 모델에 담는다.


   * 모델: 뷰에 출력할 데이터를 담아둔다. 뷰가 필요한 데이터를 모두 모델에 담아서 전달해주는 덕분에 뷰는
    비즈니스 로직이나 데이터 접근을 몰라도 되고, 화면을 렌더링 하는 일에 집중할 수 있다.


   * 뷰: 모델에 담겨있는 데이터를 사용해서 화면을 그리는 일에 집중한다. 여기서는 HTML을 생성하는 부분을
    말한다.

> * 참고
> 
> 컨트롤러에 비즈니스 로직을 둘 수도 있지만, 이렇게 되면 컨트롤러가 너무 많은 역할을 담당한다. 그래서
일반적으로 비즈니스 로직은 서비스(Service)라는 계층을 별도로 만들어서 처리한다. 그리고 컨트롤러는
비즈니스 로직이 있는 서비스를 호출하는 역할을 담당한다. 참고로 비즈니스 로직을 변경하면 비즈니스
로직을 호출하는 컨트롤러의 코드도 변경될 수 있다. 앞에서는 이해를 돕기 위해 비즈니스 로직을
호출한다는 표현 보다는, 비즈니스 로직이라 설명했다.
>


![image](https://user-images.githubusercontent.com/60100532/174059290-7228317e-e003-4a78-ac88-df0e9f065cee.png)

>dispatcher.forward() :
> 
> * 다른 서블릿이나 JSP로 이동할 수 있는 기능이다. 서버 내부에서 다시 호출이
발생한다
> 
> /WEB-INF : 
> 
> * 이 경로안에 JSP가 있으면 외부에서 직접 JSP를 호출할 수 없다. 우리가 기대하는 것은 항상 컨트롤러를
통해서 JSP를 호출하는 것이다
> 
> redirect vs forward : 
> 
> * 리다이렉트는 실제 클라이언트(웹 브라우저)에 응답이 나갔다가, 클라이언트가 redirect 경로로 다시
요청한다. 따라서 클라이언트가 인지할 수 있고, URL 경로도 실제로 변경된다. 반면에 포워드는 서버
내부에서 일어나는 호출이기 때문에 클라이언트가 전혀 인지하지 못한다
>


* ### MVC 패턴 - 한계
  * MVC 패턴을 적용한 덕분에 컨트롤러의 역할과 뷰를 렌더링 하는 역할을 명확하게 구분할 수 있다.
  * 특히 뷰는 화면을 그리는 역할에 충실한 덕분에, 코드가 깔끔하고 직관적이다. 
  * 단순하게 모델에서 필요한 데이터를 꺼내고, 화면을 만들면 된다.
  * 그런데 컨트롤러는 딱 봐도 중복이 많고, 필요하지 않는 코드들도 많이 보인다.


* ### 프론트 컨트롤러 패턴 소개
![image](https://user-images.githubusercontent.com/60100532/174319705-a19a31be-53a4-4dc8-8a84-e54743069d3b.png)

![image](https://user-images.githubusercontent.com/60100532/174319740-eafeb300-14aa-4321-8a56-ba7062f4a5b2.png)


* FrontController 패턴 특징
  * 프론트 컨트롤러 서블릿 하나로 클라이언트의 요청을 받음
  * 프론트 컨트롤러가 요청에 맞는 컨트롤러를 찾아서 호출
  * 입구를 하나로!
  * 공통 처리 가능
  * 프론트 컨트롤러를 제외한 나머지 컨트롤러는 서블릿을 사용하지 않아도 됨


* 스프링 웹 MVC와 프론트 컨트롤러
  * 스프링 웹 MVC의 핵심도 바로 FrontController
  * 스프링 웹 MVC의 DispatcherServlet이 FrontController 패턴으로 구현되어 있음

* ### 프론트 컨트롤러 도입 - v1
![image](https://user-images.githubusercontent.com/60100532/174320373-b0377c20-dcb8-4278-af5c-fc7a299178cf.png)

* ### view 분리 - v2
![image](https://user-images.githubusercontent.com/60100532/174325182-06294522-9071-4ef2-a797-45f1c0698912.png)

* ### Model 추가 - v3
![image](https://user-images.githubusercontent.com/60100532/174332282-73825a86-8751-45bc-8c36-472555d786f4.png)

* ### 단숭하고 실용적인 컨트롤러 -v4
![image](https://user-images.githubusercontent.com/60100532/174435576-361a1a23-ea02-44c0-8bf1-e141c6cfd775.png)




> * 어댑터 패턴
>   * 지금까지 우리가 개발한 프론트 컨트롤러는 한가지 방식의 컨트롤러 인터페이스만 사용할 수 있다.
>   * ControllerV3 , ControllerV4 는 완전히 다른 인터페이스이다. 따라서 호환이 불가능하다. 
>   * 마치 v3는  110v이고, v4는 220v 전기 콘센트 같은 것이다. 이럴 때 사용하는 것이 바로 어댑터이다.
>   * 어댑터 패턴을 사용해서 프론트 컨트롤러가 다양한 방식의 컨트롤러를 처리할 수 있도록 변경해보자
* ### 유연한 컨트롤러1 -v5
![image](https://user-images.githubusercontent.com/60100532/174482835-5b3da09d-aa10-4df8-9ea5-f7e2f9c6b8f6.png)


* v1: 프론트 컨트롤러를 도입
  * 기존 구조를 최대한 유지하면서 프론트 컨트롤러를 도입
* v2: View 분류
  * 단순 반복 되는 뷰 로직 분리
* v3: Model 추가
  * 서블릿 종속성 제거
  * 뷰 이름 중복 제거
* v4: 단순하고 실용적인 컨트롤러
  * v3와 거의 비슷
  * 구현 입장에서 ModelView를 직접 생성해서 반환하지 않도록 편리한 인터페이스 제공
* v5: 유연한 컨트롤러
  * 어댑터 도입
  * 어댑터를 추가해서 프레임워크를 유연하고 확장성 있게 설계


* ### Spring MVC 전체 구조
  * ![image](https://user-images.githubusercontent.com/60100532/174597892-f79eae13-52f6-4688-a44f-428319eb5549.png)

* ### DispatcherServlet 구조 살펴보기
DispacherServlet.doDispatch()
```java
protected void doDispatch(HttpServletRequest request, HttpServletResponse response) throws Exception {
  HttpServletRequest processedRequest = request;
  HandlerExecutionChain mappedHandler = null;
  ModelAndView mv = null;
  
  // 1. 핸들러 조회
  mappedHandler = getHandler(processedRequest);
  if (mappedHandler == null) {
  noHandlerFound(processedRequest, response);
  return;
  }
  
  // 2. 핸들러 어댑터 조회 - 핸들러를 처리할 수 있는 어댑터
  HandlerAdapter ha = getHandlerAdapter(mappedHandler.getHandler());
  
  // 3. 핸들러 어댑터 실행 -> 4. 핸들러 어댑터를 통해 핸들러 실행 -> 5. ModelAndView 반환
  mv = ha.handle(processedRequest, response, mappedHandler.getHandler());
  processDispatchResult(processedRequest, response, mappedHandler, mv,dispatchException);
}

private void processDispatchResult(HttpServletRequest request,HttpServletResponse response, HandlerExecutionChain mappedHandler, ModelAndView mv, Exception exception) throws Exception {
    // 뷰 렌더링 호출
    render(mv, request, response);
}

protected void render(ModelAndView mv, HttpServletRequest request,HttpServletResponse response) throws Exception {
    View view;
    String viewName = mv.getViewName();
    // 6. 뷰 리졸버를 통해서 뷰 찾기, 7. View 반환
    view = resolveViewName(viewName, mv.getModelInternal(), locale, request);
    // 8. 뷰 렌더링
    view.render(mv.getModelInternal(), request, response);
}
 
```


* #### 동작 순서
1. 핸들러 조회: 핸들러 매핑을 통해 요청 URL에 매핑된 핸들러(컨트롤러)를 조회한다.
2. 핸들러 어댑터 조회: 핸들러를 실행할 수 있는 핸들러 어댑터를 조회한다.
3. 핸들러 어댑터 실행: 핸들러 어댑터를 실행한다.
4. 핸들러 실행: 핸들러 어댑터가 실제 핸들러를 실행한다.
5. ModelAndView 반환: 핸들러 어댑터는 핸들러가 반환하는 정보를 ModelAndView로 변환해서
   반환한다.
6. viewResolver 호출: 뷰 리졸버를 찾고 실행한다.
   JSP의 경우: InternalResourceViewResolver 가 자동 등록되고, 사용된다.
7. View 반환: 뷰 리졸버는 뷰의 논리 이름을 물리 이름으로 바꾸고, 렌더링 역할을 담당하는 뷰 객체를
   반환한다.
   JSP의 경우 InternalResourceView(JstlView) 를 반환하는데, 내부에 forward() 로직이 있다.
8. 뷰 렌더링: 뷰를 통해서 뷰를 렌더링 한다


* ### 핸들러 매핑과 핸들러 어댑터
> application.properties 
> 
> spring.mvc.view.prefix=/WEB-INF/views/
> 
> spring.mvc.view.suffix=.jsp
> 

* #### 뷰 리졸버 - InternalResourceViewResolver
  - 스프링 부트는 InternalResourceViewResolver 라는 뷰 리졸버를 자동으로 등록하는데, 이때
  application.properties 에 등록한 spring.mvc.view.prefix , spring.mvc.view.suffix 설정
  정보를 사용해서 등록한다.
  
> 참고로 권장하지는 않지만 설정 없이 다음과 같이 전체 경로를 주어도 동작하기는 한다.
  return new ModelAndView("/WEB-INF/views/new-form.jsp");