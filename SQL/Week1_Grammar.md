# 개념정리

## `select`
  + table에서 호출할 columns을 조회 때 사용
```sql
SELECT 컬럼1, 컬럼2
FROM 테이블명
```

## `orderby`
  + ORDER BY 정렬할 컬럼 ASC  → 오름차순
  + ORDER BY 정렬할 컬럼 DESC → 내림차순
```sql
SELECT 컬럼1, 컬럼2
FROM 테이블명
ORDER BY 컬럼1 ASC 
-- ORDER BY 컬럼1 DESC
```

## `select distinct`
  + 고유값(Uniqu) 값을 호출할 때 사용
```sql
SELECT DISTINCT 컬럼1
FROM 테이블명
```

## `where`
  + 필요한 정보를 추출하기 위해 조건을 사용할 떄
  + =, >, >=, <, <=, !=
  + AND, OR 연산자로 다중 조건 사용 가능
```sql
SELECT *
FROM price
WHERE price > 3000
-- 가격이 3000 이상인 전체 행 조회
```

## `Limit`
  + 출력 행 갯수 지정(제한)
```sql
SELECT 컬럼1, 컬럼2
FROM 테이블명
LIMIT 10
-- 10개 행만 출력
```

## `Fetch`
  + OFFEST(시작점)부터 FETCH에서의 숫자만큼 행을 제한(갯수 지정)하여 조회
```sql
SELECT address_id , district 
FROM address a 
ORDER BY district
OFFSET 5 ROWS 
FETCH FIRST 20 ROW ONLY 
-- 시작점 6번째 행 부터 20개 행 조회
```

## `In`
  + column에 포함되어 있는 단어만을 조회 시 사용
```sql
SELECT customer_id , first_name , last_name 
FROM customer c 
WHERE first_name IN ('Maria', 'Lisa', 'Mike')
-- 이름이 'Maria', 'Lisa', 'Mike'인 정보들 중 id, 이름, 성 조회
```

## `Between`
  + 범위 조회 시 사용
```sql
SELECT *
FROM address a 
WHERE city_id BETWEEN 300 AND 310
ORDER BY city_id ASC 
-- city_id가 300 ~ 310인 행 조회 
-- WHERE city_id >= 300 AND city_id <= 310 와 동일
```

## `Like`
  + 특정 패턴으로 검색(조회)시 사용
  + 정규표현식과 유사함
```sql
SELECT *
FROM table
WHERE col1 LIKE 패턴
-- LIKE 'A%' : A로 시작하는 단어 조회 
-- LIKE '%a' : a로 끝나는 단어 조회
-- LIKE '%a%' : a가 들어가는 단어 조회
```

## `Isnull`
  + Null 값 조회 시 사용
```sql
SELECT *
FROM table
WHERE col1 IS NULL
-- WHERE col1 IS NOT NULL : Null 값이 아닌 값 조회
```

