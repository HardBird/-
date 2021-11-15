## 시퀀스란? 
 - 시퀀스란 자동으로 순차적으로 증가하는 순번을 반환하는 데이터베이스 객체입니다. 
 - 보통 PK값에 중복값을 방지하기위해 사용합니다. 예를들어 게시판에 글이 하나 추가될때마다 글번호(PK)가 생겨야 한다고 해보겠습니다.
 - 만약 100번까지 글 번호가 생성되어있다면 그 다음 글이 추가가 되었을 경우 글 번호가 101으로 하나의 ROW를 생성해주어야 할것입니다.
 - 이때 101이라는 숫자를 얻으려면 기존 글번호중 가장 큰 값에 +1을 하는 로직을 어딘가에 넣어야하는데 
 - 시퀀스를 사용하면 이러한 로직이 필요없이 데이터베이스에 ROW가 추가될때마다 자동으로 +1을 시켜주어 매우 편리합니다.

```sql
--초기 시퀀스 세팅 문법
CREATE SEQUENCE [시퀀스명]
INCREMENT BY [증감숫자] --증감숫자가 양수면 증가 음수면 감소 디폴트는 1
START WITH [시작숫자] -- 시작숫자의 디폴트값은 증가일때 MINVALUE 감소일때 MAXVALUE
NOMINVALUE OR MINVALUE [최솟값] -- NOMINVALUE : 디폴트값 설정, 증가일때 1, 감소일때 -1028 
                               -- MINVALUE : 최소값 설정, 시작숫자와 작거나 같아야하고 MAXVALUE보다 작아야함
NOMAXVALUE OR MAXVALUE [최대값] -- NOMAXVALUE : 디폴트값 설정, 증가일때 1027, 감소일때 -1
                               -- MAXVALUE : 최대값 설정, 시작숫자와 같거나 커야하고 MINVALUE보다 커야함
CYCLE OR NOCYCLE --CYCLE 설정시 최대값에 도달하면 최소값부터 다시 시작 NOCYCLE 설정시 최대값 생성 시 시퀀스 생성중지
CACHE OR NOCACHE --CACHE 설정시 메모리에 시퀀스 값을 미리 할당하고 NOCACHE 설정시 시퀀스값을 메로리에 할당하지 않음
```
```sql
--예제
CREATE SEQUENCE EX_SEQ --시퀀스이름 EX_SEQ
INCREMENT BY 1 --증감숫자 1
START WITH 1 --시작숫자 1
MINVALUE 1 --최소값 1
MAXVALUE 1000 --최대값 1000
NOCYCLE --순한하지않음
CACHE; --메모리에 시퀀스값 미리할당
```
* * *
```sql
--시퀀스 수정 문법
ALTER SEQUENCE [시퀀스명]
INCREMENT BY [증가값]
NOMINVALUE OR MINVALUE [최솟값] 
NOMAXVALUE OR MAXVALUE [최대값]
CYCLE OR NOCYCLE [사이클 설정 여부]
CACHE OR NOCACHE [캐시 설정 여부]
```
```sql
--예제
ALTER SEQUENCE EX_SEQ
INCREMENT BY 2
MINVALUE 2
MAXVALUE 10000
CYCLE
NOCACHE;
```
- 시퀀스.NEXTVAL 을하면 테이블내의 가진 마지막 시퀀스 값에 +1이 된 값이 자동으로 인덱싱된다.
- 시퀀스.CURRVAL 을하면 해당 테이블내의 시퀀스값을 출력해줍니다. 
```sql
-- 해당 시퀀스의 다음값 
   SELECT testSeq.NEXTVAL FROM DUAL; -- 해당 시퀀스의 현재값
   SELECT testSeq.CURRVAL FROM DUAL;
--INSERT에서의 시퀀스 다음값 
   INSERT INTO oracleStudy VALUES(testSeq.NEXTVAL, 'studyName' , 'class' , A);
```
- 시퀀스를 수정 할 떼에 사작값이나 최솟값&최댓값에 관련하여 값을 변동할 때 영향을 끼치는 상황이 발생할 경우, 값을 변경할 수 없다.   
   (최댓값을 넘거나 최솟값보다 작은 경우)
- 시퀀스 역시도 테이블이기에 DROP을 사용하여서 삭제할 수 있다. 
- 하지만 아래와 같은 경우에는 NEXTVAL과 CURRVAL을 사용할 수 없다.
    - view의 select절 
    - distinct 구문 
    - group by, having, order by 구문 
    - subquery 구문
    - create talbe, alter table 명령의 default값 
 
 
 ## 인덱스란? 
  - Table Full Scan에서 속도와 효율적인 측면을 위해서 나온 기술이다.
  - 시퀀스와의 차이점은 연속적인 숫자로 데이터를 저장하는것이 아니다. 
  - 넣어야할 데이터가 적을경우에는 오히려 성능이 하락하는 경우가있다. ( Index Scan을 통하여 하기 때문이다.)    
       - 데이터는 약 3000개 정도가 넘어갈때부터는 인덱싱을 해주면 좋다.
  - 책에 책갈피를 꽂듯이 특정 구간을 색인하여 표시를 해두는 기술이다. 이를 Index Scan이라고 한다. 
  - 너무 많은 인덱스를 하게되면 성능이 저하되기 때문에 아래의 명령어를 사용하여 리빌딩을 해주도록하자. 
  ```sql 
  ALTER INDEX [인덱스명] REBUILD;
  ```


## 뷰란? 
 - 물리적인 뷰가아닌 가상의 테이블이다.
 - 이는 편리성과 재사용성을 위해서 나왔으나, 수정을 하더라도 원본데이터에는 영향을 끼치지 않는 객체이다. 
