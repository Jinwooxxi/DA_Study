# 문제풀이

## No.3

- 문제1번) 고객의 기본 정보인, 고객 id, 이름, 성, 이메일과 함께 고객의 주소 address, district, postal_code, phone 번호를 함께 보여주세요.
```sql
SELECT c.customer_id , c.first_name , c.last_name , c.email , a.address , a.district , a.postal_code , a.phone 
FROM customer c 
JOIN address a ON c.address_id = a.address_id 
```

- 문제2번) 고객의 기본 정보인, 고객 id, 이름, 성, 이메일과 함께 고객의 주소 address, district, postal_code, phone , city 를 함께 알려주세요.
```sql
SELECT c.customer_id , c.first_name , c.last_name , c.email , a.address , a.district , a.postal_code , a.phone , c2.city 
FROM customer c 
JOIN address a ON c.address_id = a.address_id 
JOIN city c2 ON a.city_id = c2.city_id 
```

- 문제3번) Lima City에 사는 고객의 이름과, 성, 이메일, phonenumber에 대해서 알려주세요.
```sql
SELECT c.first_name , c.last_name , c.email , a.phone 
FROM customer c 
JOIN address a ON c.address_id = a.address_id 
JOIN city c2 ON a.city_id = c2.city_id 
WHERE c2.city = 'Lima'
```

- 문제4번) rental 정보에 추가로, 고객의 이름과, 직원의 이름을 함께 보여주세요.
  - 고객의 이름, 직원 이름은 이름과 성을 fullname 컬럼으로만들어서 직원이름/고객이름 2개의 컬럼으로 확인해주세요.
```sql
SELECT r.*, c.first_name ||' '|| c.last_name AS customer_full_name , s.first_name ||' '|| s.last_name AS staff_full_name 
FROM rental r 
JOIN customer c ON r.customer_id = c.customer_id
JOIN staff s ON r.staff_id = s.staff_id  
```

- 문제5번) [seth.hannon@sakilacustomer.org](mailto:seth.hannon@sakilacustomer.org) 이메일 주소를 가진 고객의  주소 address, address2, postal_code, phone, city 주소를 알려주세요.
```sql
SELECT a.address , a.address2 , a.postal_code , a.phone , c2.city 
FROM customer c
JOIN address a ON c.address_id = a.address_id 
JOIN city c2 ON a.city_id = c2.city_id 
WHERE c.email = 'seth.hannon@sakilacustomer.org' 
```

- 문제6번) Jon Stephens 직원을 통해 dvd대여를 한 payment 기록 정보를  확인하려고 합니다.
  - payment_id,  고객 이름 과 성,  rental_id, amount, staff 이름과 성을 알려주세요.
```sql
SELECT p.payment_id , c.first_name , c.last_name , p.rental_id , p.amount , s.first_name , s.last_name 
FROM payment p 
JOIN customer c ON p.customer_id = c.customer_id 
JOIN staff s ON p.staff_id = s.staff_id 
WHERE s.first_name = 'Jon' AND s.last_name = 'Stephens'
```

- 문제7번) 배우가 출연하지 않는 영화의 film_id, title, release_year, rental_rate, length 를 알려주세요.
```sql
SELECT f.film_id , f.title, f.release_year , f.rental_rate , f.length 
FROM film f
LEFT JOIN film_actor fa ON f.film_id = fa.film_id 
LEFT JOIN actor a ON fa.actor_id = a.actor_id
WHERE a.actor_id IS NULL 
```

- 문제8번) store 상점 id별 주소 (address, address2, distict) 와 해당 상점이 위치한 city 주소를 알려주세요.
```sql
SELECT s.store_id , a.address, a.address2, a.district , c.city 
FROM store s 
JOIN address a ON s.address_id = a.address_id 
JOIN city c ON a.city_id = c.city_id 
```

- 문제9번) 고객의 id 별로 고객의 이름 (first_name, last_name), 이메일, 고객의 주소 (address, district), phone번호, city, country 를 알려주세요.
```sql
SELECT c.customer_id , c.first_name , c.last_name , c.email , a.address , a.district , a.phone , c2.city , c3.country 
FROM customer c 
JOIN address a ON c.address_id = a.address_id 
JOIN city c2 ON a.city_id = c2.city_id 
JOIN country c3 ON c2.country_id = c3.country_id 
```

