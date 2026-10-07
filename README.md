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

