# java-DB-aggregate-function

1. Select the total number of Orders.

2. Select number of orders that have ship region.

3. Select the most expensive product.

4. Select total price of order with id = 10258;

5. Select the least expensive product from order with id = 10263

6. Select all products that have price which is above the average price.

7. Calculate number of products from category Seafood.

8. Sumarize total value of products in orders made in 1996 (before discount).












SELECT COUNT(order_id) AS total_number_of_orders FROM orders;

SELECT COUNT(order_id) AS total_orders_with_ship_region FROM orders
	WHERE ship_region IS NOT NULL;
	
SELECT product_name AS most_expansive_product FROM products 
	WHERE unit_price = (SELECT MAX (unit_price) FROM products);

SELECT SUM(unit_price * quantity) AS total_price from order_details 
	WHERE order_id = 10258;
	
SELECT p.product_name AS least_expansive_product, 
	MIN(o.unit_price) AS minimum_unit_price FROM order_details o
		JOIN products p ON o.product_id = p.product_id
		WHERE o.order_id = 10263
		GROUP BY product_name
		ORDER BY minimum_unit_price LIMIT 1;
		
SELECT product_name, unit_price FROM products
	WHERE unit_price > (SELECT AVG(unit_price) FROM products);
	
SELECT p.product_name FROM products p
	JOIN categories c ON p.category_id = c.category_id
	WHERE category_name = 'Seafood';
	
SELECT SUM (od.unit_price * od.quantity) AS total_value FROM order_details od
	JOIN orders o ON od.order_id = o.order_id
	WHERE EXTRACT(YEAR FROM o.order_date) = 1996;