- 문제10번) country 가 china 가 아닌 지역에 사는, 고객의 이름(first_name, last_name)과 , email, phonenumber, country, city 를 알려주세요
```sql
SELECT c.first_name , c.last_name , c.email , a.address , a.district , a.phone , c2.city , c3.country 
FROM customer c 
JOIN address a ON c.address_id = a.address_id 
JOIN city c2 ON a.city_id = c2.city_id 
JOIN country c3 ON c2.country_id = c3.country_id 
WHERE c3.country NOT IN ('China')
```

- 문제11번) Horror 카테고리 장르에 해당하는 영화의 이름과 description 에 대해서 알려주세요
```sql
SELECT f.title , f.description 
FROM film f
JOIN film_category fc ON f.film_id = fc.film_id 
JOIN category c ON fc.category_id = c.category_id 
WHERE c."name" = 'Horror'
```

- 문제12번) Music 장르이면서, 영화길이가 60~180분 사이에 해당하는 영화의 title, description, length 를 알려주세요.
  - 영화 길이가 짧은 순으로 정렬해서 알려주세요.
```sql
SELECT f.title , f.description , f."length"
FROM film f 
JOIN film_category fc ON f.film_id = fc.film_id 
JOIN category c ON fc.category_id  = c.category_id 
WHERE c."name" = 'Music' AND f."length" BETWEEN 60 AND 180
ORDER BY f."length" ASC 
```

- 문제13번) actor 테이블을 이용하여,  배우의 ID, 이름, 성 컬럼에 추가로    'Angels Life' 영화에 나온 영화 배우 여부를 Y , N 으로 컬럼을 추가 표기해주세요.  해당 컬럼은 angelslife_flag로 만들어주세요.
```sql
SELECT a.actor_id , a.first_name , a.last_name ,
	CASE WHEN a.actor_id IN (SELECT fa.actor_id 
			         FROM  film_actor fa 
			         JOIN  film f ON fa.film_id = f.film_id
			         WHERE f.title = 'Angels Life') THEN 'Y'
			         ELSE 'N' END AS angelslife_flag
FROM actor a 
```

- 문제14번) 대여일자가 2005-06-01~ 14일에 해당하는 주문 중에서 , 직원의 이름(이름 성) = 'Mike Hillyer' 이거나  고객의 이름이 (이름 성) ='Gloria Cook'  에 해당 하는 rental 의 모든 정보를 알려주세요.
  - 추가로 직원이름과, 고객이름에 대해서도 fullname 으로 구성해서 알려주세요.
```sql
SELECT *, s.first_name ||' '|| s.last_name AS staff_fullname, c.first_name ||' '|| c.last_name AS customer_fullname
FROM rental r 
JOIN customer c ON r.customer_id = c.customer_id 
JOIN staff s ON r.staff_id = s.staff_id 
WHERE date(r.rental_date) BETWEEN '2005-06-01' AND '2005-06-14' 
AND (s.first_name ||' '|| s.last_name = 'Mike Hillyer' OR c.first_name ||' '|| c.last_name = 'Gloria Cook') 
```

- 문제15번) 대여일자가 2005-06-01~ 14일에 해당하는 주문 중에서 , 직원의 이름(이름 성) = 'Mike Hillyer' 에 해당 하는 직원에게  구매하지 않은  rental 의 모든 정보를 알려주세요.
  - 추가로 직원이름과, 고객이름에 대해서도 fullname 으로 구성해서 알려주세요.
```sql
SELECT *, s.first_name ||' '|| s.last_name AS staff_fullname, c.first_name ||' '|| c.last_name AS customer_fullname
FROM rental r 
JOIN customer c ON r.customer_id = c.customer_id 
JOIN staff s ON r.staff_id = s.staff_id 
WHERE date(r.rental_date) BETWEEN '2005-06-01' AND '2005-06-14' AND s.first_name ||' '|| s.last_name NOT IN ('Mike Hillyer')
```

## No.4

- 문제1번) store 별로 staff는 몇명이 있는지 확인해주세요.
```sql
SELECT store_id, count(*)
FROM staff s 
GROUP BY store_id 
```

- 문제2번) 영화등급(rating) 별로 몇개 영화film을 가지고 있는지 확인해주세요.
```sql
SELECT rating , count(*) 
FROM film f 
GROUP BY rating 
```

- 문제3번) 출현한 영화배우(actor)가 10명 초과한 영화명은 무엇인가요?
```sql
SELECT f.title 
FROM (SELECT fa.film_id, count(*)
	  FROM film_actor fa 
	  GROUP BY fa.film_id 
	  HAVING count(*) > 10) fcnt
JOIN film f ON f.film_id = fcnt.film_id
```

