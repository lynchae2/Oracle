# Synonym



#### 사용하는 이유

1. 데이터베이스의 투명성을 제공하기 위해서 사용한다.
2. 다른 유저의 객체를 참조할 때 많이 사용한다.
3. 만약 다른 유저의 객체를 참조할 경우가 있을 때 시노님을 생성해서 사용하면 추후에 참조하고 있는 오브젝트가 이름을 바꾸거나 이동할 경우 객체를 사용하는 SQL문을 모두 다시 고치는 것이 아니라 시노님만 다시 정의하면 되기 때문에 편리하다.
4. 객체를 참조하는 사용자의 오브젝트를 감출 수 있어 보안을 유지할 수 있다. 
5. 시노님을 사용하는 유저는 참조하고 있는 객체들에 대한 소유자, 이름, 서버 이름을 모르고 시노님 이름만 알아도 사용할 수 있다.







#### 사용하는 경우

1. 객체의 실제 이름과 소유자, 그리고 위치를 감춤으로써 DB의 보안을 개선하는데 사용한다.
2. Public Access를 제공한다.

3. Remote DB의 TABLE, VIEW, PROGRAM UNIT 을 위해 투명성을 제공한다.
4. DB 사용자를 위해 SQL문을 단순화 할 수 있다.





#### 종류

- Private synonym : 특정 사용자만 이용
- Public synonym : 공용 사용자 그룹이 소유하며 그 DB에 있는 모든 사용자가 공유한다.







#### 예제

- scott user의 emp 테이블에 test user가 접근하여 사용하는 예제

```sql
-- 1. scott user로 접속하여 test user 에게 emp 테이블을 조작할 권한을 부여한다.
GRANT ALL ON emp TO test;

-- 2. test user로 접속하여 synonym을 생성한다.
CONNECT test/test;
CREATE SYNONYM scott_emp FOR scott.emp;

-- scott user가 소유하고 있는 emp 테이블에 대해 scott_emp 라는 synonym을 생성했다.
-- scott user의 emp 테이블을 test user가 scott_emp라는 synonym으로 접근한다.

-- 3. synonym을 이용한 쿼리
SELECT empno, name FROM scott_emp;

-- 일반 테이블을 쿼리
SELECT empno, name FROM scott.emp; 

-- 두 쿼리의 결과는 같다.

-- 4. synonym 삭제
DROP SYNONYM scott_emp;

```

