# Procedure(프로시저)

#### 프로시저

- 기능(작업단위)을 미리 만들어 놓고 호출하여 사용하는 것

  - 빠른 쿼리 구문 실행

  - 재사용성

    - 프로시저 생성 방법 (인수 없는 경우)

      ```sql
      CREATE PROCEDURE p_test
      IS
      BEGIN
      	dbms_output.put_line('Oracl 시험을 잘 볼수있겠죠~ㅎㅎ');
      END;
      
      --프로시저 호출 (인수 없는 경우)
      EXEC p_test;
      ```

      

    - 프로시저 생성 방법(인수 있는 경우)

      ```sql
      CREATE OR REPLACE PROCEDURE p2_test( str_name IN VARCHAR2  )
      IS
      BEGIN
      	dbms_output.put_line(str_name||' 님 정보처리 시험에 합격하셨습니다.');
      END;
      
      --프로시저 호출
      EXEC p2_test; --오류발생 (프로시저에 in이 있으면서 기본값이 없기 때문에 반드시 인수값 필요함)
      
      EXEC p2_test('이채린');	-- str_name 인수에 '이채린' 값이 들어간다.
      ```

      

      ex )  인수 아이디, 이름, 나이, 주소를 입력받아 userlist테이블에  INSERT 한후 인수의 값들을 출력하는 프로시저 작성한다. (인수에 기본값 지정해본다.)

      ```sql
      CREATE OR REPLACE PROCEDURE p_userinsert(
      	id IN userlist2.ID%TYPE := 'kkk',
          NAME IN userlist2.name%TYPE DEFAULT '홍길동',
          age IN USERLIST2.AGE%TYPE := 10,
          addr IN userlist2.addr%TYPE DEFAULT '서울'
      )
      IS
      BEGIN
      	INSERT INTO userlist2 VALUES(id,name,age,addr);
          dbms_output.put_line('insert정보는 ' ||id||NAME||age||addr);
      END;
      
      --프로시저 실행
      EXEC p_userinsert('java2','자바2',26,'평택');
      EXEC p_userinsert('java5','자바5',27,'경기도');
      EXEC p_userinsert;  -- 인수 넣지 않으면 기본값으로 들어감
      
      ```

      

    - 프로시저 output 매개변수 사용하기

      - 프로시저를 실행하여 특정 결과값을 out 변수에 저장하여 보냄

      - 작성방법

        ```sql
        CREATE OR REPLACE PROCEDURE p_outTest(
        	name OUT VARCHAR2, 
            age OUT VARCHAR2			-- 프로시저를 호출하는 곳으로 값을 보낸다.
        )
        IS
        BEGIN
        	name := '이나영';
            age :=20;
            DBMS_OUTPUT.PUT_LINE('out을 이용한 프로시저 완료');
        END;
        
        
        --out이 있는 프로시저 호출방법
        --DECLARE 로 선언되변수는 일회용
        
        VARIABLE v_name VARCHAR2(20);
        VARIABLE v_age NUMBER;
        
        EXEC p_outTest(:v_name, :v_age);  -- 프로시저를 실행 한후에 out을 받을 변수지정
        
        print v_name; -- 출력하기
        print v_age; -- 출력하기
        ```

        

