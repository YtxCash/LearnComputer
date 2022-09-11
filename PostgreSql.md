# postgresql

## install

debian/ubuntu

```shell
sudo apt-get --purge remove postgresql\*
dpkg -l | grep postgres
sudo apt install postgresql
```

manjaro

```shell
systemctl start postgresql.service
systemctl stop postgresql.service
systemctl enable postgresql.service

systemctl status postgresql.service
journalctl -xeu postgresql.service

su - postgres -c "initdb --locale en_US.UTF-8 -D '/var/lib/postgres/data'"
```

## database

```db
createdb db_name
psql db_name         --open database db_name
psql -l              --list databases
dropdb db_name
Ctrl + L             --clear
```

## table

create

```db
\d                                                  --list tables in current db
DROP TABLE table1, table2, table3;                  --drop table
CREATE EXTENSION IF NOT EXISTS ltree;               --open ltree

CREATE TABLE weather(
    city            varchar(80),
    temp_lo         int,           -- 最低温度
    temp_hi         int,           -- 最高温度
    prcp            real,          -- 湿度
    date            date
);

// default value
CREATE TABLE products (
    product_no integer,
    name text,
    price numeric DEFAULT 9.99
);

// 生成列
CREATE TABLE people (
    ...,
    height_cm numeric,
    height_in numeric GENERATED ALWAYS AS (height_cm / 2.54) STORED
);

// 检查约束
CREATE TABLE products (
    product_no integer,
    name text,
    price numeric CHECK (price > 0) --列约束
);

CREATE TABLE products (
    product_no integer,
    name text,
    price numeric CONSTRAINT positive_price CHECK (price > 0) --列约束，给约束起名
);

CREATE TABLE products (
    product_no integer,
    name text,
    price numeric CHECK (price > 0), --列约束
    discounted_price numeric CHECK (discounted_price > 0), --列约束
    CHECK (price > discounted_price) --表约束
);

// 非空约束
CREATE TABLE products (
    product_no integer NOT NULL, --非空约束
    name text NOT NULL, --非空约束
    price numeric
);

CREATE TABLE products (
    product_no integer PRIMARY KEY, --主键
    name text,
    price numeric
);

CREATE TABLE orders (
    order_id integer PRIMARY KEY, --主键
    shipping_address text,
    ...
);

CREATE TABLE order_items (
    product_no integer REFERENCES products ON DELETE RESTRICT, --阻止删除被引用的行
    order_id integer REFERENCES orders ON DELETE CASCADE,      --被引用行删除后，引用它的行也应该被自动删除
    quantity integer,
    PRIMARY KEY (product_no, order_id)
);
```

change

```db
ALTER TABLE products ADD COLUMN description text; --add a column

ALTER TABLE products DROP COLUMN description; --remove a column

ALTER TABLE products DROP COLUMN description CASCADE; --remove a column and dependence

ALTER TABLE products ADD CHECK (name <> ''); -add check
ALTER TABLE products ADD CONSTRAINT some_name UNIQUE (product_no);
ALTER TABLE products ADD FOREIGN KEY(column) REFERENCES product_groups;
ALTER TABLE table_name ADD PRIMARY KEY(column);

ALTER TABLE products DROP CONSTRAINT some_name; --remove check

ALTER TABLE products ALTER COLUMN price SET DEFAULT 7.77; --change default value
ALTER TABLE products ALTER COLUMN price DROP DEFAULT; --remove default value

ALTER TABLE products ALTER COLUMN price TYPE numeric(10,2); --change volumn type

ALTER TABLE products RENAME COLUMN product_no TO product_number; --rename a column

ALTER TABLE products RENAME TO items; --rename a table
```

## values

```db
INSERT INTO weather (city, temp_lo, temp_hi, prcp, date)
    VALUES ('San Francisco', 43, 57, 0.0, '1994-11-29');

COPY weather FROM '/home/user/weather.txt';

UPDATE weather
    SET temp_hi = temp_hi - 2,  temp_lo = temp_lo - 2
    WHERE date > '1994-11-28';

DELETE FROM weather WHERE city = 'Hayward';

DELETE FROM tablename;
```

## check from a table

```db
SELECT * RROM weather;


SELECT                              --查询表中所有列的属性
    column_name,
    data_type
FROM
    information_schema.columns
WHERE
    table_name = 'table_name';


SELECT city, (temp_hi+temp_lo)/2 AS temp_avg, date FROM weather;

SELECT * FROM weather
    WHERE city = 'San Francisco' AND prcp > 0.0;

SELECT * FROM weather
    ORDER BY city;

SELECT * FROM weather
    ORDER BY city, temp_lo;

SELECT DISTINCT city
    FROM weather;

SELECT DISTINCT city
    FROM weather
    ORDER BY city;

SELECT max(temp_lo) FROM weather;

SELECT city FROM weather
    WHERE temp_lo = (SELECT max(temp_lo) FROM weather);
```

## check from multi tables

```db
SELECT *
    FROM weather, cities
    WHERE city = name;

SELECT weather.city, weather.temp_lo, weather.temp_hi,
       weather.prcp, weather.date, cities.location
    FROM weather, cities
    WHERE cities.name = weather.city;

SELECT *
    FROM weather w, cities c
    WHERE w.city = c.name;
```

## views

```db
CREATE VIEW myview AS
    SELECT *
        FROM weather w, cities c
        w.city = c.name;

SELECT * FROM myview;
```

## foreign key

```db
CREATE TABLE cities (
        city     varchar(80) primary key,
        location point
);

CREATE TABLE weather (
        city      varchar(80) references cities(city),
        temp_lo   int,
        temp_hi   int,
        prcp      real,
        date      date
);
```

## transaction

```db
BEGIN;
UPDATE accounts SET balance = balance - 100.00
    WHERE name = 'Alice';
-- etc etc
COMMIT;
```

## permission

```db
ALTER TABLE table_name OWNER TO new_owner;     --修改所有者
GRANT UPDATE ON table_name TO joe;             --joe是已有角色
REVOKE ALL ON accounts FROM PUBLIC;            --撤销public的所有权限
```
