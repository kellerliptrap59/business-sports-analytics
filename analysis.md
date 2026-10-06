# Business & Sports Operations SQL Analysis

## 1. Customer Segment Revenue

**Question:** Which customer segments generate the most revenue?

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

**Question:** Which locations generate the most revenue?

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
Cleveland OH	244104.69
Chicago IL	165043.74
Columbus OH	160901.5
Detroit MI	119630.51
Indianapolis IN	114019.93
Pittsburgh PA	108644.03
Cincinnati OH	93584.32
Buffalo NY	90412.68
