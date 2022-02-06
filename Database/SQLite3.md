# SQLite3로 가볍게 배우는 데이터베이스: SQL 기초 실습

## 용어 정리

- 스키마(column 정보인거같에..) : 데이터베이스 구조(개체, 속성, 관계)와 제약 조건에 관한 전반적인 명세를 기술한 메타데이터의 집합

  1. 개념 스키마 = 전체적인 뷰 : 조직체 전체를 관리하는 입장에서 DB를 정의한 것.
  2. 내부 스키마 : 물리적인 저장장치 입장에서 DB가 저장되는 방법을 기술한 것.
  3. 외부 스키마 = 서브 스키마 - 사용자 뷰 : 사용자나 응용 프로그래머가 개인의 입장에서 필요한 데이터베이스의 논리적 구조를 정의

- [MSSQL] 테이블 스키마 컬럼 정보 조회하기

  ```sql
  select * from information_schema.columns
  where table_name = 'table_name' -- table_name : 'tableName'
  ```

  

## query 연습

### 추가, 삭제, 갱신, 조회

- **INSERT**
  1. `INSERT INTO Person(ID, Name, Birthday) VALUES(1, '이혜리', '1994-06-05');`
- **DELETE**
  
  1. `DELETE FROM Person;`
  
- **SELECT**
  
  1. `SELECT * FROM Person` : table 전체 조회
  2. `SELECT Name FROM Person` : Name 컬럼만 조회
  
- **ORDER BY**

  1. `SELECT Name FROM Person ORDER BY Name;` : Name 컬럼을 조회하되, name을 기준으로 오름차순 정렬
  2. `SELECT Name FROM Person ORDER BY Name DESC;` : Name 컬럼을 조회하되, name을 기준으로 오름차순 역순 정렬

  3. `ORDER BY` 문으로 순서를 변경한다고 table에 데이터의 순서가 바뀌진 않는다.

- **WHERE**

  1. `SELECT * FROM Person WHERE Name = '김용현'` : 이름이 '김용현'인 행을 조회

  2. ```sqlite
     SELECT * FROM Person
     WHERE Birthday IS NOT NULL
     ```

     `Birthday`가 `NULL`이 아닌 행을 조회

- **UPDATE**

  1. `UPDATE Person SET Name = '정유아' WHERE Name = '유아';` : Name행에서 '유아'를 '정유아'로 변경
  2. `UPDATE Person SET Name = '유아' WHERE ID = 7;` : ID 7번인 행에서 Name을 '유아'로 변경

- **LIKE**

  1. `SELECT * FROM Person WHERE Birthday LIKE '1992%'` : LIKE 이하의 패턴과 일치하는 문자열의 행을 찾아준다.

- **연습문제**

  1. `Person` 테이블을 나이가 많은 사람이 먼저 오도록 정렬해서 조회해보라. 생일을 알 수 없는 사람은 제외한다.

     ```sqlite
     SELECT * FROM Person
     WHERE Birthday IS NOT NULL ORDER BY Birthday;
     ```

  2. `Person` 테이블에서 `Name`이 `김`으로 시작하는 행을 찾아서, 그 값을 `유라`로 바꾸어 보라.

     ```sqlite
     UPDATE Person SET Name = '유라' WHERE Name LIKE '김%'
     ```

  3. 연습문제 3.1에서 만든 `pets` 테이블에 다음과 같이 데이터를 넣어라.

     ```sqlite
     INSERT INTO Pets (Name, Animal)
     VALUES ('Dr.Harris Bonkers', 'Rabbit'), ('Moon', 'Dog'), ('Ripley', 'Cat'), ('Tom', 'Cat');
     ```



### 테이블 변경

- **컬럼 추가**

  1. `ALTER TABLE Person ADD COLUMN New INTEGER;` : Person 테이블에 New라는 이름의 컬럼을 추가
  2. `UPDATE Person SET New = 175 WHERE Name = '용현';` : '용현' 행의 New 컬럼에 175값 추가

  - 행이 이미 있고 값이 비어있는 컬럼에 값을 추가 할 때는 `INSERT INTO`가 아니고 `UPDATE`문 사용.

- 컬럼명 변경

  1. `ALTER TABLE Person RENAME COLUMN New To Height;` : 컬럼 이름 변경



### 테이블  드롭하기

1. `DROP TABLE Person;` : 명령을 실행하면 데이터베이스 스키마와 디스크 파일에서 테이블이 삭제된다.

### 테이블  생성하기

```sqlite
-- 테이블 생성
CREATE TABLE "Person" (
	"ID"	INTEGER NOT NULL,
	"Name"	TEXT NOT NULL,
	"Birthday"	TEXT,
	"Height"	INTEGER,
	"Weight"	INTEGER,
	PRIMARY KEY("ID" AUTOINCREMENT)
);
```

