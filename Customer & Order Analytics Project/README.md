1. How many many unique orders were placed in January? In other words, how many distinct order ids do we have?
SELECT COUNT(distinct orderid)
FROM BIT_DB.JanSales
WHERE length(orderid) = 6
AND orderid <> 'Order ID';

![Query 1](../query1.jpg)
![Query 2](../query2.jpg)
![Query 3](../query3.jpg)
![Query 4](../query4.jpg)
![Query 5](../query5.jpg)
![Query 6](../query6.jpg)
![Query 7](../query7.jpg)

