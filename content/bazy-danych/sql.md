---
layout: default
title: SQL
parent: Bazy danych
nav_order: 1
---

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

Relacyjne bazy danych.


## Syntax

### SELECT

Zwraca wiersze ze wskazanymi kolumnami.

Pierwsze `n` wyników: `SELECT TOP n` (nie wszystkie dbms wspierają; odpowiednik MYSQL: `LIMIT`).

Tylko unikalne wyniki: `SELECT DISTINCT ...`

```sql
SELECT customer_name -- or * if all columns
FROM customers;
```

### WHERE

Filtrowanie wyników

```sql
SELECT *
FROM customers
WHERE age > 18;
```

### ORDER BY

Sortuje rezultaty. Domyślna kolejność ASC.

```sql
SELECT *
FROM orders
ORDER BY shipping_date; -- ORDER BY shipping_date ASC|DESC;
```

### AND, OR, NOT

Operatory używane w `WHERE`.

```sql
SELECT *
FROM customers
WHERE age > 18 AND (country = 'Poland' OR country = 'Germany'); -- WHERE NOT country = 'USA';
```

### ANY, ALL

Predykaty używane z subquery.

```sql
SELECT product_name
FROM products
WHERE product_id = ANY ( -- ALL; operators: =, <>, >, >=, <, <=
    SELECT product_id
    FROM orders
    WHERE quantity > 1000
);
```

### EXISTS

Sprawdza czy w subquery jest jakikolwiek rezultat.

```sql
SELECT supplier_name
FROM suppliers s
WHERE EXISTS (
    SELECT *
    FROM products p
    WHERE p.supplier_id = s.supplier_id AND p.price < 100
)
```

### IS NULL

Sprawdza czy wartość jest nullem. 

**Nie można użyć do tego operatora `=`.**

```sql
SELECT *
FROM orders
WHERE shipping_date IS NULL;
```

### IN

Skrót dla wielu `OR`-ów.

```sql
SELECT *
FROM customers
WHERE age > 18 AND country IN ('Poland', 'Germany');
```

### BETWEEN

Matchowanie danego przedziału, którym mogą być liczby, tekst oraz daty. Przedział jest inkluzywny.

Dla tekstu zwróci wiersze jeżeli mieszczą się one alfabetycznie w danym przedziale.

```sql
SELECT *
FROM products
WHERE price BETWEEN 10 AND 20;
```

### LIKE

Wyszukiwanie po patternie.
Do wyszukania bardziej zaawansowanymi patternami można użyc regexów (MYSQL: `REGEXP`, PostgreSQL: `~`).

Wildcardy:
- `%`: 0 lub więcej znaków.
- `_`: 1 znak.

```sql
SELECT *
FROM customers
WHERE customer_name LIKE 'a%';
```

### MIN, MAX, AVG, SUM, COUNT

```sql
SELECT MIN(price) -- MAX, AVG, SUM, COUNT, ...
FROM products;
```

### JOIN

Służą do łączenia danych znajdujących się w różnych tabelach.

Typy:
- `[INNER] JOIN`: zwraca wiersze które matchują w obu tabelach,
- Outer:
  - `LEFT [OUTER] JOIN`: zwraca wszystkie wiersze z lewej tabeli i matchujące z prawej tabeli,
  - `RIGHT [OUTER] JOIN`: zwraca wszystkie wiersze z prawej tabeli i matchujące z lewej tabeli,
  - `FULL [OUTER] JOIN`: zwraca wszystkie wiersze z prawej i lewej tabeli,
- Bez klauzuli `ON`:
  - `CROSS JOIN`: zwraca wszystkie permutacje wierszy z lewej i prawej tabeli,
  - `NATURAL [INNER, LEFT, RIGHT] JOIN`: zwraca to co wybrany join; sam dobierze kolumny po której joinować (weźmie te o tej samej nazwie),
- Self: nazwa typu joina (obojętnie jakiego) w którym lewa i prawa tabela jest tą samą tabelą; trzeba nadać im alias np. `t1`, `t2` żeby się odwołać do konkretnej.

```sql
SELECT *
FROM customers c
JOIN orders o -- [INNER, LEFT [OUTER], RIGHT [OUTER], FULL [OUTER], CROSS, NATURAL [...]] JOIN
    ON o.customer_id = c.customer_id; -- CROSS, NATURAL don't take match condition.

-- Same thing but using common column name.
SELECT *
FROM customers
JOIN orders
    USING (customer_id);
```

### UNION

Łączy wyniki wielu `SELECT`ów.

Warunki:
- muszą mieć tę samą liczbę kolumn,
- kolumny muszą mieć kompatybilny typ danych,
- kolumny muszą być w tej samej kolejności.

Defaultowo nie listuje duplikatów. Aby otrzymać duplikaty używamy `UNION ALL`.

```sql
SELECT * FROM orders
UNION -- UNION ALL
SELECT * FROM orders_archived;
```