- 문제4번) 영화 배우(actor)들이 출연한 영화는 각각 몇 편인가요?
  - 영화 배우의 이름 , 성 과 함께 출연 영화 수를 알려주세요.
```sql
SELECT a.first_name , a.last_name , acnt.cnt
FROM (SELECT fa.actor_id , count(*) AS cnt
	  FROM film_actor fa 
	  GROUP BY fa.actor_id) acnt
JOIN actor a ON a.actor_id = acnt.actor_id
```

- 문제5번) 국가(country)별 고객(customer) 는 몇명인가요?
```sql
SELECT c.country , count(*)
FROM country c 
JOIN city c2 ON c.country_id  = c2.country_id 
JOIN address a ON c2.city_id = a.city_id 
JOIN customer c3 ON a.address_id = c3.address_id 
GROUP BY c.country 
```

- 문제6번) 영화 재고 (inventory) 수량이 3개 이상인 영화(film) 는?
  - store는 상관 없이 확인해주세요.
```sql
SELECT f.title , fcnt.cnt
FROM (SELECT i.film_id, count(*) AS cnt
	  FROM inventory i
	  GROUP BY i.film_id
	  HAVING count(*) > 3) fcnt
JOIN film f ON f.film_id = fcnt.film_id
```

- 문제7번) dvd 대여를 제일 많이한 고객 이름은?
```sql
SELECT c.first_name , c.last_name 
FROM (SELECT r.customer_id , count(*) AS cnt
	FROM rental r 
	GROUP BY r.customer_id 
	) cus
JOIN customer c ON cus.customer_id = c.customer_id 
ORDER BY cus.cnt DESC 
LIMIT 1
```

- 문제8번) rental 테이블을  기준으로,   2005년 5월26일에 대여를 기록한 고객 중, 하루에 2번 이상 대여를 한 고객의 ID 값을 확인해주세요.
```sql
SELECT customer_id , count(*)
FROM rental r 
WHERE date(rental_date) = '2005-05-26'
GROUP BY customer_id 
HAVING count(*) >= 2
```

- 문제9번) film_actor 테이블을 기준으로, 출현한 영화의 수가 많은  5명의 actor_id 와 , 출현한 영화 수 를 알려주세요.
```sql
SELECT actor_id , count(*)
FROM film_actor fa
GROUP BY actor_id 
ORDER BY count(*) DESC 
LIMIT 5
```

- 문제10번) payment 테이블을 기준으로,  결제일자가 2007년2월15일에 해당 하는 주문 중에서  ,  하루에 2건 이상 주문한 고객의  총 결제 금액이 10달러 이상인 고객에 대해서 알려주세요. (고객의 id,  주문건수 , 총 결제 금액까지 알려주세요)
```sql
SELECT customer_id , count(*), sum(amount)
FROM payment p 
WHERE date(payment_date) = '2007-02-15'
GROUP BY customer_id 
HAVING count(*) >= 2 AND sum(amount) >= 10
```

- 문제11번) 사용되는 언어별 영화 수는?
```sql
SELECT l."name" , count(*)
FROM "language" l 
JOIN film f ON l.language_id = f.language_id 
GROUP BY l."name" 
-- 언어명 별 카운트 
```
```sql
SELECT language_id , count(*)
FROM film f 
GROUP BY language_id 
-- 언어 아이디별 카운트 
```

- 문제12번) 40편 이상 출연한 영화 배우(actor) 는 누구인가요?
```sql
SELECT a.first_name , a.last_name 
FROM (SELECT fa.actor_id, count(*) AS cnt 
	  FROM film_actor fa
	  GROUP BY fa.actor_id
	  HAVING count(*) >= 40) fcnt
JOIN actor a ON fcnt.actor_id = a.actor_id
```

- 문제13번) 고객 등급별 고객 수를 구하세요. (대여 금액 혹은 매출액  에 따라 고객 등급을 나누고 조건은 아래와 같습니다.)
```
A 등급은 151 이상
B 등급은 101 이상 150 이하
C 등급은   51 이상 100 이하
D 등급은   50 이하
- 대여 금액의 소수점은 반올림 하세요.
HINT
반올림 하는 함수는 ROUND 입니다.	
```
```sql
SELECT CASE WHEN rating.total >= 151 THEN 'A'
	WHEN rating.total BETWEEN 101 AND 150 THEN 'B'
	WHEN rating.total BETWEEN 51 AND 100 THEN 'C'
	WHEN rating.total <= 50 THEN 'D' END AS grade, count(*) 
FROM (SELECT customer_id , round(sum(amount)) AS total 
	FROM payment p 
	GROUP BY customer_id ) rating  
GROUP BY grade
ORDER BY grade ASC 
```

