# Business & Sports Operations SQL Analysis

**Question 1 :** Which customer segments generate the most revenue?

**SQL:**
```sql
select customer_segment , round(sum(revenue),2) total_revenue
from customers c
join transactions t
	on c.customer_id = t.customer_id
group by customer_segment
order by total_revenue desc
```
| customer_segment | total_revenue |
|------------------|--------------:|
| Casual           | 467001.71     |
| Regular          | 339418.70     |
| Loyal            | 210262.50     |
| VIP              | 86378.16      |

**Question: 2** Which locations generate the most revenue?

**SQL:**
```sql
select concat(city, ' ', state) city , round(sum(revenue),2) total_revenue
from customers c 
join transactions t
	on c.customer_id = t.customer_id
join locations loc
	on c.location_code = loc.location_code
group by concat(city, ' ', state)
order by total_revenue desc
```
| city | total_revenue |
| :--- | :--- |
| Cleveland OH | 244104.69 |
| Chicago IL | 165043.74 |
| Columbus OH | 160901.5 |
| Detroit MI | 119630.51 |
| Indianapolis IN | 114019.93 |
| Pittsburgh PA | 108644.03 |
| Cincinnati OH | 93584.32 |
| Buffalo NY | 90412.68 |

**Question: 3** Which products and product categories drive revenue?

**SQL:**
```sql
select category, product_name, round(sum(revenue),2) total_revenue, ROUND((SUM(revenue) * 100.0) / SUM(SUM(revenue)) OVER(), 2) AS percentage_of_total
from products p
join transactions t
	on p.product_id = t.product_id
group by category, product_name
order by total_revenue desc

```
| category | product_name | total_revenue | percentage_of_total |
| :--- | :--- | :--- | :--- |
| Apparel | Scarf | 65371.75 | 5.93 |
| Collectibles | Team Pennant | 64936.6 | 5.89 |
| Headwear | Premium Jersey | 61966.59 | 5.62 |
| Headwear | Signed Baseball | 60613.06 | 5.49 |
| Accessories | Duffel Bag | 57943.56 | 5.25 |
| Collectibles | Athletic Pants | 56718.13 | 5.14 |
| Apparel | Youth T-Shirt | 54962.42 | 4.98 |
| Headwear | Logo Mug | 48914.5 | 4.43 |
| Apparel | Beanie | 45135.93 | 4.09 |
| Equipment | Limited Edition Tee | 40051.82 | 3.63 |
| Headwear | Training Shorts | 39372.45 | 3.57 |
| Headwear | Pullover Hoodie | 38871.73 | 3.52 |
| Apparel | Performance Cap | 37012.3 | 3.36 |
| Collectibles | Snapback | 36275.08 | 3.29 |
| Accessories | Golf Polo | 36257.72 | 3.29 |
| Collectibles | Quarter Zip | 33998.91 | 3.08 |

**Question: 4** Who are the highest-value customers?

```sql
select c.customer_id, location_code,customer_segment, round(sum(revenue),2) total_spent, 
count(*) number_purchases, ROUND(SUM(revenue) / COUNT(*), 2) avg_transaction
from customers c
join transactions t
	on c.customer_id = t.customer_id
group by c.customer_id, location_code, customer_segment
order by total_spent desc limit 10;
```

| customer_id | location_code | customer_segment | total_spent | number_purchases | avg_transaction |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 10845 | CLE | Regular | 3252.97 | 12 | 271.08 |
| 10502 | PIT | Casual | 3096.99 | 16 | 193.56 |
| 10799 | COL | VIP | 2759.16 | 15 | 183.94 |
| 10849 | PIT | Regular | 2658.53 | 15 | 177.24 |
| 10180 | CLE | Casual | 2632.6 | 11 | 239.33 |
| 10417 | CHI | Casual | 2631.28 | 15 | 175.42 |
| 10606 | DET | Casual | 2623.13 | 14 | 187.37 |
| 10700 | PIT | Regular | 2573.89 | 13 | 197.99 |
| 10619 | CIN | Casual | 2514.59 | 16 | 157.16 |
| 10415 | COL | Casual | 2464.55 | 12 | 205.38 |

