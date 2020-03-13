#  GROUPING 과 GROUPING_ID



#### GROUPING (컬럼명)

- ROLLUP, CUBE 에 모두 사용할 수 있다.

- 해당 컬럼의 row가 GROUP BY에 의해서 산출된 row인 경우에는 0을 반환하고,

  ROLLUP이나 CUBE에 의해서 산출된 row인 경우에는 1을 반환한다.

- 따라서, 해당 row가 결과 집합에 의해 산출된 data 인지, ROLLUP이나 CUBE에 의해서 산출된 data 인지 알 수 있도록 지원하는 함수이다.









#### GROUPING_ID(컬럼 1, 컬럼 2, ... )

- GROUPING(컬럼 1) || GROUPING(컬럼 2) 의 값을 2진수에서 10진수로 변환한 값

  

```SQL
SELECT deptno
     , empno
     , SUM(sal) s_sal
     , GROUPING(deptno) grp_dept
     , GROUPING(empno)  grp_emp
     , GROUPING_ID(deptno, empno) gid
FROM emp
GROUP BY ROLLUP(deptno, empno);
 
 
-- GRP_DEPT : deptno로 GROUP BY 되면 0, deptno의 값이 없으면 1
-- GRP_EMP : empno로 GROUP BY 되면 0, empno의 값이 없으면 1
-- GID : GROUPING(deptno) || GROUPING(empno)의 값을 2진수에서 10진수로 변환한 값
DEPTNO      EMPNO      S_SAL   GRP_DEPT    GRP_EMP        GID
------ ---------- ---------- ---------- ---------- ----------
    10       7782       2450          0          0          0
    10       7839       5000          0          0          0
    10       7934       1300          0          0          0
    10                  8750          0          1          1
    20       7369        800          0          0          0
    20       7566       2975          0          0          0
    20       7788       3000          0          0          0
    20       7876       1100          0          0          0
    20       7902       3000          0          0          0
    20                 10875          0          1          1
    30       7900        950          0          0          0
    30       7499       1600          0          0          0
    30       7521       1250          0          0          0
    30       7654       1250          0          0          0
    30       7698       2850          0          0          0
    30       7844       1500          0          0          0
    30                  9400          0          1          1
                       29025          1          1          3
```







#### 소계만 표시, 총계 제거

```sql
SELECT deptno
-- GROUPING(empno)가 1 이면(=ROLLUP 또는 CUBE로 산출된 행인 경우) 소계, 아니면 empno 출력
, DECODE(GROUPING(empno),1,'소계',empno) empno
, SUM(sal) s_sal
FROM emp
GROUP BY ROLLUP(deptno, empno)
-- 총계는 GROUPING(deptno)의 값이 없다 = 1이된다 = 총계가 나오지 않는다.
HAVING GROUPING(deptno) = 0; 

 DEPTNO EMPN      S_SAL
------- ---- ----------
     10 7782       2450
     10 7839       5000
     10 7934       1300
     10 소계        8750
     20 7369        800
     20 7566       2975
     20 7788       3000
     20 7876       1100
     20 7902       3000
     20 소계       10875
     30 7900        950
     30 7499       1600
     30 7521       1250
     30 7654       1250
     30 7698       2850
     30 7844       1500
     30 소계        9400
```







