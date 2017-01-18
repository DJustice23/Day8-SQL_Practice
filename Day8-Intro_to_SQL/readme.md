* How many users are there?

SELECT COUNT(*) FROM users

Answer: 50 Users



* What are the 5 most expensive items?

SELECT items.title, items.price FROM items ORDER BY items.price DESC LIMIT 5 ;

Answer:
"Small Cotton Gloves"	"9984"
"Small Wooden Computer"	"9859"
"Awesome Granite Pants"	"9790"
"Sleek Wooden Hat"	"9390"
"Ergonomic Steel Car"	"9341"


* What's the cheapest book? (Does that change for "category is exactly 'book'" versus "category contains 'book'"?)

SELECT items.category, items.price, items.title FROM items WHERE category = 'Books' ORDER BY items.price ASC LIMIT 1

Answer 1) "Books"	"1496"	"Ergonomic Granite Chair"



SELECT items.category, items.price, items.title FROM items WHERE category LIKE '%Books%' ORDER BY items.price ASC LIMIT 1

Answer 2)  The result is the same



* Who lives at "6439 Zetta Hills, Willmouth, WY"? Do they have another address?

SELECT users.first_name, users.last_name FROM users inner join addresses on users.id = addresses.user_id WHERE addresses.street = "6439 Zetta Hills"

Answer 1) "Corrine"	"Little"

SELECT addresses.street , addresses.city, addresses.state FROM addresses inner join users on users.id = addresses.user_id WHERE users.last_name = "Little"

Answer 2)  "54369 Wolff Forges"	"Lake Bryon"	"CA"



* Correct Virginie Mitchell's address to "New York, NY, 10108".

UPDATE addresses SET city = 'New York', state = 'NY', zip = 10108 WHERE id = 41 and user_id = 39

After query executes:  "Virginie"	"Mitchell"	"12263 Jake Crossing"	"New York"	"NY"	"10108"



* How much would it cost to buy one of each tool?

SELECT SUM(items.price) FROM items WHERE items.category LIKE “%Tools%”

Answer) 46477



* How many total items did we sell?

SELECT SUM(orders.quantity) FROM orders

Answer)  2125



* How much was spent on books?

SELECT SUM(orders.quantity * items.price) FROM items INNER JOIN orders ON orders.item_id = items.id where items.category LIKE "%Books%"

Answer)  1081352



* Simulate buying an item by inserting a User for yourself and an Order for that User.

INSERT INTO users VALUES (100, "David", "Justice", "justice@boomtownroi.com")

INSERT INTO orders VALUES (123123, 100, 1, 1, "2017-01-18 10:00:00")


Adventurer Mode
* What item was ordered most often? Grossed the most money?

SELECT items.title, SUM(orders.quantity) FROM orders INNER JOIN items ON orders.item_id = items.id GROUP BY items.title ORDER BY SUM(orders.quantity) DESC

Answer 1)
Two items had the same quantity sold:
"Incredible Granite Car"	"72"
"Rustic Steel Shirt"	"72"

SELECT items.title, SUM(orders.quantity * items.price ) FROM orders INNER JOIN items ON orders.item_id = items.id GROUP BY items.title ORDER BY SUM(orders.quantity * items.price ) DESC

Answer 2)
"Incredible Granite Car"	"525240"



* What user spent the most?

SELECT users.first_name, users.last_name, SUM(orders.quantity * items.price ) FROM users INNER JOIN orders ON orders.user_id = users.id INNER JOIN items ON items.id = orders.item_id GROUP BY orders.user_id ORDER BY SUM(orders.quantity * items.price ) DESC LIMIT 1

Answer)  "Hassan"	"Runte"	"639386"



* What were the top 3 highest grossing categories?
SELECT items.category, SUM(orders.quantity * items.price ) FROM items INNER JOIN orders ON orders.item_id = items.id GROUP BY items.category ORDER BY SUM(orders.quantity * items.price ) DESC LIMIT 3

Answer)
"Music, Sports & Clothing"	"525240"
"Beauty, Toys & Sports"	"449496"
"Sports"	"448410"

