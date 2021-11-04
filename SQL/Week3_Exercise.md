# 문제풀이

## No.6
* 문제1번) 매출을 가장 많이 올린 dvd 고객 이름은? (subquery 활용)
```sql
SELECT first_name 
FROM customer c 
WHERE c.customer_id IN (SELECT p.customer_id
    FROM payment p
    GROUP BY p.customer_id
    ORDER BY sum(amount) DESC 
    LIMIT 1)
```
* 문제2번) 대여가 한번도이라도 된 영화 카테 고리 이름을 알려주세요. (쿼리는, Exists조건을 이용하여 풀어봅시다)
```sql
SELECT c."name" 
FROM category c
WHERE EXISTS (SELECT fc.category_id 
    FROM rental r 
    JOIN inventory i ON r.inventory_id = i.inventory_id 
    JOIN film_category fc ON i.film_id = fc.film_id
    WHERE fc.category_id = c.category_id)
```
* 문제3번) 대여가 한번도이라도 된 영화 카테 고리 이름을 알려주세요. (쿼리는, Any 조건을 이용하여 풀어봅시다)
```sql
SELECT c."name" 
FROM category c 
WHERE c.category_id = ANY (SELECT fc.category_id 
     FROM rental r 
     JOIN inventory i ON r.inventory_id = i.inventory_id 
     JOIN film_category fc ON i.film_id = fc.film_id
     WHERE fc.category_id = c.category_id)
```
* 문제4번) 대여가 가장 많이 진행된 카테고리는 무엇인가요? (Any, All 조건 중 하나를 사용하여 풀어봅시다)
```sql
SELECT c."name" 
FROM category c 
WHERE c.category_id = ANY (SELECT fc.category_id 
     FROM rental r 
     JOIN inventory i ON r.inventory_id = i.inventory_id 
     JOIN film_category fc ON i.film_id = fc.film_id
     GROUP BY fc.category_id
     ORDER BY count(DISTINCT r.rental_id) DESC 
     LIMIT 1)
```
* 문제5번) dvd 대여를 제일 많이한 고객 이름은? (subquery 활용)
```sql
SELECT first_name 
FROM customer c 
WHERE c.customer_id IN (SELECT p.customer_id
      FROM payment p
      GROUP BY p.customer_id
      ORDER BY count(p.rental_id) DESC 
      LIMIT 1)
```
* 문제6번) 영화 카테고리값이 존재하지 않는 영화가 있나요?
```sql
SELECT *
FROM film f 
WHERE NOT EXISTS (SELECT
      FROM film_category fc
      WHERE fc.film_id = f.film_id)
```
## No.7
* 문제1번) 대여점(store)별 영화 재고(inventory) 수량과 전체 영화 재고 수량은? (grouping set)
```sql
SELECT store_id , count(*) 
FROM inventory i 
GROUP BY  
GROUPING SETS ((store_id),())
ORDER BY store_id
```
* 문제2번) 대여점(store)별 영화 재고(inventory) 수량과 전체 영화 재고 수량은? (rollup)
```sql
SELECT store_id , count(*)
FROM inventory i 
GROUP BY 
ROLLUP (store_id)
ORDER BY store_id 
```
* 문제3번) 국가(country)별 도시(city)별 매출액, 국가(country)매출액 소계 그리고 전체 매출액을 구하세요. (grouping set)
```sql
SELECT c3.country , c2.city , sum(p.amount) 
FROM payment p 
JOIN customer c ON c.customer_id = p.customer_id 
JOIN address a ON a.address_id = c.address_id 
JOIN city c2 ON c2.city_id = a.city_id 
JOIN country c3 ON c3.country_id = c2.country_id
GROUP BY 
GROUPING SETS ((c3.country, c2.city), (c3.country), ())
ORDER BY c3.country , c2.city 

```
* 문제4번) 국가(country)별 도시(city)별 매출액, 국가(country)매출액 소계 그리고 전체 매출액을 구하세요. (rollup)
```sql
SELECT c3.country , c2.city , sum(p.amount) 
FROM payment p 
JOIN customer c ON c.customer_id = p.customer_id 
JOIN address a ON a.address_id = c.address_id 
JOIN city c2 ON c2.city_id = a.city_id 
JOIN country c3 ON c3.country_id = c2.country_id
GROUP BY 
ROLLUP (c3.country, c2.city)
ORDER BY c3.country , c2.city
```
* 문제5번) 영화배우별로  출연한 영화 count 수 와,   모든 배우의 전체 출연 영화 수를 합산 해서 함께 보여주세요.
```sql
SELECT actor_id , count(*)
FROM film_actor fa 
GROUP BY
ROLLUP (actor_id)
ORDER BY actor_id 
```
* 문제6번) 국가 (Country)별, 도시(City)별  고객의 수와 ,  전체 국가별 고객의 수를 함께 보여주세요. (grouping sets)
```sql
SELECT c3.country , c2.city , count(DISTINCT c.customer_id) 
FROM payment p 
JOIN customer c ON c.customer_id = p.customer_id 
JOIN address a ON a.address_id = c.address_id 
JOIN city c2 ON c2.city_id = a.city_id 
JOIN country c3 ON c3.country_id = c2.country_id
GROUP BY 
GROUPING SETS ((c3.country, c2.city), (c3.country), ())
ORDER BY c3.country , c2.city 
```
* 문제7번) 영화에서 사용한 언어와  영화 개봉 연도 에 대한 영화  갯수와  , 영화 개봉 연도에 대한 영화 갯수를 함께 보여주세요.
```sql
SELECT language_id , release_year, count(*) 
FROM film f 
GROUP BY
GROUPING SETS ((language_id, release_year), (release_year))
```
* 문제8번) 연도별, 일별 결제  수량과,  연도별 결제 수량을 함께 보여주세요.
```
결제수량은 결제 의 id 갯수 를 의미합니다.
```
```sql
SELECT EXTRACT (YEAR FROM payment_date) , date(payment_date) , count(payment_id) 
FROM payment p 
GROUP BY 
ROLLUP ((EXTRACT (YEAR FROM payment_date), date(payment_date)), (date(payment_date)))
ORDER BY date(payment_date)

SELECT EXTRACT (YEAR FROM payment_date) , date(payment_date) , count(payment_id) 
FROM payment p 
GROUP BY 
GROUPING SETS ((EXTRACT (YEAR FROM payment_date), date(payment_date)), (date(payment_date)))
ORDER BY date(payment_date)
```
* 문제9번) 지점 별,  active 고객의 수와 ,   active 고객 수 를  함께 보여주세요.
```
지점과, active 여부에 대해서는 customer 테이블을 이용하여 보여주세요.
grouping sets 를 이용해서 풀이해주세요.
```
```sql
SELECT store_id , active , count(customer_id)
FROM customer c 
GROUP BY
GROUPING SETS ((store_id, active), (active))
ORDER BY store_id 
```
* 문제10번) 지점 별,  active 고객의 수와 ,   active 고객 수 를  함께 보여주세요.
```
지점과, active 여부에 대해서는 customer 테이블을 이용하여 보여주세요.
roll up으로 풀이해보면서, grouping sets 과의 차이를 확인해보세요.
```
```sql
SELECT store_id , active , count(customer_id)
FROM customer c 
GROUP BY
ROLLUP ((store_id, active))
ORDER BY store_id 
```

