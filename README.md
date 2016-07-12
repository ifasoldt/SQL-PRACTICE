# SQL-PRACTICE

PROMPT: How many users are there?
A: 50
Query: SELECT COUNT(id) FROM users;

P: What are the 5 most expensive items?
A:
title|price
Small Cotton Gloves|9984
Small Wooden Computer|9859
Awesome Granite Pants|9790
Sleek Wooden Hat|9390
Ergonomic Steel Car|9341
Query: SELECT title, price FROM items ORDER BY price DESC LIMIT 5;

P:What's the cheapest book? (Does that change for "category is exactly 'book'" versus "category contains 'book'"?)
A:
title|price|category
Ergonomic Granite Chair|1496|Books
NO DOESN'T CHANGE
QUERY: SELECT title, price, category FROM items WHERE category = "Books" ORDER BY price ASC LIMIT 1; SELECT title, price, category FROM items WHERE category LIKE  "%Books%" ORDER BY price ASC LIMIT 1;

P: Who lives at "6439 Zetta Hills, Willmouth, WY"? Do they have another address?
A:
first_name|last_name|id
Corrine|Little|40
SHE DOES HAVE ANOTHER ADDRESS: 54369 Wolff Forges|Lake Bryon|CA
QUERY:
SELECT users.first_name, users.last_name, users.id FROM users JOIN addresses ON users.id = addresses.user_id WHERE addresses.street = "6439 Zetta Hills" AND addresses.city ="Willmouth" AND addresses.state = "WY";
SELECT addresses.street, addresses.city, addresses.state FROM addresses JOIN users ON addresses.user_id = users.id WHERE users.id = 40;

P: Correct Virginie Mitchell's address to "New York, NY, 10108"
A:
Query: UPDATE addresses SET city = "New York", state = "NY", zip = "10108" WHERE user_id = (SELECT id FROM users WHERE first_name = "Virginie" AND last_name = "Mitchell");

P: How much would it cost to buy one of each tool?
A: 46477
Query: SELECT SUM(price) FROM items WHERE category LIKE "%tool%";

P:How many total items did we sell?
A: 2125
Query: SELECT SUM(quantity) FROM orders

P:How much was spent on books?
A: 1081352
Query: SELECT SUM(items.price * orders.quantity) FROM items JOIN orders ON items.id = orders.item_id WHERE items.category LIKE "%books%";

P:Simulate buying an item by inserting a User for yourself and an Order for that User.
A:
Query:INSERT INTO users (first_name, last_name, email) VALUES ("Isaiah", "Fasoldt", "ifasoldt@gmail.com");
INSERT INTO orders (user_id, item_id, quantity, created_at) VALUES (51,47,5,"2015-02-09 00:40:31.095881");

P:What item was ordered most often? Grossed the most money?
A: Incredible Granite Car|72, Practical Rubber Computer|67806
Query:
SELECT items.title, SUM(quantity) FROM orders JOIN items ON orders.item_id = items.id GROUP BY item_id ORDER BY COUNT(quantity) DESC LIMIT 1;
SELECT items.title, SUM(orders.quantity * items.price) FROM orders JOIN items ON orders.item_id = items.id GROUP BY item_id ORDER BY SUM(orders.quantity * items.price) DESC LIMIT 1;

P: What user spent the most?
A:
first_name|last_name|SUM(items.price)
Hassan|Runte|639386
Query: SELECT users.first_name, users.last_name, SUM(items.price * orders.quantity) FROM orders JOIN users ON orders.user_id = users.id JOIN items ON orders.item_id = items.id GROUP BY users.id ORDER BY SUM(orders.quantity * items.price)DESC LIMIT 1;

P: What were the top 3 highest grossing categories?
A: Music, Sports & Clothing|525240
Beauty, Toys & Sports|449496
Sports|448410
Query: SELECT items.category, SUM(orders.quantity * items.price) FROM items JOIN orders ON items.id = orders.item_id GROUP BY category ORDER BY SUM(orders.quantity * items.price) DESC LIMIT 3;

![alt tag](./screenshot.png?raw=true)