```sqlite
-- 행 추가
INSERT INTO Person VALUES
    (1, '혜리', '1994-06-09', NULL, 50),
    (2, '소진', '1986-05-21', 167, NULL),
    (3, '유라', '1992-11-06', 170.3, 54),
    (4, '민아', NULL, 164, 46);
```



### 컬럼 별명, 뷰

1. `SELECT`할 때 컬럼명을 다른 이름으로 바꿔서 조회 할 수 있다.

   ```sqlite
   SELECT
   	-- AS 생략 가능
   	Name AS "이름",
   	Birthday AS "생일"
   FROM Person;
   ```

- **뷰(View) 생성**

  1. 뷰는 `SELECT`문을 미리 만들어서 이름을 붙여둔 것이라 할 수 있다.

  ```sqlite
  CREATE VIEW Birthday
  AS
  SELECT
  	Name,
  	-- 뒤에 붙은 단어는 새로운 컬럼 생성(bdate, YYYY, MM, DD)
  	Birthday bdate,
  	substr(Birthday, 1, 4) YYYY,	-- 첫째 자리부터 네 글자
  	substr(Birthday, 6, 2) MM,		-- 여섯째 자리부터 두 글자
  	substr(Birthday, 9, 2) DD
  FROM Person;
  ```

  2. `substr()` : 함수는 문자열의 일부를 반환한다.

  3. 뷰 삭제 : `DROP VIEW Birthday;`



### 조건절

```sqlite
SELECT
	bdate,
	MM,
	CASE
		WHEN MM = '01' THEN 'Jan.'
	END Month
FROM Birthday;
```

- `MM` 컬럼에서 조건에 맞는 값을 `THEN` 뒤의 값으로 변경한 값을 생성된 `Month` 컬럼에 추가해준다.



### 날짜와 시간

- **현재 시간** : `SELECT strftime('%Y-%m-%d %H:%M:%S', 'now', 'localtime') 현재시간;`

- **나이 구하기**

  1. 혜리의 만 나이 구하기

     ```sqlite
     SELECT
     	-- Birthday에 '생일' 컬럼에서
     	Birthday "생일",
     	-- 현재 년도에서 - 첫째 자리부터 네글자 - (현재 월일 < 생일 월일)
     	-- 생일이 지났으면 1을 안 빼주고, 안 지났으면 1을 빼준다.
     	strftime('%Y', 'now') - substr(Birthday, 1, 4) - (strftime('%m-%d', 'now') < substr(Birthday, 6)) "나이"
     FROM Person
     WHERE Name = '혜리';
     ```

- **리터럴 값**

  - `SELECT CURRENT_DATE, CURRENT_TIME, CURRENT_TIMESTAMP;`

- **연습문제**

  ```sqlite
  SELECT
  	Name,
  	strftime('%Y', 'now') - substr(Birthday, 1, 4) - (strftime('%m-%d', 'now') < substr(Birthday, 6)) "Age"
  FROM Person
  ```



### 집계 함수

1. `count(*)` : 선택한 table의 모든 행의 개수를 센다.
   - `count(Height)` :  특정 컬럼의 행의 개수
2. `max()` : 인자로 컬럼을 넣어주면 컬럼의 최댓값을 구한다.
3. `min()` : 인자로 컬럼을 넣어주면 컬럼의 최솟값을 구한다.
4. `sum()` : 인자로 컬럼을 넣어주면 컬럼 전체 합계를 구한다.
5. `avg()` : 인자로 컬럼을 넣어주면 컬럼의 평균값을 구한다.

- **연습문제**
  1. `NULL`값은 정해지지 않은 값이니까 빼고 계산 했을 듯
  2. 혜리의 키는 166.8
     - 추가 코드 : `UPDATE Person SET Height = 166.8 WHERE Name = '혜리';`
     - 평균 계산 : `SELECT avg(Height) FROM Person;`
  3. `pets` 테이블에서 고양이가 몇 마리 인지 세어보라.
     - `SELECT count(Animal) FROM pets WHERE Animal = "Cat";`



### 그룹화

- **GROUP BY**

  키를 반올림한 값이 같은 사람 수를 세어보자.

  ```sqlite
  SELECT round(Height), count(*)
  FROM Person
  -- 1은 첫번째 컬럼을 가리킨다.
  GROUP BY 1;
  ```

- **HAVING**

  두 명 이상인 경우, 즉 `count(*)`가 `2`이상인 경우만 조회

  ```sqlite
  SELECT round(Height), count(*)
  FROM Person
  GROUP BY round(Height)
  HAVING count(*) > 1;
  ```

- **연습문제**

  1. `pets` 테이블에는 동물이 종류별로 몇 마리씩 있는지 세어보라.

     ```sqlite
     SELECT Animal, count(*)
     FROM pets
     GROUP BY 1;
     ```

  2. `pets` 테이블에서 한 마리가 넘게 있는 동물에는 어떤 것이 있는지 알아보라.

     ```sqlite
     SELECT Animal, count(*)
     FROM pets
     GROUP BY Animal
     HAVING count(*) > 1;
     ```

     

