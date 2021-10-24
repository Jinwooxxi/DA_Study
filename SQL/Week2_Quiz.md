# 2주차 퀴즈

* 문제1) 주문일이 2017-09-02 일에 해당 하는 주문건에 대해서,  어떤 고객이, 어떠한 상품에 대해서 얼마를 지불하여 상품을 구매했는지 확인해주세요.
```sql
SELECT o.orderdate , o.customerid , od.productnumber , od.quotedprice * od.quantityordered AS price
FROM orders o 
JOIN order_details od ON o.ordernumber = od.ordernumber
WHERE date(o.orderdate) = '2017-09-02'
ORDER BY o.orderdate , o.customerid 
```

* 문제2) 헬멧을 `주문한 적 없는 고객`을 보여주세요. 
  - 헬맷은, Products 테이블의 productname 컬럼을 이용해서 확인해주세요.
```sql
SELECT c.customerid , order_helmet.customerid
FROM customers c 
LEFT JOIN (SELECT o.ordernumber , o.customerid , od.productnumber , p.productname 
		   FROM orders o
		   JOIN order_details od ON o.ordernumber = od.ordernumber
		   JOIN products p ON od.productnumber = p.productnumber 
		   WHERE p.productname LIKE '%Helmet') AS order_helmet ON c.customerid = order_helmet.customerid
WHERE order_helmet.customerid IS NULL 
```

* 문제3) 모든 제품 과 주문 일자를 나열하세요. (`주문되지 않은 제품`도 포함해서 보여주세요.)
```sql
SELECT p.productnumber , p.productname , o.orderdate 
FROM products p 
LEFT JOIN order_details od ON p.productnumber = od.productnumber 
LEFT JOIN orders o ON od.ordernumber = o.ordernumber 
ORDER BY p.productnumber 
```

* 문제4) 캘리포니아 주와 캘리포니아 주가 아닌 STATS 로 구분하여 각 주문량을 알려주세요. (CASE문 사용)
```sql
SELECT st_quantity.state , count(DISTINCT st_quantity.ordernumber)
FROM (SELECT c.customerid , c.custstate ,
		CASE WHEN c.custstate = 'CA' THEN 'CA'
		ELSE 'OTHER' END AS state, o.ordernumber 
	  FROM customers c 
	  JOIN orders o ON c.customerid = o.customerid 
	  LEFT JOIN order_details od ON o.ordernumber = od.ordernumber) AS st_quantity
GROUP BY st_quantity.state
```

* 문제5) 공급 업체 와 판매 제품 수를 나열하세요. 단 판매 제품수가 2개 이상인 곳만 보여주세요.
```sql
SELECT v.vendorid , count(*)
FROM product_vendors pv 
JOIN vendors v ON pv.vendorid = v.vendorid 
GROUP BY v.vendorid 
HAVING count(*) >= 2
ORDER BY v.vendorid  
```

* 문제6) 가장 높은 주문 금액을 산 고객은 누구인가요?
  - 주문일자별, 고객의 아이디별로, 주문번호, 주문 금액도 함께 알려주세요.
```sql
SELECT most.orderdate, most.customerid, most.ordernumber, sum(most.price) AS prices
FROM (SELECT o.orderdate, o.customerid, o.ordernumber, od.quotedprice * od.quantityordered AS price 
	  FROM orders o
	  JOIN order_details od ON o.ordernumber = od.ordernumber) AS most 
GROUP BY most.orderdate , most.customerid , most.ordernumber 
ORDER BY prices DESC 
LIMIT 1
```

* 문제7) 주문일자별로, 주문 갯수와, 고객수를 알려주세요.
  - ex) 하루에 한 고객이 주문을 2번이상했다고 가정했을때 -> 해당의 경우는 고객수는 1명으로 계산해야합니다.
```sql
SELECT o.orderdate , count(ordernumber) AS order_cnt , count(DISTINCT customerid) AS cus_cnt 
FROM orders o 
GROUP BY o.orderdate 
```
```sql
SELECT orderdate, count(DISTINCT ordernumber) , count(DISTINCT customerid)
FROM (SELECT o.orderdate, o.customerid, o.ordernumber, od.productnumber , od.quotedprice * od.quantityordered AS prices
	  FROM orders o
	  JOIN order_details od ON o.ordernumber = od.ordernumber) AS db
GROUP BY orderdate
```

* 문제8) 생략

* 문제9) 타이어과 헬멧을 모두 산적이 있는 고객의 ID 를 알려주세요.
  - 타이어와 헬멧에 대해서는 , Products 테이블의 productname 컬럼을 이용해서 확인해주세요.
```sql
SELECT o.customerid  
FROM orders o 
JOIN order_details od ON o.ordernumber = od.ordernumber 
JOIN products p ON od.productnumber = p.productnumber 
WHERE p.productname LIKE '%Tire%'
INTERSECT 
SELECT o.customerid  
FROM orders o 
JOIN order_details od ON o.ordernumber = od.ordernumber 
JOIN products p ON od.productnumber = p.productnumber 
WHERE p.productname LIKE '%Helmet%'
ORDER BY customerid 
```

*  문제10) 타이어는 샀지만, 헬멧을 사지 않은 고객의 ID 를 알려주세요. Except 조건을 사용하여, 풀이 해주세요.
   - 타이어, 헬멧에 대해서는, Products 테이블의 productname 컬럼을 이용해서 확인해주세요.
```sql
SELECT o.customerid  
FROM orders o 
JOIN order_details od ON o.ordernumber = od.ordernumber 
JOIN products p ON od.productnumber = p.productnumber 
WHERE p.productname LIKE '%Tire%'
EXCEPT 
SELECT o.customerid  
FROM orders o 
JOIN order_details od ON o.ordernumber = od.ordernumber 
JOIN products p ON od.productnumber = p.productnumber 
WHERE p.productname LIKE '%Helmet%'
ORDER BY customerid
```