## 2. Sports/Event Operations

**Question 5:** Which events attract the most attendees?

```sql
select e.event_id ,opponent, event_type, result, 
count(*) attendance, SUM(a.ticket_price) total_revenue, event_date
from events e
join attendance a
	on e.event_id = a.event_id
group by e.event_id, opponent, event_type, result, event_date
order by attendance desc;
```

| event_id | opponent | event_type | result | attendance | total_revenue | event_date |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 30058 | Pittsburgh | Regular Season | Loss | 81 | 4270 | 2025-05-27 |
| 30008 | Buffalo | Regular Season | Loss | 76 | 4264 | 2024-03-18 |
| 30024 | New York | Regular Season | Win | 72 | 3641 | 2024-08-05 |
| 30004 | New York | Exhibition | Loss | 71 | 4238 | 2024-02-18 |
| 30051 | Detroit | Exhibition | Loss | 71 | 3828 | 2025-04-27 |
| 30010 | Cincinnati | Regular Season | Win | 71 | 3898 | 2024-04-01 |
| 30036 | Chicago | Regular Season | Loss | 69 | 3667 | 2024-11-24 |
| 30030 | Columbus | Regular Season | Loss | 69 | 3415 | 2024-10-07 |
| 30022 | Chicago | Regular Season | Loss | 68 | 3695 | 2024-07-01 |
| 30071 | Columbus | Regular Season | Loss | 68 | 3716 | 2025-10-11 |
| 30067 | Cincinnati | Regular Season | Win | 67 | 3706 | 2025-08-12 |
| 30063 | New York | Regular Season | Win | 67 | 3629 | 2025-06-23 |
| 30026 | Cincinnati | Regular Season | Loss | 66 | 3873 | 2024-08-22 |
| 30041 | Pittsburgh | Regular Season | Win | 65 | 3741 | 2024-12-26 |
| 30034 | Pittsburgh | Regular Season | Loss | 65 | 3228 | 2024-11-17 |
| 30031 | Detroit | Regular Season | Win | 64 | 3236 | 2024-11-09 |

**Question 6:** Does team performance appear to affect attendance?

```sql
SELECT 
    e.event_type, 
    e.result, 
    COUNT(DISTINCT e.event_id) AS number_of_events,
    round(count(a.attendance_id) *1.0 / count(distinct e.event_id)) avg_attendance,
    round(sum(ticket_price) / count(*),2) avg_ticket_price
FROM events e
JOIN attendance a
    ON e.event_id = a.event_id
WHERE e.event_type != 'Special Event'
GROUP BY e.event_type, e.result
ORDER BY 
	field(e.event_type, 'Regular Season', 'Playoff', 'Exhibition'),
    field(e.result, 'Win', 'Loss', 'OT Loss');
```

| event_type | result | number_of_events | avg_attendance | avg_ticket_price |
| :--- | :--- | :--- | :--- | :--- |
| Regular Season | Win | 28 | 55 | 54.19 |
| Regular Season | Loss | 22 | 60 | 53.6 |
| Regular Season | OT Loss | 3 | 55 | 56.67 |
| Playoff | Win | 5 | 54 | 55.14 |
| Playoff | Loss | 5 | 56 | 52.34 |
| Playoff | OT Loss | 1 | 54 | 55.41 |
| Exhibition | Win | 2 | 55 | 56.29 |
| Exhibition | Loss | 10 | 56 | 54.33 |
| Exhibition | OT Loss | 1 | 47 | 49.34 |

**Question 7:** Which opponents generate the strongest attendance?

