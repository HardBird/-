## 스트럿츠 프레임구조 
[이미지를 못가져와서 해당 링크참조](https://kkiuk.tistory.com/67)
- 기본적으로 스프링 MVC 패턴처럼 큰 틀에서는 Model, View, Controller로 나뉘어서 작동한다. 
- View 에서는 JSP, Controller에서는 Action 관련된 클래스, Model에서는 DB관련 내용들이 저장되어있다.

## 스트럿츠 문법
[문법의 양이 많으나 설명에대한 래퍼가 적다. 필요할때 찾아서 쓰자.](https://secr.tistory.com/151)

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