## No.5

- 문제1번) 영화 배우가, 영화 180분 이상의 길이 의 영화에 출연하거나, 영화의 rating 이 R 인 등급에 해당하는 영화에 출연한 영화 배우에 대해서, 
```
영화 배우 ID 와 (180분이상 / R등급영화)에 대한 Flag 컬럼을 알려주세요.
film_actor 테이블와 film 테이블을 이용하세요.
union, unionall, intersect, except 중 상황에 맞게 사용해주세요.
actor_id 가 동일한 flag 값 이 여러개 나오지 않도록 해주세요.
```
```sql
SELECT fa.actor_id, 'over 180 min' AS flag
FROM film_actor fa 
WHERE fa.film_id IN (SELECT f.film_id 
		     FROM film f
		     WHERE f.length >= 180)
UNION 
SELECT fa.actor_id , 'rating R ' AS flag
FROM film_actor fa 
WHERE fa.film_id IN (SELECT f.film_id 
		  			 FROM film f
			         WHERE f.rating = 'R')
ORDER BY actor_id 
```

- 문제2번) R등급의 영화에 출연했던 배우이면서, 동시에, Alone Trip의 영화에 출연한  영화배우의 ID 를 확인해주세요.
```
film_actor 테이블와 film 테이블을 이용하세요.
union, unionall, intersect, except 중 상황에 맞게 사용해주세요.
```
```sql
SELECT actor_id 
FROM film_actor fa 
WHERE fa.film_id IN (SELECT f.film_id 
		     FROM film f
		     WHERE f.title = 'Alone Trip')
INTERSECT 
SELECT actor_id 
FROM film_actor fa 
WHERE fa.film_id IN (SELECT f.film_id 
		     FROM film f
 	             WHERE f.rating = 'R')
ORDER BY actor_id 
```
```sql
-- UNION 사용하지 않고 JOIN으로 동일하게 조회
SELECT fa.actor_id 
FROM film f 
JOIN film_actor fa ON f.film_id = fa.film_id 
WHERE f.title = 'Alone Trip' AND f.rating = 'R'
```
- 문제3번) G 등급에 해당하는 필름을 찍었으나, 영화를 20편이상 찍지 않은 영화배우의 ID 를 확인해주세요.
```
film_actor 테이블와 film 테이블을 이용하세요.
union, unionall, intersect, except 중 상황에 맞게 사용해주세요.
```
- INTERSECT
```sql
SELECT actor_id 
FROM film_actor fa 
WHERE fa.film_id IN (SELECT f.film_id 
		     FROM film f
		     WHERE f.rating = 'G')
INTERSECT 
SELECT actor_id 
FROM film_actor fa 
GROUP BY actor_id 
HAVING count(*) < 20
ORDER BY actor_id 
```
- EXCEPT 
```sql
SELECT actor_id 
FROM film_actor fa 
WHERE fa.film_id IN (SELECT f.film_id 
		     FROM film f
		     WHERE f.rating = 'G')
EXCEPT 
SELECT actor_id 
FROM film_actor fa 
GROUP BY actor_id 
HAVING count(*) >= 20
ORDER BY actor_id 
```
- 문제4번) 필름 중에서,  필름 카테고리가 Action, Animation, Horror 에 해당하지 않는 필름 아이디를 알려주세요.
  - category 테이블을 이용해서 알려주세요.
- 조회 방법은 다양하게 존재함
- JOIN + IN + EXCEPT
```sql
SELECT f.film_id 
FROM film f 
EXCEPT
SELECT fc.film_id 
FROM category c 
JOIN film_category fc ON c.category_id = fc.category_id 
WHERE c."name" IN ('Action', 'Animation', 'Horror')
ORDER BY film_id 
```
- JOIN + NOT IN + INTERSECT
```sql
SELECT f.film_id 
FROM film f 
INTERSECT
SELECT fc.film_id 
FROM category c 
JOIN film_category fc ON c.category_id = fc.category_id 
WHERE c."name" NOT IN ('Action', 'Animation', 'Horror')
ORDER BY film_id 
```

