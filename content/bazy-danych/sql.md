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

**Updateable view**: rodzaj widoku który może być wykorzystywany do aktualizowania/kasowania
danych. Używamy tego samego syntaxu co normalnie, tylko zamiast nazwy tabeli dajemy nazwę view.
View jest updateowalny jeżeli bazuje tylko na jednej tabeli i nie zawiera:
- aggregate functions (`MIN`, `MAX`, ...),
- subqueriesów,
- `GROUP BY`/`HAVING`,
- `DISTINCT`,
- `UNION`,
- (i inne niszowe warunki).

Po co:
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
DELIMITER $$ -- This delimiter thing is required only by MySQL.
CREATE PROCEDURE get_customers()
BEGIN
  SELECT * FROM customers;
  ... -- more statements terminated by ';'.
END$$
DELIMITER ;

-- Somewhere else.
CALL get_clients();

-- Other related.
SHOW PROCEDURE STATUS;
DROP PROCEDURE get_customers; -- Or DROP PROCEDURE IF EXISTS get_customers;
```

## Trigger

Zbiór komend SQL które odpalają się automatycznie przed/po zmianie (`INSERT`, `UPDATE`, `DElETE`) w danej tabeli.

Triggery które modyfikują tabele mogą wywołać kaskadę innych triggerów (i przy tym nieskończoną rekurencję). 

Po co:
- audyt zmian,
- ~~data integrity~~,
- ~~formatowanie danych przed dodaniem~~.

```sql
DELIMITER $$ -- This delimiter thing is required only by MySQL.
CREATE TRIGGER orders_after_insert
  AFTER INSERT ON orders -- [BEFORE, AFTER] [INSERT, UPDATE, DELETE] ON table 
  FOR EACH ROW -- [FOR EACH ROW, FOR EACH STATEMENT]. Statement means "once per command".
BEGIN
  INSERT INTO orders_audit
  VALUES (NEW.customer_id, NEW.total, ...); -- NEW, OLD: refer to new/old version of the row.
END
DELIMITER ;

-- Other related.
SHOW TRIGGERS;
DROP TRIGGER IF EXISTS orders_after_insert;
```

## Event

Zbiór komend SQL które odpalają się automatycznie co daną jednostkę czasu.

```sql
DELIMITER $$ -- This delimiter thing is required only by MySQL.
CREATE EVENT IF NOT EXISTS yearly_delete_stale_audit_rows
ON SCHEDULE EVERY 1 YEAR
DO BEGIN
  DELETE FROM orders_audit
  WHERE action_date < NOW() - INTERVAL 1 YEAR;
END $$
DELIMITER ;

-- Other related.
SHOW EVENTS;
DROP EVENT IF EXISTS yearly_delete_stale_audit_rows;
ALTER EVENT yearly_delete_stale_audit_rows DISABLE; -- Or ENABLE.
```

## Transakcja

Zbiór komend SQL które stanowią pojedynczy Unit of Work.
Albo wszystkie zostaną zakończone poprawnie, albo żadne z nich.

Przykład: Tabela z pieniędzmi. Jeżeli podczas przelewu pobieramy z jednego konta
i dodajemy na drugie, to jeżeli jedna z tych operacji sfailuje, to druga powinna
zostać odrzucona.

```sql
START TRANSACTION;
  INSERT INTO orders (product_id, quantity)
  VALUES (99, 3);

  UPDATE products
  SET quantity = quantity - 3
  WHERE product_id = 99;
COMMIT; -- Or ROLLBACK.

-- Other related.
SET [SESSION, GLOBAL] TRANSACTION ISOLATION LEVEL [READ UNCOMMITED,READ COMMITED,REPEATABLE READ,SERIALIZABLE];
-- Typically used before START TRANSACTION. Sets the isolation level.
--   *None*: for the next transaction.
--   SESSION: for a single connection. 
--   GLOBAL: for all new transactions in all sessions.
```

### ACID
- **Atomicity**
  - Każda transakcja to pojedynczy UoW, bez względu na to ile zawiera komend.
  - Albo wszystkie zostaną wykonane i zacommitowane, albo wszystkie zostaną zrollbackowane.
- **Consistency**
  - Baza danych jest zawsze w poprawnym stanie.
- **Isolation** 
  - Transakcje które modyfikują te same wiersze nie wpływają na siebie nawzajem, bo wiersze są lockowane.
  - Jeżeli kilka będzie starało się zmodyfikować to samo, zrobią to po kolei. 
- **Durability** 
  - Dane po zacommitowanej transakcji zostają zapisane na stałe. 
  - Jeżeli np. wysiądzie prąd, po ponownym uruchomieniu db dane nie zostaną stracone.

### Isolation levels
- `READ UNCOMMITED`
  - Jest w stanie odczytać dane które zostały zmienione przez inną transakcję, nawet jeżeli nie zostały jeszcze zacommitowane.
- `READ COMMITED`
  - Jest w stanie odczytać dane które zostały zacommitowane przez inną transakcję w trakcie działania naszej transakcji.
  - Rozwiązuje problem **dirty readów**, tzn. faktu że `COMMIT` może się nigdy nie wydarzyć - transakcja może zostać np. `ROLLBACK`'nięta, a my pracowalibyśmy na danych które nigdy nie trafiły do bazy danych.
- `REPEATABLE READ`: 
  - W obrębie jednej transakcji dane pozostaną niezmienne, nawet jeżeli inna transakcja zmodyfikuje je w tym czasie.
  - Rozwiązuje problem **Non-repeatable readów**, tzn. sytuacji w której nasza transakcja zawiera np. dwa `SELECT`'y wyciągające ten sam wiersz, to jeżeli w czasie między nimi jakaś inna transakcja zupdateowałaby ten wiersz (czyli `UPDATE`), to oba zwróciłyby różną wersję tego samego wiersza.
- `SERIALIZABLE`: 
  - Tak naprawdę wyłączenie jakiejkolwiek współbieżności; transakcje będą wykonywane jedna po drugiej.
  - Rozwiązuje problem **Phantom readów**, tzn. sytuacji w której nasza transakcja zawierałaby np. dwa `SELECT`'y wyciągające wiele wierszy, to jeżeli w czasie między nimi jakaś inna transakcja zupdateowałaby tą tabelę tak żeby więcej/mniej wierszy matchowało naszego `SELECT`a (czyli `INSERT` lub `DELETE`), to oba zwróciłyby różną ilość wierszy.
 
|                                   | Dirty reads | Non-repeatable reads | Phantom reads |
| --------------------------------- | ----------- | -------------------- | ------------- |
| `READ UNCOMMITED`                 |             |                      |               |
| `READ COMMITED`                   | ✓          |                      |               |
| `REPEATABLE READ` (MySQL default) | ✓          | ✓                    |               |
| `SERIALIZABLE`                    | ✓          | ✓                    | ✓            |

"✓" znaczy że dany isolation level (wiersz) zapobiega danemu problemowi (kolumna).

Im bardziej podnosimy isolation level (im niższy wiersz), tym bardziej tracimy performance bazy danych,
bo będzie coraz więcej locków.