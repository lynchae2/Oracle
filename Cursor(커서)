# CURSOR(커서)

- 오라클 서버에서 할당한 전용 메모리 영역에 대한 포인터
- 질의의 결과로 얻어진 여러 행이 저장된 메모리상의 위치
- 커서는 SELECT 문의 결과 집합을 처리하는데 사용된다.



- 종류

  1) 암시적 커서

  - 정의

    - 모든 SQL 문장은 암시적 커서가 생성되며, 커서 속성을 사용할 수 있다.
    - 모든 DML과 PL/SQL SELECT문에 선언된다.
    - 암시적인 커서는 오라클이나 PL/SQL 실행 메커니즘에 의해 처리되는 SQL 문장이 처리되는 곳에 대한 익명의 주소이다.
    - 오라클 서버에서 SQL문을 처리하기 위해 내부적으로 생성하고 관리한다.
    - 암시적 커서는 SQL문이 실행되는 순간 자동으로 OPEN, CLOSE 를 실행한다.
    - SQL 커서 속성을 사용하면 SQL 문의 결과를 테스트할 수 있다.

     

  - 속성

    - SQL%FOUND : 해당 SQL 문에 의해 반환된 총 행수가 1개 이상일 경우 TRUE
    - SQL%NOTFOUND : 해당 SQL 문에 의해 반환된 총 행수가 없을 경우 TRUE
    - SQL%ISOPEN : 항상 FALSE, PL/SQL 은 실행 후 바로 묵시적 컷서를 닫기 때문에
    - SQL%ROWCOUNT : 해당 SQL 문에 의해 반환된 총 행수, 가장 최근 수행된 SQL 문에 의해 영향을 받은 행의 개수

    

    ```SQL
    SET SERVEROUTPUT ON;
    DECLARE
        BEGIN
        DELETE FROM emp WHERE DEPTNO = 10;
        DBMS_OUTPUT.PUT_LINE('처리 건수 : ' || TO_CHAR(SQL%ROWCOUNT)|| '건');
        END;
        
    -- 결과 
    처리 건수 : 21건
    PL/SQL 처리가 정상적으로 완료되었습니다.
    
    ```



​	2) 명시적 커서

- 정의
  - 프로그래머에 의해 선언되며, 이름있는 커서	
- 속성
  - %ROWCOUNT : 현재까지 반환된 모든 행의 수를 출력
  - %FOUND : FETCH한 데이터가 행을 반환하면 TRUE
  - %NOTFOUND : FETCH한 데이터가 행을 반환하지 않으면 TRUE (LOOP를 종료할 시점을 찾는다.)
  - %ISOPEN : 커서가 OPEN되어있으면 TRUE



- 문법

  - 명시적 커서 FOR LOOP 

  ```sql
  SET SERVEROUTPUT ON;
  DECLARE
      CURSOR ID_LIST IS
      SELECT 'GOD' AS USER_ID FROM DUAL;
  BEGIN
      FOR TEST_CURSOR IN ID_LIST
      LOOP
          DBMS_OUTPUT.putline(TEST_CURSOR.USER_ID);
      END LOOP;
  END;
  
  ```

  

  ```SQL
  SET SERVEROUTPUT ON;
  DECLARE
  BEGIN
      FOR ID_LIST IN 
      (
          SELECT 'GOD' AS USER_ID FROM DUAL
      )
      LOOP
      DBMS_OUTPUT.putline(ID_LIST.USER_ID);
  END LOOP;
  END;
  
  ```

  
