# YtxCash

## finance

### tree

创建颜色类型

```db
CREATE TYPE color AS(
r smallint,
g smallint,
b smallint,
);
```

创建树表

```db
CREATE TABLE finance_tree(
path ltree primary key,
code text,
description text,
currency text,
smallest_fraction numeric,
color color,
notes text,
type text,
);
```

### table

创建枚举类型

```db
CREATE TYPE status AS ENUM('Y','N','P');
```

创建二维表

```db
CREATE TABLE finance_data(
path ltree references finance_tree(path),
transfer_from text,
time timestamp default current_timestamp,
date date default current_date,
number integer,
description text,
transfer_to text,
status status,
debit numeric(1000,2),
credit numeric(1000,2),
balance numeric(1000,2));
```

## products

```db
CREATE TABLE products (
    product_no integer PRIMARY KEY,
    name text,
    price numeric
);
```

## orders

```db
CREATE TABLE orders (
    order_id integer PRIMARY KEY,
    shipping_address text,
);

CREATE TABLE order_items (
    product_no integer REFERENCES products ON DELETE RESTRICT ON UPDATE CASCADE,
    order_id integer REFERENCES orders ON DELETE CASCADE ON UPDATE CASCADE,
    quantity integer,
    PRIMARY KEY (product_no, order_id)
);
```