## No.8
* 문제1번) dvd 대여를 제일 많이한 고객 이름은? (analytic funtion 활용)

* 문제2번) 매출을 가장 많이 올린 dvd 고객 이름은? (analytic funtion 활용)

* 문제3번) dvd 대여가 가장 적은 도시는? (anlytic funtion)

* 문제4번) 매출이 가장 안나오는 도시는? (anlytic funtion)

* 문제5번) 월별 매출액을 구하고 이전 월보다 매출액이 줄어든 월을 구하세요. (일자는 payment_date 기준)

* 문제6번) 도시별 dvd 대여 매출 순위를 구하세요.

* 문제7번) 대여점별 매출 순위를 구하세요.

* 문제8번) 나라별로 가장 대여를 많이한 고객 TOP 5를 구하세요.

* 문제9번) 영화 카테고리 (Category) 별로 대여가 가장 많이 된 영화 TOP 5를 구하세요

* 문제10번) 매출이 가장 많은 영화 카테고리와 매출이 가장 작은 영화 카테고리를 구하세요. (first_value, last_value)

## No.9
*  문제1번) dvd 대여를 제일 많이한 고객 이름은?   (with 문 활용)

*  문제2번) 영화 시간 유형 (length_type)에 대한 영화 수를 구하세요.
```
영화 상영 시간 유형의 정의는 다음과 같습니다.
영화 길이 (length) 은 60분 이하 short , 61분 이상 120분 이하 middle , 121 분이상 long 으로 한다.
```