```sql
SELECT opponent,
count(distinct e.event_id) number_of_games,
count(a.customer_id) total_attendance,
round(count(a.customer_id) / count(distinct e.event_id)) avg_attendance,
round(avg(a.ticket_price),2) avg_ticket_price
FROM events e
JOIN attendance a
    ON e.event_id = a.event_id
group by opponent
order by avg_attendance desc
```
| opponent | number_of_games | total_attendance | avg_attendance | avg_ticket_price |
| :--- | :--- | :--- | :--- | :--- |
| Columbus | 6 | 359 | 60 | 53.01 |
| Buffalo | 6 | 345 | 58 | 54.03 |
| Pittsburgh | 9 | 525 | 58 | 53.76 |
| Chicago | 7 | 398 | 57 | 53.16 |
| Detroit | 9 | 511 | 57 | 53.2 |
| New York | 14 | 796 | 57 | 54.81 |
| Cincinnati | 11 | 621 | 56 | 56.36 |
| Indianapolis | 12 | 653 | 54 | 53.02 |
| Toronto | 3 | 151 | 50 | 53.34 |
| Milwaukee | 3 | 141 | 47 | 56.73 |

**Question 8:** Which products generate more revenue than the average product?

```sql
SELECT
    p.product_name,
    ROUND(SUM(t.revenue), 2) AS total_revenue
FROM products p
JOIN transactions t
    ON p.product_id = t.product_id
GROUP BY p.product_name
HAVING SUM(t.revenue) > (
    SELECT AVG(total_revenue)
    FROM (
        SELECT
            p.product_name,
            SUM(t.revenue) AS total_revenue
        FROM products p
        JOIN transactions t
            ON p.product_id = t.product_id
        GROUP BY p.product_name
    ) AS product_revenue
)
ORDER BY total_revenue DESC;
```

| product_name | total_revenue |
| :--- | :--- |
| Scarf | 65371.75 |
| Team Pennant | 64936.6 |
| Premium Jersey | 61966.59 |
| Signed Baseball | 60613.06 |
| Duffel Bag | 57943.56 |
| Athletic Pants | 56718.13 |
| Youth T-Shirt | 54962.42 |
| Logo Mug | 48914.5 |
| Beanie | 45135.93 |
| Limited Edition Tee | 40051.82 |
| Training Shorts | 39372.45 |
| Pullover Hoodie | 38871.73 |
| Performance Cap | 37012.3 |


**Question 9:** How can customers be classified based on their total spending?

```sql
select c.customer_id, customer_segment, round(SUM(t.revenue),2) total_spent,
	case
		when SUM(t.revenue) >= 2000 then 'High Value'
		when SUM(t.revenue) < 2000 and SUM(t.revenue) >= 1000 then  'Medium Value'
		Else 'Low Value'
	end as customer_priority
from customers c
join transactions t
	on c.customer_id = t.customer_id
group by c.customer_id, customer_segment;
```

| customer_id | customer_segment | total_spent | customer_priority |
| :--- | :--- | :--- | :--- |
| 10072 | Loyal | 1130.85 | Medium Value |
| 10645 | Regular | 582.09 | Low Value |
| 10732 | Casual | 993.56 | Low Value |
| 10885 | Regular | 627.93 | Low Value |
| 10965 | Loyal | 550.8 | Low Value |
| 10689 | Casual | 935.67 | Low Value |
| 10282 | Regular | 942.99 | Low Value |
| 10133 | Regular | 2233.89 | High Value |
| 10534 | Loyal | 314.55 | Low Value |
| 10975 | Regular | 1097.04 | Medium Value |
| 10269 | Regular | 1584.89 | Medium Value |
| 10412 | Regular | 1142.36 | Medium Value |


**Question 10:** Which customers have spent more than the average customer?
```sql
SELECT 
    c.customer_id, 
    ROUND(SUM(t.revenue), 2) AS total_spent
FROM customers c
JOIN transactions t
    ON c.customer_id = t.customer_id
GROUP BY c.customer_id
HAVING SUM(t.revenue) > (
    SELECT AVG(customer_total)
    FROM (
        SELECT SUM(revenue) AS customer_total
        FROM transactions
        GROUP BY customer_id
    ) avg_spent
)
```

| customer_id | total_spent |
| :--- | :--- |
| 10861 | 1270.72 |
| 10082 | 1397.06 |
| 10754 | 2265.06 |
| 10945 | 1220.91 |
| 10172 | 1382.43 |
| 10720 | 1695.21 |
| 10270 | 1210.95 |
| 10899 | 2300.27 |
| 10626 | 1379.61 |
| 10062 | 1467.1 |
| 10874 | 1691.36 |
| 10320 | 1613.33 |


