# Java SQL

A student that completes this project shows that they can:

* Query data from a single table
* Query data from multiple tables
* Create a new database using PostgreSQL

## Introduction

Working with SQL

## Instructions

Reimport the Northwind database into PostgreSQL using pgAdmin. This is the same data we used during the guided project.

* [x] ***pgAdmin data refresh***

* Select the northwind database created during the guided project.

* Tools -> Query Tool
  * Open file northwind.sql (you used this script during the guided project)
  * Execute

* Look under
  * northwind -> Schemas -> public -> tables

* Clear query windows

### Answer the following data queries. Keep track of the SQL you write by pasting it into this document under its appropriate header below in the provided SQL code block. You will be submitting that through the regular fork, change, pull process

* [x] ***find all customers that live in London. Returns 6 records***

  <details><summary>hint</summary>

  * This can be done with SELECT and WHERE clauses
  </details>

```SQL
SELECT *
FROM customers
WHERE city='London'
```

* [x] ***find all customers with postal code 1010. Returns 3 customers***

  <details><summary>hint</summary>

  * This can be done with SELECT and WHERE clauses
  </details>

```SQL
SELECT *
FROM customers
WHERE postal_code='1010'
```

* [x] ***find the phone number for the supplier with the id 11. Should be (010) 9984510***

  <details><summary>hint</summary>

  * This can be done with SELECT and WHERE clauses
  </details>

```SQL
SELECT *
FROM suppliers
where supplier_id='11'
```

* [x] ***list orders descending by the order date. The order with date 1998-05-06 should be at the top***

  <details><summary>hint</summary>

  * This can be done with SELECT, WHERE, and ORDER BY clauses
  </details>

```SQL
SELECT *
FROM orders
ORDER BY order_date DESC
```

* [x] ***find all suppliers who have names longer than 20 characters. Returns 11 records***

  <details><summary>hint</summary>

  * This can be done with SELECT and WHERE clauses
  * You can use `length(company_name)` to get the length of the name
  </details>

```SQL
SELECT *
FROM suppliers
WHERE length(company_name) > 20
```

* [x] ***find all customers that include the word 'MARKET' in the contact title. Should return 19 records***

  <details><summary>hint</summary>

  * This can be done with SELECT and a WHERE clause using the LIKE keyword
  * Don't forget the wildcard '%' symbols at the beginning and end of your substring to denote it can appear anywhere in the string in question
  * Remember to convert your contact title to all upper case for case insensitive comparing so upper(contact_title)
  </details>

```SQL
SELECT *
FROM customers
WHERE UPPER(contact_title) LIKE '%MARKET%'
```

* [x] ***add a customer record for***
* customer id is 'SHIRE'
* company name is 'The Shire'
* contact name is 'Bilbo Baggins'
* the address is '1 Hobbit-Hole'
* the city is 'Bag End'
* the postal code is '111'
* the country is 'Middle Earth'
  <details><summary>hint</summary>

  * This can be done with the INSERT INTO clause
  </details>

```SQL
INSERT INTO customers(customer_id, company_name, contact_name, address, city, postal_code, country)
			VALUES('SHIRE', 'The Shire', 'Bilbo Baggins', '1 Hobbit-Hole', 'Bag End', '111', 'Middle Earch')
```

* [x] ***update _Bilbo Baggins_ record so that the postal code changes to _"11122"_***

  <details><summary>hint</summary>

  * This can be done with UPDATE and WHERE clauses
  </details>

```SQL
UPDATE customers
SET postal_code='11122'
WHERE customer_id='SHIRE'
```

