Домашнее задание к модулю 4 (часть 2)

1.Добавление/изменение/удаление записи из таблицы.


a. Добавить в таблицу [Reskilling].[EDU].[customers] себя как клиента, округ [district]: South.

![1 1](https://github.com/user-attachments/assets/bbb0eeed-4f5d-47ad-83d8-d2defa9e1e8b)

[Uploading SQLQuery1.1sql.sql…]()

INSERT INTO EDU.customers(first_name, last_name, phone_number, district,street, house, apartment)

VALUES ('Vladimir', 'Savin', 79998788028, 'South', 'Rabinovaja',55, 160);

SELECT *

FROM EDU.customers;



b. В созданной строке в таблице [Reskilling].[EDU].[customers] изменить округ [district] на North в рамках транзакции с ROLLBACK.

![1 2](https://github.com/user-attachments/assets/509278ff-a295-4038-9517-fec4ea0a346a)

[Uploading SQLQuery1.2.sql…]()

BEGIN TRANSACTION;

    SELECT *
    
    FROM EDU.customers
    
    WHERE customer_id = 78;
    
UPDATE EDU.customers

SET district = 'North'

WHERE customer_id = 78;

    SELECT *
    
    FROM EDU.customers
    
    WHERE customer_id = 78;
    
ROLLBACK TRANSACTION;

    SELECT *
    
    FROM EDU.customers
    
    WHERE customer_id = 78;



c. Удалить созданную строку в таблице [Reskilling].[EDU].[customers] в рамках транзакции с ROLLBACK. 

![1 3](https://github.com/user-attachments/assets/06334f1e-854f-473b-869e-ac812dc08c91)

[Uploading SQLQuery1.3-- Start a new transaction

BEGIN TRANSACTION;

    SELECT *
    
    FROM EDU.customers
    
    WHERE customer_id = 78;
    
DELETE FROM EDU.customers

WHERE customer_id = 78;

    SELECT *
    
    FROM EDU.customers
    
    WHERE customer_id = 78;
    
ROLLBACK TRANSACTION;

    SELECT *
    
    FROM EDU.customers
    
    WHERE customer_id = 78;
    
.sql…]()


2.Из списка заказов [Reskilling].[EDU].[orders] выведи id заказа [order_id], фамилию клиента [last_name], имя клиента [first_name] и дату заказа [date_get]. 

![2 1](https://github.com/user-attachments/assets/30f4c12a-dd91-427a-87c2-2e8d0dab256b)

[Uploading SQLQuery2.1.

SELECT EDU.orders.order_id, 

       EDU.customers.last_name, 
       
       EDU.customers.first_name, 
       
       EDU.orders.date_get
       
FROM EDU.orders 

INNER JOIN EDU.customers

ON EDU.orders.customer_id = EDU.customers.customer_id;sql…]()


3.Добавить в таблицу [Reskilling].[EDU].[courier_info] 5 строк с данными, указанными ниже, двумя способами в рамках транзакции с ROLLBACK.

![3 1](https://github.com/user-attachments/assets/a2003d82-3e68-4cb5-a092-a61ddfaff313)

[Uploading SQLQuery3.1.sql…]

BEGIN TRANSACTION

SELECT *

FROM EDU.courier_info

INSERT INTO EDU.courier_info(

		first_name,
  
		last_name,
  
		phone_number,
  
		delivery_type
  
	)
 
SELECT 'Kate',

	'Radriges',
 
	14163203211,
 
	'bike';
 
INSERT INTO EDU.courier_info(

		first_name,
  
		last_name,
  
		phone_number,
  
		delivery_type
	)
 
SELECT 'Jennifer',

	'Gordon',
 
	14163211252,
 
	'car';
 
INSERT INTO EDU.courier_info(

		first_name,
  
		last_name,
  
		phone_number,
  
		delivery_type
	)
 
SELECT 'Mario',

	'Lorenson',
 
	14161224054,
 
	'foot';
 
INSERT INTO EDU.courier_info(

		first_name,
  
		last_name,
  
		phone_number,
  
		delivery_type
  
	)
 
SELECT 'Bella',

	'Visputchu',
 
	14167581967,
 
	'foot';
 
INSERT INTO EDU.courier_info(

		first_name,
  
		last_name,
  
		phone_number,
  
		delivery_type
  
	)
 
SELECT 'Erica',

	'Evans',
 
	14167618582,
 
	'bike';
 
SELECT *

FROM EDU.courier_info ROLLBACK TRANSACTION

SELECT *

FROM EDU.courier_info()


[Uploading SQLQuery3.2.sql…]()

BEGIN TRANSACTION

SELECT *

FROM EDU.courier_info

INSERT INTO EDU.courier_info(

		first_name,
  
		last_name,
  
		phone_number,
  
		delivery_type
	)
 
VALUES ('Kate', 'Radriges', 14163203211, 'bike'),

	('Jennifer', 'Gordon', 14163211252, 'car'),
 
	('Mario', 'Lorenson', 14161224054, 'foot'),
 
	('Bella', 'Visputchu', 14167581967, 'foot'),
 
	('Erica', 'Evans', 14167618582, 'bike')
 
SELECT *

FROM EDU.courier_info ROLLBACK TRANSACTION

SELECT *

FROM EDU.courier_info

 4.Из списка доставок [Reskilling].[EDU].[delivery_list] выведи фамилию курьера [last_name] + имя курьера [first_name] (назови столбец Курьер) и количество его доставленных заказов. 

 ![4 1](https://github.com/user-attachments/assets/ab0faabf-b5ac-47d6-9bbb-18f2c9923434)

[Uploading SQLQuery4.

SELECT CONCAT(EDU.courier_info.last_name,' ' ,EDU.courier_info.first_name) AS ������, COUNT(EDU.delivery_list.courier_id) AS �����������

FROM EDU.courier_info INNER JOIN EDU.delivery_list

ON EDU.courier_info.courier_id = EDU.delivery_list.courier_id

WHERE taken = 'YES'

GROUP BY EDU.courier_info.first_name ,EDU.courier_info.last_name;1.sql…]()




5.Выведи название продукта и его спрос. Если продукт купили более 3 шт. за всё время, то "Высокий спрос", если 3 и менее шт., то "Низкий спрос".

[Uploading SQLQuery5.1.sql…]()

SELECT EDU.products.menu_name,

	CASE
 
		WHEN SUM(EDU.orders_products.quantity) > 3 THEN '������� �����'
  
		ELSE '������ �����'
  
	END AS 'Product_demand'
 
FROM EDU.products

	INNER JOIN EDU.orders_products ON EDU.products.product_id = EDU.orders_products.product_id
 
GROUP BY EDU.products.menu_name

ORDER BY Product_demand ASC;



