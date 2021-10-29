# 개념정리

## 집합연산자와 서브쿼리
* ANY, ALL, EXISTS

### `ANY`, `ALL`, `EXISTS`
* 공통 : 서브쿼리의 결과에 반환되는 값과 비교
* `ANY` : 서브쿼리의 결과가 어떠한 조건이라도 만족한다면 성립
* `ALL` : 서브쿼리의 모든 결과가 만족해야 성립
* `EXISTS`: 서브쿼리 내 결과에 존재여부를 판단함

```sql

```
```sql

```
```sql

```

## 조인과 집계데이터
* GROUPING SET, ROLL UP, CUBE, AVG, ROW NUMBER, RANK, DENSE_RANK, FIRST_VALUE, LAST_VALUE, LAG, LEAD, 분석함수

### `GROUPING SET`
* 여러개의 UNION ALL 문법을 활용한 쿼리와 같은 결과 도출

### `ROLL UP`
* GROUPING 된 column의 소계를 생성

### `CUBE`
* GROUPING 된 column의 다차원 소계를 생성

## 분석함수
* 테이블에 있는 데이터를 특정용도로 분석하여 결과를 반환
### `AVG`
* 평균

### `ROW NUMBER`, `RANK`, `DENSE_RANK`
* 순위 지정
* `ROW NUMBER` : 순차적 순위 지정 (동일한 스코어가 존재하더라도 순차적으로 순위 지정)
* `RANK` : 동일한 스코어가 있을 시 그 다음 순위로 건너 뛰어 순위 지정
* `DENSE_RANK` : 동일한 스코어가 있을 시 그 다음 순위로 건너 뛰지 않음

## `WINDOW FUNCTION`
* 테이블에서 행집합을 대상으로 하는 함수
* 함수 + OVER(윈도우 함수 지정)
* `ROW NUMBER`, `RANK`, `DENSE_RANK`도 `WINDOW FUNCTION`로 사용 가능
### `FIRST_VALUE`, `LAST_VALUE`
* `FIRST_VALUE` : 첫번째 레코드 추출
* `LAST_VALUE` : 마지막 레코드 추출

### `LAG`, `LEAD`, 
* 그룹별로 행 내리기(LAG), 행 올리기(LEAD)

## CTE
* Common Table Expression : 공통 테이블 표현식
### `WITH`
* 더 큰 쿼리문에 사용할 보조문을 작성하는 방법 제공
* `WITH`절의 보조 명령문은 `SELECT`, `INSERT`, `UPDATE`, `DELETE`

### `PARTITION BY`
* 그룹을 묶어주는 역할
