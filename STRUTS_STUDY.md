## 스트럿츠 프레임구조 
[이미지를 못가져와서 해당 링크참조](https://kkiuk.tistory.com/67)
- 기본적으로 스프링 MVC 패턴처럼 큰 틀에서는 Model, View, Controller로 나뉘어서 작동한다. 
- View 에서는 JSP, Controller에서는 Action 관련된 클래스, Model에서는 DB관련 내용들이 저장되어있다.

## 스트럿츠 문법
[문법의 양이 많으나 설명에대한 래퍼가 적다. 필요할때 찾아서 쓰자.](https://secr.tistory.com/151)   
[프로젝트에서 제일 유사하게 사용하는 문법을 정리한 사이트이다. 프로젝트 이해할 때 참고하면서보자.](https://heeestorys.tistory.com/469)
## 스트럿츠 작동원리 
- ActionServlet 
  - 클라이언트의 요청을 직접받는 곳이며, 해당 요청을 처리해줄 RequestProcessor를 찾아서 전달한다.
  
- Action Form 클래스
  - 기본적으로 클라이언트 측에서 요구가 있을대 데이터를 입력하는 폼이 생기면 발생하는 이벤트이다. 
  - ActionServlet으로 들어온 요청을 사용자와 비지니스 게층들 사이로 전달한다. 
  - Action 인스턴스에 폼 빈을 이용하여 DTO 구조로 데이터를 주고받는다.
  
- Action 클래스 
  - 요청에 따라서 발생하는 로직이다. FLUX 패턴에 Action과 같은 원리이다. 
  - 해당 결과값은 DTO형태나 Action Form에 저장된다.
  - Model에서 받은 결과 값 Action Forward를 RequestProcessor로 변환해준다.   
  (RequestProcessor는 최종적인 Action 실행로직이나 View를 수정해줄때 쓰이는 클래스이다.)
  
- Struts 설정파일
  - JSP의 시작은 POM.xml의 버전관리처럼 사용하는 Struts를 사용하기 위한 기분 설정 파일이다. 
  - 이는 ActionForm 클래스를 설정하고 Action 클래스를 설정하고 라우팅을 해주는 부분이다.  
  
- ActionMapping 클래스 
  - 이는 복수의 요청에 대한 Action이 설정되어 있다. 
  - Struts설정 파일의 Action에 대한 요소내용을 보관하는 클래스이다.  


## 스트럿츠 프로젝트 끄적끄적 
  - 기본적인 작동구조는 JSP에서 라우팅을 해주느 개념으로 이해해주면 된다. 
    - form 이벤트를 주면 해당 struts.xml으로 이동해서 각종 이벤트에 맞게 핸들링된다 
    - struts에서 이벤트를 받으면 해당 이벤트에 맞는 DAO로 가서 이벤트처리후에 이벤트값을 return 해준다. ( return 값은 임의이다. ) 
    - return 값은 임의이지만 success를 준다면 result 태그에서 아무것도 옵션을 주지않은 result로 이동하여서 이벤트를 수행한다.
    - 이는 보통 라우팅경로를 주어서 값을 가지고 이동할 수 있게 해주며, 조금 더 확장된 기능으로는 interceptor가 있다.
    - 옵션을 주지 않으면 default로 result는 success로 가지만    
    기본적으로 name="success"를 줘서 코드리뷰를 할 때에 명확성을 주도록 하자. 
      
  - 만약에 라우팅을 하는 경로를 따라가라가려하는데 보이지 않는다면 밑에 namespace를 따라가보자 .. 종종 숨어있다. 
    - 해당 다이렉팅 폴더에서 action 값을 찾으면 해당 struts도 따라서 찾을 수 있다.
  - 보통 데이터를 뿌려줄때에는 s:iterator id="temp" value="#id" 를통해서 특정 id값을 대입해서 데이터를 빼온뒤에    
  그 값을 넣어주고있는 iterator의 id값을 이용해서 페이지 핸들링 처리를 해주면 된다. 
  
## 스트럿츠 인터셉터 
 - 이는 사전 유효성을 검사해주는 문법이다.
 - 가장큰 메인 struts 파일에서 import를 하여서 초기 선언을해주고 사용하고자하는 action에서
으로 선언을 해주어서 비즈니스 로직전 검사를 수행할 수 있다.
 - 기본적으로 프로젝트 프로세스는 뷰 -> 인터셉트 -> 비니지스 로직의 과정을 거친다. 뷰에서 유효성검사를 실시해도, console로 값을 넣으면 들어가기 때문에,
보통은 공통적으로 해야하는 세션및 쿠키체크나 계열사 URL 필터링등을 수행해준다. 로그인이후에 비지니스 로직에서 유효성 검사를 하면
이미 DB에 접근해서 유효성 검사를하기에 보안적으로 문제가 되는 부분이 있어서 interceptor에서 필터링을 해줘야 한다.
## 스트럿츠 디자인패턴 
 - 보통 페이지의 레이아웃 효율성과 재사용성을 높이기 위해서 사용되는 기술이다.
 - 기본적으로는 데코레이터 디자인패턴을 사용한다. 
 - 개념으로는 여러페이지가 있는데 헤더가 고정돼서 사용이 될 경우에 작성한다. (리액트의 HOC 개념이랑 같다.)
 - 사용방법
  - 첫째로 pom.xml, web.xml에 의존성을 추가해준다.    
 ```html
    -pom.xml
         <dependency>
            <groupId>opensymphony</groupId>
            <artifactId>sitemesh</artifactId>
            <version>2.4.2</version>
        </dependency>

    -web.xml 
        <!-- sitemesh -->
            <filter>
            <filter-name>sitemesh</filter-name>
            <filter-class>com.opensymphony.module.sitemesh.filter.PageFilter
            </filter-class>
            </filter>
            <filter-mapping>
            <filter-name>sitemesh</filter-name>
            <url-pattern>/*</url-pattern>
            </filter-mapping>
```
  - 추가를 하였으면 새로운 xml파일을 만들어서 기본세팅인 decorators.xml 과 의존성들을 추가해줄 sitemesh.xml을 추가해준다.        
```html
    -decorators.xml 
            <?xml version="1.0" encoding="UTF-8"?>

            <decorators defaultdir="/decorators">
            <excludes>
            <pattern>/index.do</pattern>
            </excludes>

            <decorator name="layout" page="/WEB-INF/views/layout/layout.jsp">
            <pattern>/*</pattern>
            </decorator>

            <decorator name="layout2" page="/WEB-INF/views/layout/layout2.jsp">
            <pattern>/web/*</pattern>
            </decorator>
            </decorators>

    -sitemesh.xml (예시)
            <sitemesh>

            <page-parsers>
            <parser default="true" class="com.opensymphony.module.sitemesh.parser.DefaultPageParser"/>
            <parser content-type="text/html" class="com.opensymphony.module.sitemesh.parser.FastPageParser"/>
            </page-parsers>

            <decorator-mappers>
            <mapper class="com.opensymphony.module.sitemesh.mapper.PageDecoratorMapper">
            <param name="property.1" value="meta.decorator"/>
            <param name="property.2" value="decorator"/>
            </mapper>

            <mapper class="com.opensymphony.module.sitemesh.mapper.FrameSetDecoratorMapper">
            </mapper>

            <mapper class="com.opensymphony.module.sitemesh.mapper.AgentDecoratorMapper">
            <param name="match.MSIE" value="ie"/>
            <param name="match.Mozilla [" value="ns"/>
            <param name="match.Opera" value="opera"/>
            <param name="match.Lynx" value="lynx"/>
            </mapper>

            <mapper class="com.opensymphony.module.sitemesh.mapper.PrintableDecoratorMapper">
            <param name="decorator" value="printable"/>
            <param name="parameter.name" value="printable"/>
            <param name="parameter.value" value="true"/>
            </mapper>

            <mapper class="com.opensymphony.module.sitemesh.mapper.RobotDecoratorMapper">
            <param name="decorator" value="robot"/>
            </mapper>

            <mapper class="com.opensymphony.module.sitemesh.mapper.ParameterDecoratorMapper">
            <param name="decorator.parameter" value="decorator"/>
            <param name="parameter.name" value="confirm"/>
            <param name="parameter.value" value="true"/>
            </mapper>

            <mapper class="com.opensymphony.module.sitemesh.mapper.FileDecoratorMapper">
            </mapper>

            <mapper class="com.opensymphony.module.sitemesh.mapper.ConfigDecoratorMapper">
            <param name="config" value="/WEB-INF/decorators.xml"/>
            </mapper>

            </decorator-mappers>

            </sitemesh>
```     
 - 그리고 베이스가 되는 레이아웃 페이지에서 상단 <header> 태그안에 <decorator:head /> 혹은 <decorator:body />  를 추가해준다. 
  - 그리고 베이스를 받는 레이아웃 부분에서는 그냥 <head></head>로 태그를 닫아두면 자동으로 sitemash가 적용이된다. 
  - 그리고 한글이 깨질수 있으니 UTF8 인코딩을 해주는것을 잊지 말자 ! 
  - xml파일에서는 부등호가 적용이 되지 않는다 그렇기 때문에 &lt ;(<) &gt ;(>) 를 사용하자    
  ## JAVA extends 
  - 상속의 대표적인 형태이다.
  - 모든 선언/정의는 부모가 하며, 자식은 오버라이딩 할 필요 없이 부모의 메소드와 변수를 그대로 사용할 수 있다. 
  - 물론 필요헤 따라 오버라이딩을 할 수 있다. 
  ```java
    class Vehicle {
    protected int speed = 3;

    public int getSpeed(){
      return speed;
    }
    public void setSpeed(int speed){
      this.speed = speed;
    }
  }

  class Car extends Vehicle{
    public void printspd(){
      System.out.println(speed);
    }
  }

  public class ExtendsSample {
    public static main (String[] args){
      Car A = new Car();
      System.out.println(A.getSpeed());
      A.printspd();
    }
  
  cf)
  Car 클래스는 Vehicle 클래스의 변수와 메소드를 상속 받았다.
  따라서, Car 클래스는 Vehicle 클래스의 speed, getSpeed(), setSpeed()를 사용할 수 있다.
  하지만, Java는 "다중상속"을 지원하지 않는다.
  즉, 부모 클래스가 두 개 이상 존재할 수 없다는 것이다.
  ex) public class Son extends Father, Mother {...}
  하지만, Java는 implements를 사용해 여러 interface를 상속 받을 수 있다.
  ```   
  ## JAVA implements
  - 부모는 선언만 하며, 자식이 오버라이딩을해서 사용하는 구조이다.
  - 이는 부모의 메서드를 재정의를 해야해서 상소의 개념에 벗어날지 모르지만 JAVA특정상 계약 및 분류의 의미가 강하여 사용한다. 
  ```java
     interface TestInterface{
    public static int num = 8;
    public void fun1();
    public void fun2();
  }

  class InterfaceExam implements TestInterface{
    @Override
    public void fun1(){
      System.out.println(num);
    }

    @Override
    public void fun2() {

    }
  }

  public class InterfaceSample{
    public static void main(String args[]){
      InterfaceExam exam = new InterfaceExam();
      exam.fun1();
    }
  }  
  ```   
  ## STRUTS2 DAO,INTERFACE,BEAN 
    - 기본적으로 최상단 .xml에서 인터페이스와 DAO를 선언해줘야한다.     
    - ID값이 다르면 호출을 하더라도 null 값을 호출하고 서비스나 DAO를 대입시킬 수 없다.   
    - 생성자인 BEAN은 별도의 선언없이도, 생성자로 접근하면되기 때문에 핸들링이 필요없다.   
  
  ## 인프라인증 CI&DI
  ![images](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F991CFB385EE8DDB123)
  - CI
    - Connecting Information 의 약자이며 88bytes로 이루어져있다.
    - 이는 주민등록번호처럼 사용자의 고유 번호라고 하면 된다.
    - 어디 사이트나 값이 같기에 조금 큰 서비스나 통합 서비스를 이용할 때 사용된다. 
  - DI 
    - Duplication Information 의 약자이면 64bytes로 이루어져있다. 
    - 이는 중복가입체크를 하거나 입력한 정보로 한 사람의 명의로 여러개의 계정을 만들어내는 것을 방지 할 수 있다. 
    - 특정사이트에서 회원가입을 하면 CI처럼 그 사이트내에 DI값은 같기 때문에 이 방법을 사용해서 중복가입을 막을 수 있다.
  
  ## SSO
  ![images](https://t1.daumcdn.net/cfile/tistory/99CE614E5B27749C0D) 
  - Single Sign-On의 약자로 한번의 로그인으로 다른곳에서도 자동적으로 사용하여 별도의 인증없이 서비스를 이용하는 기술이다.     
  - (1) 사용자 통합 로그인 (2) 인증 서버 (3) 통합 에이전트 : 각 정보 시스템에 대한 정보 관리으로 이루어져있다.     
  - 사용자 주소 제한이나 유효 시간 제한같은 보안 기술을 사용하며, 토큰을 네트워크에 노출시키지 말아야한다. 
  
  ## 크롬80 이슈     
  - 기존에 있던 쿠키 특성중 Samesite=None 에서 Samsite=Lax가 도입된것    
  - Samsite란 Cross-Site Request Forgery(CSRF) 공격 클래스에 대한 방어책이다. 
  - None을 설정하면 외부 사이트에 쿠키 전송할 범위를 정할 수 있는데 그 범위는 아래와 같다.    
  
  ```
  Set-Cookie: CookieName=CookieValue; SameSite=Strict;
  Set-Cookie: CookieName=CookieValue; SameSite=Lax; 
  Set-Cookie: CookieName=CookieValue; SameSite=None; Secure
  
    - Strict : 이는 특정한 경우 없이, 도메인이 다를경우에 쿠키를 전송하지 않는다.
    - Lax : 현재 크롬80 이슈를 발생시킨 기본 형식이다 이는 기본 GET 요청과 같은 특정한 경우에만 쿠키를 전송한다. 
            쿠키를 전송하는 경우 : 링크(href, link), FORM 태그의 GET 
            쿠키를 전송하지 않는 경우 : iframe, Ajax, Image, FORM 태그의 POST 
    - None : 다른 도메인에 쿠키를 전송하는 경우이다. 이는 원래 그냥 선언만해주면 자유롭게 사용할 수 있지만     
             쿠키에 암호화된 HTTPS 연결을 해야한다. 해당 HTTPS 연결은 아래 Secure 특성을 선언해주면 된다.    
              SameSite=None; Secure (조건은 URL이 HTTPS로 설정이 되어야 한다.)
  ```    
  ## XSS
    - 이는 cross site script의 약자로 input 태그안에 script 문법을 추가하여서 로직적인 스크립트 실행이 아닌,     
    의도적으로 스크립트를 만들어서 서버에 부하나 쿠키 값 탈취등을 하는 SQL INJECTION의 기본적인 공격법이다. 
    - 이를 해결 할 때에는 스크립트태그인 <> 부분을 일반 문자로 치환시켜서 &lg 나 &gt로 바꾸어서 문자를 출력해주면     
    쉽게 해결이 가능한 부분이다. 
    - 출력하는 구문에 아래의 문장을 추가해주면된다.     
  ```java
  출력문자열.replaceAll("<","&lt;")
  ```
  
