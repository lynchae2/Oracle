# PL/SQL

- 오라클에서 지원하는 프로그래밍 언어의 특성을 수용하여 SQL에서는 사용할 수 없는 절차적 프로그래밍 기능을 가지고 있어 SQL 단점을 보완 
  - Procedure, Function, Trigger
  - IF문을 사용하여 조건에 따라 문장들을 분기
  - LOOP 문을 사용하여 일련의 문장을 반복
  - CURSOR 를 사용하여 여러 행을 검색 및 처리



#### 기본적인 PL/SQL 블록 구조

| Section   | 설명                                                         | 필수여부 |
| --------- | ------------------------------------------------------------ | -------- |
| DECLARE   | 모든 변수나 상수를 선언하는 부분<br /> - 변수, 상수, 커서 선언 | 옵션     |
| BEGIN     | 절차적 형식으로 SQL문을 실행할 수 있도록 절차적 언어의 요소인 제어문, 반복문, 함수 정의 등 로직을 기술할 수 있는 부분<br /> - 실제적 연산처리 | 필수     |
| EXCEPTION | PL/SQL문이 실행되는 중 에러가 발생할 수 있는데 이를 예외 사항이라고 한다. 이러한 예외 상황이 발생했을 때 이를 해결하기 위한 문장을 기술할 수 있는 부분<br /> - 예외처리 | 옵션     |
| END       | 실행문 종료                                                  | 필수     |



#### PL/SQL 프로그램의 작성 요령

- PL/SQL
  - 변수선언
  - 제어문
  - 프로시저(input, output, cursor)



- 변수 선언 방법

```sql
DECLARE NAME VARCHAR(10);
DECLARE NAME VARCHAR(10) := '이채린';
DECLARE NAME VARCHAR(10) DEFAULT '이채린';
```



- 데이터 타입

  - 스칼라 : number(), VARCHAR2(), CHAR(), INT, DATE ...

  - 레퍼런스

    - %TYPE : 칼럼 단위로 데이터 타입 참조

      ex ) emp의 ename 필드의 타입을 참조한다. 

      ```sql
      DECLARE name emp.ename%TYPE;
      ```

      ​	   

    - %ROWTYPE : 행 전체에 대한 데이터 타입을 참조

      ex ) emp의 행 전체의 데이터 타입을 참조한다.

      ```sql
      DECLARE data_base emp%ROWTYPE;
      ```

  

- 대입문

  - 변수값 저장 :=

  - INTO 절에는 조회 결과 값을 저장할 변수를 기술, SELECT 문은 INTO 절에 의해 하나의 행만을 저장 가능 

    - ex ) 검색을 통해 얻은 eno, ename 값을 v_eno, v_ename 변수에 할당한다. 

      ```sql
      SELECT eno, ename INTO v_eno, v_ename	
      
      FROM employee
      
      WHERE ename='SCOTT';
      ```



- 제어문

  - IF

    ```
    IF 조건식 THEN 실행문장;
    
    [ELSIF 조건식 THEN 
    
    실행문장;
    
    ..
    
    ELSIF 조건식 THEN 
    
    실행문장;
    
    ...
    
    ELSE 
    
    실행문장;
    
    ..]
    
    END IF;
    ```

    

  

  ​		ex )  EMP테이블에서 empno가 7788인 레코드의 DEPTNO 가 10이면 'ACCOUNTING' 

  ​				20이면 'Research' 30이면 'SALES', 40이면 'OPERATION'을 출력해주세요.

  ​				변수(dname)에 담아서 if문 끝난 다음에 출력해주세요.

  ```sql
  DECLARE e_empno emp.EMPNO%TYPE ; 
  		e_name emp.ENAME%TYPE;
  		e_deptno EMP.DEPTNO%TYPE;
          e_str VARCHAR2(20);
          e_dname dept.dname%TYPE;            
  BEGIN
  	SELECT empno , ename, deptno  INTO e_empno, e_name,e_deptno FROM EMP WHERE empno='7788';
  	IF e_deptno = 10 THEN e_dname := 'ACCOUNTING' ;
  	ELSIF e_deptno = 20 THEN e_dname :='Research';
  	ELSIF e_deptno = 30 THEN e_dname := 'SALES';
  	ELSIF e_deptno = 40 THEN e_dname := 'OPeration';
  	ELSE e_dname:='부서이름 없음';
  	END IF;
  
  	DBMS_OUTPUT.PUT_LINE('사번    이름    부서명 '); 
  	DBMS_OUTPUT.PUT_LINE(e_empno ||'   '||e_name ||'    '||e_dname);
  	
  END;
  ```

  

  

  - LOOP

    - FOR LOOP문 

    ```sql
    BEGIN
    	FOR i IN 1 .. 10 LOOP				-- 변수 i를 1부터 10까지 LOOP 반복 
    		DBMS_OUTPUT.put_line(i);		-- 실행 문장
        END LOOP;							-- LOOP 끝 
    END;
    ```

    => FOR LOOP는 반복횟수가 정해진 반복문을 처리하는데 유용

    => LOOP을 반복할때마다 자동적으로 1씩 증가 

    => REVERSE를 지정하면 끝에서 시작까지 반복하면서 인덱스를 1씩 감소

    

    

    - BASIC LOOP (LOOP END) 문 

    ```SQL
    DECLARE i INT :=1;
    BEGIN
    	LOOP						-- LOOP 시작
    	DBMS_OUTPUT.put_line(i);	-- 실행문장
    	i:= i+1;					-- 증감식
    	EXIT WHEN (i>10);			-- EXIT [WHEN 조건식]	조건식 만족 시 LOOP 빠져나감
        END LOOP;					-- LOOP 끝
    END;
    ```

    => EXIT 문이 사용되었을 경우 무조건 LOOP빠져나간다.

    => EXIT WHEN 일  사용되었을경우 WHEN절에 LOOP를 빠져 나가는 조건 제어할수있다.

    

    

    - WHILE LOOP문

      ```SQL
      DECLARE i INT :=1;
      	BEGIN
      		WHILE (i<=10) loop					-- LOOP 시작, 조건식
      			DBMS_OUTPUT.put_line(i);		-- 실행문장
              	i:= i+1;						-- 증감식
           	END LOOP;							-- LOOP 끝
        	END;
      ```

      => WHILE LOOP문은 제어 조건이 TRUE인 동안만 문장을 반복
