


> Written with [StackEdit](https://stackedit.io/).

# 제약조건
물리적인 제약
**제약조건**을 설정하는 이유는 데이터의 **무결성**을 보장하기 위함
**무결성**을 보장하는 것은 무엇이냐?
데이터의 **일관성**, **유효성**, **정확성** 보장
이상한 데이터가 들어오지 않게 제약을 가하겠다.

## PK, FK, UK

### PK Primary Key
모델링 관점에서 하나의 테이블 => 즉 엔티티에는 반드시  **주식별자**가 존재해야 한다.
EX) **학생** 테이블의 **학번**
EX) **사원** 테이블의 **사번**
EX) **회원** 테이블의 **ID**
**주식별자**에 해당 -> 변하지 않고 중복되지 않아야 한다.

**주식별자**에 물리적 제약을 두는 것이 바로 **PK**
**주식별자**와 **PK**는 다른 의미
**주식별자 = 논리적, PK = 물리적**

그래서 **하나의 `테이블(엔티티)`에는 반드시 `주식별자`가 존재해야 하지만
`PK`는 존재할 수도 있고, 안할 수도 있다.**

기숙사 => 통금 12시일때 논리적은 12시가 통금시간 => 
물리적으로 12시에는 문을 잠글 수도 있고 잠그지 않을수도 있다.

주식별자와 PK는 100% 일치하지 않을수도 있지만, 
일반적인 경우에는 **주식별자를 PK로 설정**한다.

### PK의 특징
테이블 당 **하나씩만** 설정가능
**중복**과 **NULL**이 허용되지 않음
**PK**로 설정할 수 있는 컬럼이 **두개 이상 존재 가능** 
=> **학생** 테이블에 **학번**과 **주민번호**, 즉 둘 다 **PK**로 사용가능할 때
=> 좀 더 많이 쓰이고 길이가 짧은 **학번** 컬럼을 **PK**로 지정 
=> Unique Index가 자동으로 생성

**PK**를 설정하는 명령어
```SQL
CONSTRAINT PK이름 PRIMARY KEY(PK컬럼);
```
ex)
```SQL
ALTER TABLE PARENT ADD CONSTRAINT P_PK PRIMARY_KEY(P_ID);
```
### UK Unique Key
**고유키**라고 불림
테이블 당 여러개 설정 가능
중복은 허용되지 않지만 **NULL은 허용**된다.
**PK**와 마찬가지로 Unique Index가 자동으로 생성됨
**UK**를 설정하는 명령어
```SQL
CONSTRAINT UK이름 UNIQUE KEY (UK컬럼);
```

### FK Foreign Key
**외래키**라고 불림
**FK**로 설정된 컬럼이 있다. => 그것이 참조하는 **부모 테이블**이 있다.
예를 들어 회원 테이블에 있는 회원 등급 컬럼에 FK를 설정해놨다 
=> 회원 등급 테이블이 존재함을 의미
=> 회원 등급 테이블에 정의되어 있는 데이터만 회원 테이블에 있는 회원 등급 컬럼에 입력이 될 수 있다.
회원 등급 테이블에 다이아몬드, 골드, 실버 등급이 정의가 되어 있는데 
=> 회원 테이블의 회원 등급 컬럼에 패밀리 등급이 들어갈 순 없다. (참조 무결성 위배)
=> 단 NULL값이 들어가는 경우는 있다!

**FK**컬럼 설정 전에 당연히 **부모 테이블이 먼저 생성**되어 있어야 하고, **참조 컬럼과 데이터 타입도 반드시 일치**해야 한다. 
=> 지키지 않으면 **참조 무결성 위배**

부모 테이블에 **참조되는 컬럼**은 **PK**나 **UK**로 설정되어 있어야 함
그렇지 않을 경우에는 오류가 나게 되어있음
**FK**를 설정하는 명령어
```SQL
ALTER TABLE CHILD ADD CONSTRAINT FK이름 FOREIGN KEY (FK컬럼) REFERENCES 부모테이블 (참조할 컬럼);
```
Ex)
```SQL
CREATE TABLE PARENT (
	P_ID VARCHAR2(2) NOT NULL
);
ALTER TABLE PARENT ADD CONSTRAINT P_PK PRIMARY KEY(P_ID);
CREATE TABLE CHILD (
	C_ID VARCHAR2(2) NOT NULL,
	P_ID VARCHAR2(2)
);
ALTER TABLE CHILD ADD CONSTRAINT C_PK PRIMARY KEY(C_ID);
ALTER TABLE CHILD ADD CONSTRAINT C_FK FOREIGN KEY(P_ID) REFERENCES PARENT (P_ID);
```
SQL 코드 참고 : [https://www.youtube.com/watch?v=oyk3y1XzLW8](https://www.youtube.com/watch?v=oyk3y1XzLW8)
**CASCADE** 같은 옵션값을 줄수도 있음

참조 당하고 있는 부모 테이블의 튜플(혹은 레코드)를 삭제할 경우,  자식에서 이를 참조하고 있는 튜플이 있기 때문에 지울 수 없다며 오류가 발생 => 해결방법은 아래와 같다.
```SQL
ALTER TABLE CHILD DROP CONSTRAINT FK이름; -- 제약조건 삭제 후
ALTER TABLE CHILD ADD CONSTRAINT FK이름 FOREIGN KEY (FK 컬럼) REFERENCES PARENT (부모의 PK 혹은 UK컬럼) ON DELETE CASCADE;
```
**CASCADE**로 인해 부모 테이블의 레코드 삭제 시, 자식에서 그 레코드를 참조하는 레코드들도 같이 삭제된다.

**CASCADE** : **PARENT**삭제 시 **CHILD**해당 필드도 삭제
**SET NULL** :  **PARENT**삭제 시 **CHILD**해당 필드 NULL로 UPDATE
**SET DEFAULT**: **PARENT**삭제 시 **CHILD**해당 필드 DEFAULT 값으로 UPDATE
**RESTRICT**: **CHILD**에 **PK** 값이 없는 경우만 **PARENT** 삭제
**NO ACTION** : 참조 무결성 제약조건을 위배하는 행동 불가

### 실무에서는 외래키 사용을 매우 지양하는 편 이걸 사용하면 하나의 부모 테이블을 참조하는 자식 테이블이 너무 많음, 테이블 락이 걸릴 수도 있고, DML 과부하 가능성

## 식별자와 비식별자 관계 비교

![그림19](http://www.dbguide.net/publishing/img/knowledge/SQL_070.jpg)

## 정규화
정규화를 왜 하지?
> 데이터에 대한 중복성 제거, 데이터가 관ㅅ미사별로 
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTExNDk3MTYwMzgsMTA4OTg1ODY5MF19
-->