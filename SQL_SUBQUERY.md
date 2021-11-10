## 서브쿼리란?
  - SQL을 조금 더 폭 넓게 사용할 수 있는 기술이다. SQL문안에 SQL문을 넣으며 종류는 아래와 같다.   
  - 이는 JOIN으로도 다 표현이 가능하다. 속도면에서도 JOIN이 우세하다.
  - 하지만 오라클의 경우 스칼라서브쿼리가 종종빠를 경우가 있다.
  
|사용위치|명칭|
|------|---|
|SELECT절|스칼라 서브쿼리|
|FROM절|인라인 뷰|
|WHERE절|단일행서브쿼리 or 복수행서브쿼리|


### 스칼라서브쿼리 
![images](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FCF0TG%2FbtqD3d8IddN%2FKKeLuMlT2MmfNV5dth4HOk%2Fimg.png)
  - 기본적으로 SELECT안에 서브쿼리가 들어가는 구조이다. 
  - 이를 사용할 때 중요한 점은 서브 쿼리의 결과가 한개만 나와야한다. 대표적인 예시는 아래의 기재하도록 하겠다.
  ```sql
  
  
  select name as 학생이름, (select major_title from class.major b where b.major_id = a.major_id) as 학과명 from class.student a;   
  select major_title as 학과명, (select name from class.student b where b.major_id = a.major_id) as 학생이름 from class.major a;   
  
  
  
    - 이 둘의 차이는 서브 쿼리의 결과 값인데 아래는 오류를 발생시킨다.   
    그 이유는 학과는 고유한 값 즉, 한개의 결과를 출력하는대 학생에 대학 학과는 중복이 될 수 있다. 
       그렇기 때문에 서브쿼리 결과값이 2개가 나오기에 후자의 쿼리는 오류를 발생시킨다.      
    (철수는 영희와 같은과 같은것처럼)
  ```

### 인라인 뷰 
![images](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FnyHvQ%2FbtqD5Jr9t8i%2FhmTueWjXrAZSwRlSBKKzB0%2Fimg.png)
  - 기본적으로 FROM문안에 서브쿼리가 들어가는 구조이다. 
  - 검색을 하는 테이블에 서브쿼리를 넣는것이기에 인라인뷰에 없는 행은 그 어디에서도 사용할 수 없다. 
  - 그렇기에 인라인뷰에서 선언한 행들은 메인쿼리에서든 사용할 수 있다. 
  ```sql 
  
  select a.name as 학생이름, b.major_title as 학과명      
  from class.student a, (select major_title, major_id from class.major) b      
  where a.major_id = b.major_id;


  ```
### 단일행서브쿼리&복수행서브쿼리
  - 중첩서브쿼리와 서브쿼리는 아래의 표에 쓰이는 것에 따라서 정의하는 것이 달라진다. 
  - 쉽게 이해해서 연산자와 문법의 차이이다. 
##### 단일행 서브쿼리 

|연산자|의미|
|------|---|
|=|같다|
|<>|같지않다|
|>|크다|
|>=|크거나 같다|
|<|작다|
|<=|작거나 같다|
```sql
select name as 학생이름    
from class.student    
where major_id = (select major.major_id from class.major where major_title = '컴퓨터공학과');
```
##### 복수행 서브쿼리 

|연산자|의미|
|------|---|
|IN(NOT IN)|모두 포함함|
|EXIST|서브쿼리의 값이 있는 경우 반환|
|NOT EXIST|서브쿼리의 값이 없는 경우 반환|
```sql
select name as 학생이름   
from class.student    
where major_id in (select major.major_id from class.major where major_title in ('컴퓨터공학과','국문학과'));
```