### GROUP BY

Grupuje wiersze które mają takie same wartości.

```sql
SELECT country, COUNT(customer_id) AS count -- Or COUNT(*) which will count all rows, incl. null ones.
FROM customers
WHERE age > 18
GROUP BY country
ORDER BY count DESC;
```

### HAVING

Filtrowanie wyników po zgrupowaniu.
Można się odwoływać tylko do tego co zselectowaliśmy.

```sql
SELECT country, COUNT(customer_id) AS count
FROM customers
WHERE age > 18
GROUP BY country
HAVING COUNT(customer_id) > 5
ORDER BY count DESC;
```

### INSERT

Dodaje wiersze do tabeli.

```sql
-- Specify values for all columns in the order they appear in the table.
INSERT INTO customers
VALUES (DEFAULT, 'Adam B', 29, ...); -- Use DEFAULT placeholder to fallback to default value.

-- Specify values only for columns that you care about in any order.
INSERT INTO customers (age,  customer_name,  country)
VALUES                (29,   'Adam B',      'Poland');

-- Insert multiple rows.
INSERT INTO customers (age,  customer_name,  country)
VALUES                (29,   'Adam B',      'Poland'),
                      (30,   'Adam C',      'Germany'),
                      ...;
```

### UPDATE

Aktualizuje wiersze w tabeli.

```sql
UPDATE customers
SET country = 'Finland'
WHERE customer_id = 1;
```

### DELETE

Kasuje wiersze w tabeli.

Jeżeli nie damy `WHERE` to wyczyści całą tabelę.

```sql
DELETE FROM customers
WHERE country = 'Germany';
```

## View

Wirtualna tabela która może być używane w innych miejscach.
Nie przechowują danych, tylko widok na tabelę pod spodem, stąd nazwa "view".
Często współdzielone przez VCS jako skrypty `*.sql` żeby wszyscy mogli używać.

Updateable view: rodzaj widoku który może być wykorzystywany do aktualizowania/kasowania
danych. Używamy tego samego syntaxu co normalnie, tylko zamiast nazwy tabeli dajemy nazwę view.
View jest updateowalny jeżeli bazuje tylko na jednej tabeli i nie zawiera:
- aggregate functions (`MIN`, `MAX`, ...),
- subqueriesów,
- `GROUP BY`/`HAVING`,
- `DISTINCT`,
- `UNION`,
- (i inne niszowe warunki).

Zalety widoków:
- upraszczają queriesy,
- wprowadzenie abstrakcji nad bezpośrednim dostępem do tabeli,
- jeden punkt zmian (w odniesieniu do wielu luźno leżących skryptów),
- ograniczenie bezpośredniego dostępu do danych w tabeli (np. ukrycie jakiś wyników przez wbudowanie `WHERE` w viewsa)
  - jeżeli mamy viewsa który zwraca dane za np. >2019 rok, to
    nie możemy przypadkowo zmodyfikować historycznych danych.

```sql
CREATE VIEW sales_by_customer AS
SELECT c.customer_id, c.customer_name, SUM(o.total) AS sum
FROM customers c
JOIN orders o USING (customer_id)
GROUP BY c.client_id, c.customer_name;

-- Somewhere else.
SELECT *
FROM sales_by_customer
WHERE sum > 100;

-- Other related.
DROP VIEW sales_by_customer;
CREATE OR REPLACE VIEW sales_by_customer AS ...;
```

## Stored Procedure

Zgrupowane komendy SQL które mozna zapisać i używać w innych miejscach.
Mogą przyjmować parametry. Coś w rodzaju funkcji.

Po co:
- trzymanie rzeczy związanych z bazą danych w bazie danych,
- możliwość zmiany działania apki bez konieczności rekompilacji i redeployu,
- szybsze, bo wykonywane bezpośrednio na bazie (optymalizowane przez db),
- ograniczenie bezpośredniego dostępu do danych w tabeli.

| [Stored Procedure](#stored-procedure)                   | [View](#view)                                            |
| ------------------------------------------------------- | -------------------------------------------------------- |
| Może przyjmować parametry.                              |  Nie może przyjmować parametrów.                         |
| Może zawierać wiele statementów (pętle, ify, ...).      |  Może zawierać tylko jednego `SELECT`a.                  |
| Używane do grupowania wielu często wykonywanych komend. |  Używane do grupowania wielu często wykonywanych joinów. |

```sql
-- This delimiter thing is required only by MySQL.
DELIMITER $$
CREATE PROCEDURE get_customers()
BEGIN
  SELECT * FROM customers;
  ... -- more statements terminated by ';'.
END$$
DELIMITER ;

-- Somewhere else.
CALL get_clients();

-- Other related.
DROP PROCEDURE get_customers; -- Or DROP PROCEDURE IF EXISTS get_customers;
```

## Trigger/Event

## Transakcja

## 