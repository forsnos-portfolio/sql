# sql

```sql
### CREATE TABLE ###
CREATE TABLE products
(
    id          BIGINT PRIMARY KEY AUTO_INCREMENT,
    name        TINYTEXT,
    description TEXT,
    price       INT       DEFAULT 0,
    updated_at  TIMESTAMP DEFAULT CURRENT_TIMESTAMP NOT NULL ON UPDATE CURRENT_TIMESTAMP,
    created_at  TIMESTAMP DEFAULT CURRENT_TIMESTAMP NOT NULL
);
```
```sql
CREATE TABLE customers
(
    id         BIGINT PRIMARY KEY AUTO_INCREMENT,
    name       VARCHAR(100)                        NOT NULL,
    phone      VARCHAR(16)                         NOT NULL,
    email      VARCHAR(128),
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP NOT NULL ON UPDATE CURRENT_TIMESTAMP,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP NOT NULL,

    UNIQUE (phone, email)
);
```
```sql
CREATE TABLE carts
(
    customer_id BIGINT NOT NULL,
    product_id  BIGINT NOT NULL,

    FOREIGN KEY (customer_id) REFERENCES customers (id) ON DELETE CASCADE,
    FOREIGN KEY (product_id) REFERENCES products (id) ON DELETE CASCADE,

    UNIQUE (customer_id, product_id)
);
```
```sql
### INSERT ###
INSERT INTO products (name, description) VALUE ('Булка', 'Мягкий хлеб');

INSERT INTO customers (name, phone, email)
VALUES ('Диана', '89566779090', 'sc@mail.ru'),
       ('Катя', '89778778845', 'f@mail.ru');

INSERT INTO carts (customer_id, product_id) VALUE (?, ?);
```
```sql
### SELECT ###
SELECT name, phone, email FROM customers;
SELECT name, description, price FROM products;
SELECT name FROM customers WHERE id = 2;
SELECT name, phone FROM customers WHERE id = 2 AND email = 'flo@mail.ru';
SELECT name, phone FROM customers WHERE id = 4 OR email = 'flo@mail.ru';

SELECT email FROM customers WHERE id BETWEEN 1 AND 4;
SELECT * FROM customers WHERE name LIKE 'К%';

SELECT * FROM customers ORDER BY id DESC;
SELECT * FROM customers ORDER BY id LIMIT 4;
SELECT * FROM customers ORDER BY id LIMIT 2 OFFSET 1;
SELECT * FROM products ORDER BY created_at DESC LIMIT 1;
SELECT * FROM customers LIMIT 1, 2;
```
```sql
### RELATIONS ###
SELECT id, name, phone, email FROM carts c JOIN customers c1 ON c.customer_id = c1.id;
SELECT * FROM customers c LEFT JOIN carts c1 ON c.id = c1.customer_id;
SELECT phone FROM customers c LEFT JOIN carts c1 ON c.id = c1.customer_id;
SELECT name FROM carts c LEFT JOIN customers c1 on c1.id = c.customer_id;
```
```sql
### FUNCTIONS ###
SELECT SUM(price), name FROM products GROUP BY name;
SELECT SUM(price) FROM products WHERE name = 'Булка';
SELECT COUNT(id) FROM products;
SELECT AVG(id) FROM products;
```
```sql
### UPDATE ###
UPDATE customers SET name = 'Петя' WHERE id = 2;
UPDATE products SET price = 34 WHERE name = 'Булка';
```
```sql
### DELETE ###
DELETE FROM customers WHERE id = 2;
```
```sql
### TABLE ###
ALTER TABLE products ADD price INT NOT NULL;
ALTER TABLE customers DROP PRIMARY KEY;
DROP TABLE products;
TRUNCATE products;
```