- IN + EXCEPT
```sql
SELECT f.film_id 
FROM film f 
EXCEPT	
SELECT fc.film_id 
FROM film_category fc 
WHERE fc.category_id IN (SELECT c.category_id 
			 FROM category c
			 WHERE c."name" IN ('Action', 'Animation', 'Horror'))
ORDER BY film_id 
```
- NOT IN + INTERSECT
```sql
SELECT f.film_id 
FROM film f 
INTERSECT 	
SELECT fc.film_id 
FROM film_category fc 
WHERE fc.category_id IN (SELECT c.category_id 
			 FROM category c
			 WHERE c."name" NOT IN ('Action', 'Animation', 'Horror'))
ORDER BY film_id 
```
- JOIN + NOT IN
```sql
SELECT fc.film_id 
FROM category c 
JOIN film_category fc ON c.category_id = fc.category_id 
WHERE c."name" NOT IN ('Action', 'Animation', 'Horror')
```

- 문제5번) Staff  의  id , 이름, 성 에 대한 데이터와 , Customer 의 id, 이름 , 성에 대한 데이터를  하나의  데이터셋의 형태로 보여주세요.
  - 컬럼 구성 : id, 이름 , 성, flag (직원/고객여부) 로 구성해주세요.
```sql
SELECT staff_id AS id , first_name , last_name , 'staff' AS flag
FROM staff s 
UNION ALL	
SELECT customer_id AS id , first_name , last_name , 'customer' AS flag 
FROM customer c 
ORDER BY id
```

- 문제6번) 직원과 고객의 이름이 동일한 사람이 혹시 있나요? 있다면, 해당 사람의 이름과 성을 알려주세요.
```sql
SELECT first_name , last_name , 'staff' AS flag
FROM staff s 
WHERE first_name IN (SELECT first_name
		     FROM customer c)
UNION 
SELECT first_name , last_name , 'customer' AS flag 
FROM customer c  
WHERE first_name IN (SELECT first_name
		     FROM staff s)
```
- 정답지에는 아래와 같이 나와있으나 이름과 성이 둘다 동일한 사람은 없다.
- 그래서 위와 같이 이름이 동일한 사람을 찾은 다음 이름과 성을 조회하는 쿼리로 작성하였음 
```sql
SELECT first_name , last_name 
FROM customer c 
INTERSECT 
SELECT first_name , last_name 
FROM staff s 
```
- 문제7번) 반납이 되지 않은 대여점(store)별 영화 재고 (inventory)와 전체 영화 재고를 같이 구하세요. (union all)
```sql
SELECT NULL , count(*)
FROM rental r
JOIN inventory i ON r.inventory_id = r.inventory_id
WHERE return_date IS NULL  
UNION ALL 
SELECT i.store_id , count(*)
FROM rental r 
JOIN inventory i ON r.inventory_id = r.inventory_id 
WHERE return_date IS NULL 
GROUP BY i.store_id
```

- 문제8번) 국가(country)별 도시(city)별 매출액, 국가(country)매출액 소계 그리고 전체 매출액을 구하세요. (union all)
```sql
SELECT c3.country , c2.city , sum(p.amount) AS total_amount
FROM payment p 
JOIN customer c ON p.customer_id = c.customer_id 
JOIN address a ON c.address_id = a.address_id 
JOIN city c2 ON a.city_id = c2.city_id 
JOIN country c3 ON c2.country_id = c3.country_id 
GROUP BY c3.country , c2.city 
UNION ALL 
SELECT c3.country , NULL , sum(p.amount) AS total_amount 
FROM payment p 
JOIN customer c ON p.customer_id = c.customer_id 
JOIN address a ON c.address_id = a.address_id 
JOIN city c2 ON a.city_id = c2.city_id 
JOIN country c3 ON c2.country_id = c3.country_id 
GROUP BY c3.country 
UNION ALL 
SELECT NULL , NULL , sum(p.amount) AS total_amount 
FROM payment p 
ORDER BY 1, 2

-- 마지막 전체 매출액의 경우 payment table로만 조회가 가능, 아래쿼리로 조회했을 때 동일한 것 확인 
--SELECT NULL , NULL , sum(p.amount) AS total_amount
--FROM payment p 
--JOIN customer c ON p.customer_id = c.customer_id 
--JOIN address a ON c.address_id = a.address_id 
--JOIN city c2 ON a.city_id = c2.city_id 
--JOIN country c3 ON c2.country_id = c3.country_id 
--ORDER BY 1, 2 
```
