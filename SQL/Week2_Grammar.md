# 개념정리

## Join

### Inner Join
* table1, table2에서 같은 행만 조회

### Outer Join
* LEFT Join : 왼쪽에 있는 table1의 모든 결과를 가져온 후, table2와 매칭하여 매칭이 되는 데이터가 없을 경우 Null 값 처리
* Right Join : 오른쪽에 있는 table2의 모든 결과를 가져온 후, table1와 매칭하여 매칭이 되는 데이터가 없을 경우 Null 값 처리

### Self Join
* 자기 자신을 참조하는 Join
* table에 한 column이 자신에게 있는 다른 레코드를 참조할 경우 사용

### Full Join
* table1, table2 매칭하여 없는 데이터는 Null 값 처리

### Cross Join
* table1, table2 모든행을 Join함, 두 table의 행의 개수를 곱한 것과 같음

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
