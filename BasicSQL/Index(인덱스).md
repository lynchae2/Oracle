# 인덱스

#### 인덱스

- 테이블이나 클러스트에서 쓰여지는 선택적인 객체
- 오라클 DB의 테이블 내의 원하는 레코드를 빠르게 찾아갈 수 있도록 만들어진 데이터 구조
  - 자동 인덱스 : PK 또는 UNIQUE 제한 규칙에 의해 자동으로 생성되는 인덱스
  - 수동 인덱스 : CREATE INDEX 명령을 실행해서 만드는 인덱스 







#### 인덱스를 생성하는 것이 좋은 컬럼

- WHERE 절이나 JOIN조건 안에서 자주 사용되는 컬럼
- NULL 값이 많이 포함되어 있는 컬럼
- WHERE 절이나 JOIN 조건에서 자주 사용되는 두 개 이상의 컬럼들







#### 인덱스 종류

1. 비트맵 인덱스

   - 각 컬럼에 대해 적은 개수의 독특한 값이 있을 경우 ex) 남, 여

   - 테이블이 매우 크거나 수정/변경이 잘 일어나지 않는 경우에 사용

     ```sql
     CREATE BITMAP INDEX emp_deptno_indx ON emp(deptno); 
     ```

     

2. UNIQUE 인덱스

   - 인덱스를 사용한 컬럼의 중복값들을 포함하지 않고 사용할 수 있는 장점이 있다.

   - PK와 UNIQUE 제약 조건 시 생성되는 인덱스

     ```sql
     CREATE UNIQUE INDEX emp_ename_indx
     ON emp(ename);
     ```

     

3. NON-UNIQUE 인덱스

   - 인덱스를 사용한 컬럼에 중복 데이터 값을 가질 수 있다.

     ```sql
     CREATE INDEX dept_dname_indx
     ON dept(dname);
     ```

     

4. 결합 인덱스

   - 복수개의 컬럼에 생성할 수 있으며 복수키 인덱스가 가질 수 있는 최대 컬럼값은 16개 이다.







#### 인덱스 삭제

- 인덱스의 구조는 테이블과 독립적이므로 인덱스의 삭제는 테이블의 데이터에는 아무런 영향도 미치지 않는다.

- 인덱스를 삭제하려면 인덱스의 소유자 이거나 DROP ANY INDEX권한을 가지고 있어야 한다.

- 인덱스는 ALTER를 할 수 없다.

  ```sql
   DROP INDEX emp_empno_ename_indx; 
  ```

  
