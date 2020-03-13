-- GROUP BY 절 사용 결과

SELECT B.dname, A.job, SUM(A.sal) sal, COUNT(A.empno) emp_cont
FROM emp A, dept B
WHERE A.deptno = B.deptno
GROUP BY B.dname, A.job;

DNAME       JOB               SAL  EMP_COUNT
----------  ---------- ---------- ----------
ACCOUNTING  CLERK            1300          1
ACCOUNTING  MANAGER          2450          1
ACCOUNTING  PRESIDENT        5000          1
RESEARCH    ANALYST          6000          2
RESEARCH    CLERK            1900          2
RESEARCH    MANAGER          2975          1
SALES       MANAGER         28500          1
SALES       SALESMAN         4000          3


-- 부서별로 인원과, 급여합계가 한 눈에 보이지 않는다.
-- 일일이 부서에 해당하는 직업별 급여와 사원수를 일일이 더해야 한다.

-- 이런 경우 ROLLUP을 사용하여 쉽게 조회 할 수 있다.


SELECT B.dname, A.job, SUM(A.sal) sal, COUNT(A.empno) emp_cont
FROM emp A, dept B
WHERE A.deptno = B.deptno
GROUP BY ROLLUP(B.dname, A.job);

DNAME      JOB               SAL  EMP_COUNT
---------- ---------- ---------- ----------
ACCOUNTING CLERK            1300          1
ACCOUNTING MANAGER          2450          1
ACCOUNTING PRESIDENT        5000          1
ACCOUNTING                  8750          3  -->  ACCOUNTING 부서의 급여 합계와 전체 사원 수
RESEARCH   ANALYST          6000          2
RESEARCH   CLERK            1900          2
RESEARCH   MANAGER          2975          1
RESEARCH                   10875          5 -->  RESEARCH 부서의 급여 합계와 전체 사원 수
SALES      MANAGER         28500          1
SALES      SALESMAN         4000          3
SALES                      32500          4 -->  SALES부서의 급여 합계와 전체 사원 수
                               52125         12 ->  전체 급여 합계와 전체 사원 수
 

-- 위와 같이ROLLUP은 일반적인 누적에 대한 총계를 구할 때 아주 편리하게 사용 할 수 있다.