**Question 11:** What percentage of customers' total spending comes from each customer segment?

```sql
WITH percent_total AS (
    SELECT
        c.customer_segment,
        count(distinct c.customer_id) total_customers,
        SUM(t.revenue) AS total_spending
    FROM customers c
    JOIN transactions t
        ON c.customer_id = t.customer_id
    GROUP BY c.customer_segment
)
select 
	customer_segment, 
	total_customers, 
	round(total_spending,2) total_spending,
    round((total_spending * 100) / sum(total_spending) OVER(),2) percentage
from percent_total
order by percentage desc
```

| customer_segment | total_customers | total_spending | percentage |
| :--- | :--- | :--- | :--- |
| Casual | 427 | 467001.71 | 42.34 |
| Regular | 299 | 339418.7 | 30.77 |
| Loyal | 197 | 210262.5 | 19.06 |
| VIP | 77 | 86378.16 | 7.83 |


**Question 12:** Are customers who attend more events also higher-value customers?

```sql
with attendance as (
	select c.customer_id, 
    count(*) games_attended
	from customers c
	join attendance a
		on c.customer_id = a.customer_id
	group by c.customer_id
),
customer_rev as(
	select 
	c.customer_id, 
	round(sum(revenue),2) total_revenue
	from customers c
	join transactions t
		on c.customer_id = t.customer_id
	group by c.customer_id
)
select a.customer_id, 
games_attended, 
total_revenue as total_spent,
case
	when games_attended >= 7 then 'High Engagement'
    when games_attended <= 6  and games_attended >= 3  then 'Medium Engagement'
    else 'Low Engagement'
end as engagement_level
from attendance a
join customer_rev cr
	on a.customer_id = cr.customer_id
````

| customer_id | games_attended | total_spent | engagement_level |
| :--- | :--- | :--- | :--- |
| 10336 | 3 | 1156.8 | Medium Engagement |
| 10668 | 8 | 1726.44 | High Engagement |
| 10124 | 4 | 538.32 | Medium Engagement |
| 10014 | 2 | 724.96 | Low Engagement |
| 10416 | 5 | 1312.4 | Medium Engagement |
| 10776 | 6 | 902.9 | Medium Engagement |
| 10027 | 5 | 1369.79 | Medium Engagement |
| 10757 | 7 | 1883.17 | High Engagement |

**Question 13:** Rank the Top Customers in Each Segment.

```sql
create temporary table customer_spending(
	select c.customer_id, 
	customer_segment, 
	count(c.customer_id) number_of_purchases,
	round(sum(revenue),2) total_spent
	from customers c
	join transactions t
		on c.customer_id = t.customer_id
	group by c.customer_id, customer_segment
);

with ranked_customers as(
	select 
	customer_id,
	customer_segment,
    total_spent,
    number_of_purchases,
	rank() over(partition by customer_segment order by total_spent desc) segment_rank
	from customer_spending
)
select
	customer_id,
    customer_segment,
    number_of_purchases,
    total_spent,
    segment_rank
from ranked_customers
where segment_rank <=3
order by customer_segment, segment_rank;
```
| customer_id | customer_segment | number_of_purchases | total_spent | segment_rank |
| :--- | :--- | :--- | :--- | :--- |
| 10502 | Casual | 16 | 3096.99 | 1 |
| 10180 | Casual | 11 | 2632.6 | 2 |
| 10417 | Casual | 15 | 2631.28 | 3 |
| 10623 | Loyal | 15 | 2403.4 | 1 |
| 10293 | Loyal | 15 | 2282.85 | 2 |
| 10680 | Loyal | 13 | 2088.55 | 3 |
| 10845 | Regular | 12 | 3252.97 | 1 |
| 10849 | Regular | 15 | 2658.53 | 2 |
| 10700 | Regular | 13 | 2573.89 | 3 |
| 10799 | VIP | 15 | 2759.16 | 1 |
| 10121 | VIP | 10 | 2387.3 | 2 |
| 10560 | VIP | 16 | 2340.23 | 3 |





