# 개념정리

## Join

### Inner Join

### Outer Join

### Self Join

### Full Join

### Cross Join

### Natural Join

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