* [x] ***list orders grouped and ordered by customer company name showing the number of orders per customer company name. _Rattlesnake Canyon Grocery_ should have 18 orders***

  <details><summary>hint</summary>

  * This can be done with SELECT, COUNT, JOIN and GROUP BY clauses. Your count should focus on a field in the Orders table, not the Customer table
  * There is more information about the COUNT clause on [W3 Schools](https://www.w3schools.com/sql/sql_count_avg_sum.asp)
  </details>

```SQL
SELECT c.company_name, COUNT(*) number_of_orders
FROM orders o JOIN customers c
on o.customer_id=c.customer_id
GROUP BY c.company_name
ORDER BY c.company_name
```

* [x] ***list customers by contact name and the number of orders per contact name. Sort the list by the number of orders in descending order. _Jose Pavarotti_ should be at the top with 31 orders followed by _Roland Mendal_ with 30 orders. Last should be _Francisco Chang_ with 1 order***

  <details><summary>hint</summary>

  * This can be done by adding an ORDER BY clause to the previous answer and changing the group by field
  </details>

```SQL
SELECT c.contact_name, COUNT(*) number_of_orders
FROM orders o JOIN customers c
on o.customer_id=c.customer_id
GROUP BY c.contact_name
ORDER BY number_of_orders DESC
```

* [x] ***list orders grouped by customer's city showing the number of orders per city. Returns 69 Records with _Aachen_ showing 6 orders and _Albuquerque_ showing 18 orders***

  <details><summary>hint</summary>

  * This is very similar to the previous two queries, however, it focuses on the City rather than the Customer Names
  </details>

```SQL
SELECT c.city, COUNT(*) number_of_orders
FROM orders o JOIN customers c
on o.customer_id=c.customer_id
GROUP BY c.city
ORDER BY number_of_orders DESC
```

## Data Normalization

Note: This step does not use PostgreSQL!

* [ ] ***Take the following data and normalize it into a 3NF database***

| Person Name | Pet Name | Pet Type | Pet Name 2 | Pet Type 2 | Pet Name 3 | Pet Type 3 | Fenced Yard | City Dweller |
|-------------|----------|----------|------------|------------|------------|------------|-------------|--------------|
| Jane        | Ellie    | Dog      | Tiger      | Cat        | Toby       | Turtle     | No          | Yes          |
| Bob         | Joe      | Horse    |            |            |            |            | No          | No           |
| Sam         | Ginger   | Dog      | Miss Kitty | Cat        | Bubble     | Fish       | Yes         | No           |

Below are some empty tables to be used to normalize the database

* Not all of the cells will contain data in the final solution
* Feel free to edit these tables as necessary

Table Name: Person

|person_id   |name        |fenced_yard |city_dweller|            |            |            |            |            |
|------------|------------|------------|------------|------------|------------|------------|------------|------------|
|1           |Jane        |No          |Yes         |            |            |            |            |            |
|2           |Bob         |No          |No          |            |            |            |            |            |
|3           |Sam         |Yes         |No          |            |            |            |            |            |
|            |            |            |            |            |            |            |            |            |
|            |            |            |            |            |            |            |            |            |
|            |            |            |            |            |            |            |            |            |
|            |            |            |            |            |            |            |            |            |

Table Name: Pet

|pet_id      |name        |person_id   |type_id     |            |            |            |            |            |
|------------|------------|------------|------------|------------|------------|------------|------------|------------|
|1           |Ellie       |1           |1           |            |            |            |            |            |
|2           |Joe         |2           |2           |            |            |            |            |            |
|3           |Ginger      |3           |1           |            |            |            |            |            |
|4           |Tiger       |1           |3           |            |            |            |            |            |
|5           |Miss Kitty  |3           |3           |            |            |            |            |            |
|6           |Toby        |1           |4           |            |            |            |            |            |
|7           |Bubble      |3           |5           |            |            |            |            |            |

Table Name: type   

|type_id     |type        |            |            |            |            |            |            |            |
|------------|------------|------------|------------|------------|------------|------------|------------|------------|
|1           |Dog         |            |            |            |            |            |            |            |
|2           |Horse       |            |            |            |            |            |            |            |
|3           |Cat         |            |            |            |            |            |            |            |
|4           |Turtle      |            |            |            |            |            |            |            |
|5           |Fish        |            |            |            |            |            |            |            |
|            |            |            |            |            |            |            |            |            |
|            |            |            |            |            |            |            |            |            |

Table Name:

|            |            |            |            |            |            |            |            |            |
|------------|------------|------------|------------|------------|------------|------------|------------|------------|
|            |            |            |            |            |            |            |            |            |
|            |            |            |            |            |            |            |            |            |
|            |            |            |            |            |            |            |            |            |
|            |            |            |            |            |            |            |            |            |
|            |            |            |            |            |            |            |            |            |
|            |            |            |            |            |            |            |            |            |
|            |            |            |            |            |            |            |            |            |

---

### Stretch Goals

* [x] ***delete all customers that have no orders. Should delete 2 (or 3 if you haven't deleted the record added) records***

```SQL
DELETE
FROM customers
WHERE NOT EXISTS (SELECT customer_id
				  FROM orders
				  WHERE customer_id = customers.customer_id)
```

* [x] ***Create Database and Table: After creating the database, tables, columns, and constraint, generate the script necessary to recreate the database. This script is what you will submit for review***

* use pgAdmin to create a database, naming it `budget`.
* add an `accounts` table with the following _schema_:

  * `id`, numeric value with no decimal places that should autoincrement.
  * `name`, string, add whatever is necessary to make searching by name faster.
  * `budget` numeric value.

* constraints
  * the `id` should be the primary key for the table.
  * account `name` should be unique.
  * account `budget` is required.

```SQL
--
-- PostgreSQL database dump
--

-- Dumped from database version 12.4
-- Dumped by pg_dump version 12.4

-- Started on 2020-09-30 15:27:21 CDT

SET statement_timeout = 0;
SET lock_timeout = 0;
SET idle_in_transaction_session_timeout = 0;
SET client_encoding = 'UTF8';
SET standard_conforming_strings = on;
SELECT pg_catalog.set_config('search_path', '', false);
SET check_function_bodies = false;
SET xmloption = content;
SET client_min_messages = warning;
SET row_security = off;

SET default_tablespace = '';

SET default_table_access_method = heap;

--
-- TOC entry 203 (class 1259 OID 24914)
-- Name: accounts; Type: TABLE; Schema: public; Owner: postgres
--

CREATE TABLE public.accounts (
    id integer NOT NULL,
    name text,
    budget numeric NOT NULL
);


ALTER TABLE public.accounts OWNER TO postgres;

--
-- TOC entry 202 (class 1259 OID 24912)
-- Name: accounts_id_seq; Type: SEQUENCE; Schema: public; Owner: postgres
--

CREATE SEQUENCE public.accounts_id_seq
    AS integer
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1;


ALTER TABLE public.accounts_id_seq OWNER TO postgres;

--
-- TOC entry 2965 (class 0 OID 0)
-- Dependencies: 202
-- Name: accounts_id_seq; Type: SEQUENCE OWNED BY; Schema: public; Owner: postgres
--

ALTER SEQUENCE public.accounts_id_seq OWNED BY public.accounts.id;


--
-- TOC entry 2827 (class 2604 OID 24917)
-- Name: accounts id; Type: DEFAULT; Schema: public; Owner: postgres
--

ALTER TABLE ONLY public.accounts ALTER COLUMN id SET DEFAULT nextval('public.accounts_id_seq'::regclass);


--
-- TOC entry 2959 (class 0 OID 24914)
-- Dependencies: 203
-- Data for Name: accounts; Type: TABLE DATA; Schema: public; Owner: postgres
--

COPY public.accounts (id, name, budget) FROM stdin;
\.


--
-- TOC entry 2966 (class 0 OID 0)
-- Dependencies: 202
-- Name: accounts_id_seq; Type: SEQUENCE SET; Schema: public; Owner: postgres
--

SELECT pg_catalog.setval('public.accounts_id_seq', 1, false);


--
-- TOC entry 2829 (class 2606 OID 24924)
-- Name: accounts accounts_name_key; Type: CONSTRAINT; Schema: public; Owner: postgres
--

ALTER TABLE ONLY public.accounts
    ADD CONSTRAINT accounts_name_key UNIQUE (name);


--
-- TOC entry 2831 (class 2606 OID 24922)
-- Name: accounts accounts_pkey; Type: CONSTRAINT; Schema: public; Owner: postgres
--

ALTER TABLE ONLY public.accounts
    ADD CONSTRAINT accounts_pkey PRIMARY KEY (id);


-- Completed on 2020-09-30 15:27:21 CDT

--
-- PostgreSQL database dump complete
--
```

To see the script

* Right Click on the database name
  * Select Backup...
    * Set a filename
      * To put the file backup.sql in your home directory, you could use `backup.sql`
    * Set format to `Plain`
* The script you want is now in the text file named above.
  * Copy the script from the text file into the SQL code block above!

![Database Script](assets/jx-12-m3-script.gif)