*  문제3번) 약어로 표현되어 있는 영화등급(rating) 을 영문명, 한글명과 같이 표현해 주세요. (with 문 활용)
```
G        ? General Audiences (모든 연령대 시청가능)
PG      ? Parental Guidance Suggested. (모든 연령대 시청가능하나, 부모의 지도가 필요)
PG-13 ? Parents Strongly Cautioned (13세 미만의 아동에게 부적절 할 수 있으며, 부모의 주의를 요함)
R         ? Restricted. (17세 또는 그이상의 성인)
NC-17 ? No One 17 and Under Admitted.  (17세 이하 시청 불가)
```

*  문제4번) 고객 등급별 고객 수를 구하세요. (대여 횟수에 따라 고객 등급을 나누고 조건은 아래와 같습니다.)
```
A 등급은 31회 이상
B 등급은 21회 이상 30회 이하
C 등급은 11회 이상 20회 이하
D 등급은 10회 이하
```

*  문제5번) 고객 이름 별로 , flag  를 붙여서 보여주세요.
```
고객의 first_name 이름의 첫번째 글자가, A, B,C 에 해당 하는 사람은 각 A,B,C 로 flag 를 붙여주시고
A,B,C 에 해당하지 않는 인원에 대해서는 Others 라는 flag 로 붙여주세요.
```

*  문제6번) payment 테이블을 기준으로,  2007년 1월~ 3월 까지의 결제일에 해당하며,  staff2번 인원에게 결제를 진행한  결제건에 대해서는, Y 로그 외에 대해서는 N 으로 표기해주세요. with 절을 이용해주세요.

*  문제7번) Payement 테이블을 기준으로, 결제에 대한 Quarter 분기를 함께 표기해주세요.
```
- with 절을 활용해서 풀이해주세요.
1~월의 경우 Q1
4~6월 의 경우 Q2
7~9월의 경우 Q3
10~12월의 경우 Q4
```

*  문제8번) Rental 테이블을 기준으로,  회수일자에 대한 Quater 분기를 함께 표기해주세요.
```
- with 절을 활용해서 풀이해주세요.
1~월의 경우 Q1
4~6월 의 경우 Q2
7~9월의 경우 Q3
10~12월의 경우 Q4 로 함께 보여주세요.
```

*  문제9번) 직원이이  월별  대여를 진행 한  대여 갯수가 어떻게 되는 지 알려주세요.
```
- 대여 수량이   아래에 해당 하는 경우에 대해서, 각 flag 를 알려주세요 .
0~ 500개 의 경우  under_500
501~ 3000 개의 경우  under_3000
3001 ~ 99999 개의 경우  over_3001
```

*  문제10번) 직원의 현재 패스워드에 대해서, 새로이  패스워드를 지정하려고 합니다.
```
직원1의 새로운 패스워드는 12345  ,  직원2의 새로운 패스워드는 54321입니다.
해당의 경우, 직원별로 과거 패스워드와 현재 새로이 업데이트할 패스워드를 함께 보여주세요.
with 절을 활용하여  새로운 패스워드 정보를 저장 후 , 알려주세요.
```
