# Business & Sports Operations SQL Analysis

## 1. Customer Segment Revenue

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
select category, product_name, sum(revenue) total_revenue, ROUND((SUM(revenue) * 100.0) / SUM(SUM(revenue)) OVER(), 2) AS percentage_of_total
from products p
join transactions t
	on p.product_id = t.product_id
group by category, product_name
order by total_revenue desc
```
| category | product_name | total_revenue | percentage_of_total |
| :--- | :--- | :--- | :--- |
| Apparel | Scarf | 65371.74999999999 | 5.93 |
| Collectibles | Team Pennant | 64936.59999999996 | 5.89 |
| Headwear | Premium Jersey | 61966.58999999989 | 5.62 |
| Headwear | Signed Baseball | 60613.06000000014 | 5.49 |
| Accessories | Duffel Bag | 57943.55999999991 | 5.25 |
| Collectibles | Athletic Pants | 56718.130000000056 | 5.14 |
| Apparel | Youth T-Shirt | 54962.42000000008 | 4.98 |
| Headwear | Logo Mug | 48914.49999999988 | 4.43 |
| Apparel | Beanie | 45135.929999999906 | 4.09 |
| Equipment | Limited Edition Tee | 40051.82000000002 | 3.63 |
| Headwear | Training Shorts | 39372.449999999924 | 3.57 |
| Headwear | Pullover Hoodie | 38871.72999999997 | 3.52 |
| Apparel | Performance Cap | 37012.30000000012 | 3.36 |
| Collectibles | Snapback | 36275.07999999991 | 3.29 |
| Accessories | Golf Polo | 36257.719999999965 | 3.29 |
| Collectibles | Quarter Zip | 33998.91000000007 | 3.08 |
| Apparel | Keychain | 33739.04000000006 | 3.06 |
| Equipment | Youth Hoodie | 32563.28999999997 | 2.95 |
| Equipment | Mini Helmet | 30370.34000000002 | 2.75 |
| Collectibles | Replica Jersey | 30281.079999999976 | 2.75 |
| Accessories | Travel Tumbler | 28436.039999999957 | 2.58 |
| Equipment | Backpack | 25746.319999999923 | 2.33 |
| Accessories | Performance Polo | 24389.329999999944 | 2.21 |
| Apparel | Championship Coll... | 21830.69999999997 | 1.98 |
| Apparel | Classic Logo T-Shirt | 21036.31000000027 | 1.91 |
| Equipment | Signed Basketball | 20540.36000000003 | 1.86 |
| Accessories | Sunglasses | 20348.619999999974 | 1.84 |
| Accessories | Classic Cap | 14287.429999999962 | 1.3 |
| Collectibles | Water Bottle | 13787.299999999997 | 1.25 |
| Equipment | Performance T-Shirt | 7302.359999999985 | 0.66 |

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

**Question 6:** Does attendance effect the teams preformance?

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

Question: How can customers be classified based on their total spending?

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
