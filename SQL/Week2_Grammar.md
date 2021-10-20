# 개념정리

## Join

### Inner Join
* table1, table2에서 같은 행만 조회
```sql
SELECT c.first_name , c.last_name , c.email , a.phone 
FROM customer c 
JOIN address a ON c.address_id = a.address_id 
JOIN city c2 ON a.city_id = c2.city_id 
WHERE c2.city = 'Lima'
-- customer, address table에서 address_id 가 같은 행
-- address, city table에서 city_id 가 같은 행
```

### Outer Join
* Left (Outer) Join : 왼쪽에 있는 table1의 모든 결과를 가져온 후, table2와 매칭하여 매칭이 되는 데이터가 없을 경우 Null 값 처리
* Right (Outer) Join : 오른쪽에 있는 table2의 모든 결과를 가져온 후, table1와 매칭하여 매칭이 되는 데이터가 없을 경우 Null 값 처리
```sql
SELECT f.film_id , f.title, f.release_year , f.rental_rate , f.length 
FROM film f
LEFT JOIN film_actor fa ON f.film_id = fa.film_id 
LEFT JOIN actor a ON fa.actor_id = a.actor_id
WHERE a.actor_id IS NULL 
-- film, film_actor table에서 film_id 가 같은 행
-- actor, film_actor table에서 actor_id 가 같은 행
-- table join을 통해 매칭되는 데이터가 없는 행만 추출하기 위해 "WHERE a.actor_id IS NULL" 사용
```

### Self Join
* 자기 자신을 참조하는 Join
* table에 한 column이 자신에게 있는 다른 레코드를 참조할 경우 사용

### Full Join
* table1, table2 매칭하여 두 table의 모든 값을 출력 (Left Join + Right Join)
```sql
SELECT a.address_id , s.first_name , s.last_name , s.picture 
FROM address a 
FULL JOIN staff s ON a.address_id = s.address_id 
```
* table1, table2 매칭하여 두 table에 공통된 부분을 제외한 나머지 부분을 출력
```sql
SELECT a.address_id , s.first_name , s.last_name , s.picture 
FROM address a 
FULL JOIN staff s ON a.address_id = s.address_id 
WHERE a.address_id IS NULL OR s.picture IS NULL 
```
* only table1 에만 존제하는 값 출력 - 추가적으로 공부
```sql

```
* only table2 에만 존제하는 값 출력 - 추가적으로 공부
```sql

```

### Cross Join
* table1, table2 모든행을 Join함, 두 table의 행의 개수를 곱한 것과 같음
* 데이터 복제에 많이 
```sql
SELECT *
FROM staff s 
CROSS JOIN category c 
-- staff 2개 행, category 16개 행
-- staff 2개 행을 16번 반복하여 32개 행으로 만들어 줌
```

### Natural Join
* table간 중복된 column이 존재한다면 한개만 표시하는 Join
* 스스로 중복을 걸러내는 것이 아니라 직접 걸러내야함 (Inner Join과 유사, 실무에서 사용빈도 적음)

## Group By, Having
* Group By : 조회할 컬럼의 값을 그룹화 후 조회
* Having : Group By와 함꼐 사용되며 그룹화 한 결과에 조건을 추가하여 조회

* 기본
```sql
SELECT 컬럼1
FROM 테이블
GROUP BY 그룹화 할 컬럼
```
* 선 조건 후 그룹화
```sql
SELECT 컬럼1
FROM 테이블
WHERE 조건
GROUP BY 그룹화 할 컬럼
```
* 선 그룹화 후 조건
```sql
SELECT 컬럼1
FROM 테이블
GROUP BY 그룹화 할 컬럼
HAVING 조건
```

## 집합연산자와 서브쿼리

### Union, Union All

### Intersect

### Except