### 평균, 편차, 분산, 표준편차

- **평균**

  Person 테이블에서 Height의 개별 값과, 평균 컬럼을 생성하면 평균값을 보여준다.

  ```sqlite
  SELECT
  	Name "이름",
  	Height "키",
  	-- 집계 함수(avg())로 계산한 결과를 행에 나타내려고 over()구문을 사용
  	avg(Height) over() "평균"
  FROM Person;
  ```

- **편차** - 관측값에서 **평균 또는 중앙값을 뺀** 것이다.

  편차 컬럼을 추가하여 편차를 계산한다.

  ```sqlite
  SELECT
  	Name "이름",
  	Height "키",
  	avg(Height) over() "평균",
  	round(Height - avg(Height) over(), 3) "편차"
  FROM Person;
  ```

- **편차 합계**

  ```sqlite
  SELECT sum(편차)
  -- Person table에서 편차 컬럼만 SELECT
  FROM (
  	SELECT
  		round(Height - avg(Height) over(), 3) "편차"
      FROM Person
  )
  ```

- **분산** - 편차 제곱의 평균

  ```sqlite
  SELECT avg(편차*편차) "분산"
  FROM (
  	SELECT
  		round(Height - avg(Height) over(), 3) "편차"
  	FROM Person
  )
  ```



### 조인

- **노래 테이블 생성**

  ```sqlite
  CREATE TABLE 노래 (
  	ID INTEGER NOT NULL PRIMARY KEY,
  	제목 TEXT NOT NULL
  );
  ```

- **음반 데이블 생성**

  ```sqlite
  CREATE TABLE 음반 (
  	ID INTEGER NOT NULL PRIMARY KEY,
  	제목 TEXT NOT NULL,
  	연도 INTEGER
  );
  ```

- **수록곡 테이블 생성**(음반과 노래의 관계를 표현)

  ```sqlite
  CREATE TABLE 수록곡 (
  	음반ID INTEGER NOT NULL,
  	노래ID INTEGER NOT NULL
  );
  ```

- **각 테이블에 데이터 삽입**

  ```sqlite
  INSERT INTO 노래 VALUES
  (1, '갸우뚱'),
  (2, 'Shuppy Shuppy'),
  (3, 'Control'),
  (4, '영러브'),
  (5, '한번만 안아줘'),
  (6, '반짝반짝'),
  (7, '기대해'),
  (8, 'I Don''t Mind'),
  (9, 'Easy go'),
  (10, '여자대통령');
  
  INSERT INTO 음반 VALUES
  (1, 'Girl''s Day Party #1', 2010),
  (2, 'Everyday', 2011),
  (3, 'Expectation', 2013),
  (4, '여자대통령', 2013);
  
  INSERT INTO 수록곡 VALUES 
  (1, 1),
  (1, 2),
  (1, 3),
  (2, 4),
  (2, 5),
  (2, 6),  -- Everyday - 반짝반짝
  (3, 7),
  (3, 8),
  (3, 9),
  (3, 6),  -- Expectation - 반짝반짝
  (3, 5),
  (4, 10);
  ```

- **데이터 조회**

  ```sqlite
  -- 각 테이블에서 컬럼을 조회해 새로운 컬럼명으로 변경
  SELECT 음반.제목 앨범명, 음반.연도 발매년도, 노래.제목 곡명
  FROM 수록곡
  -- 음반 테이블과 수록곡의 음반ID 컬럼이랑 조인을 음반 테이블의 ID값과
  INNER JOIN 음반 ON 수록곡.음반ID = 음반.ID
  INNER JOIN 노래 ON 수록곡.노래ID = 노래.ID
  
  
  -- WHERE절로도 가능
  SELECT 음반.제목 앨범명, 음반.연도 발매년도, 노래.제목 곡명
  FROM 노래, 음반, 수록곡
  -- ID 값이 같은 값끼리 조회
  WHERE 음반.ID = 수록곡.음반ID AND 노래.ID = 수록곡.노래ID;
  ```



### UNION ALL과 UNION

- **UNION ALL**

  서로 다른 테이블의 같은 컬럼을 한꺼번에 조회 가능(컬럼명이 달라도 조회가능)

  ```sqlite
  SELECT 제목 FROM 음반
  UNION ALL
  SELECT 제목 FROM 노래;
  ```

- **UNION**

  UNION ALL과 비슷하지만 UNION은 **중복값을 제거**

  중복값을 제거할 때 정렬을 수행한다.(메모리 사용)

- RDBMS는 테이블 연산에 최적화 되어 있고 조건절은 부가적인 기능 같다. 그러므로 테이블 간의 조인으로 계산을 할 수 있는 경우에는 조건절 보다 조인으로 처리하는 것이 낫다.



### 이미지

- **BLOB**(Binary Large Object)
