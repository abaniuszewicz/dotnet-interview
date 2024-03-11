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

### Wyciąganie danych

#### SELECT

Zwraca wiersze ze wskazanymi kolumnami.

Może zwrócić pierwszych n rezultatów przez `SELECT TOP n` (nie wszystkie dbms wspierają; odpowiednik MYSQL: `LIMIT`).

```sql
SELECT column1, column2 -- or * if all
FROM table1;
```

#### WHERE

Określa kryteria które musi spełnić wiersz żeby został uwzględniony w wyniku.

```sql
SELECT *
FROM table1
WHERE age > 18;
```

#### AND, OR, NOT

Operatory używane w `WHERE`.

```sql
SELECT *
FROM table1
WHERE age > 18 AND (country = 'Poland' OR country = 'Germany'); -- WHERE NOT country = 'USA';
```

#### IN

Skrót dla wielu `OR`-ów.

```sql
SELECT *
FROM table1
WHERE age > 18 AND country IN ('Poland', 'Germany');
```

#### BETWEEN

Matchowanie danego przedziału, którym mogą być liczby, tekst oraz daty. Przedział jest inkluzywny.

Dla tekstu zwróci wiersze jeżeli mieszczą się one alfabetycznie w danym przedziale.

```sql
SELECT *
FROM table1
WHERE price BETWEEN 10 AND 20;
```

#### LIKE

Wyszukiwanie po patternie.
- `%`: 0 lub więcej znaków.
- `_`: 1 znak.

Do wyszukania bardziej zaawansowanymi patternami można użyc regexów (MYSQL: `REGEXP`, PostgreSQL: `~`).

```sql
SELECT *
FROM table1
WHERE customer_name LIKE 'a%';
```

#### IS NULL

Sprawdza czy wartość jest nullem. **Nie można użyć do tego operatora `=`.**

```sql
SELECT *
FROM table1
WHERE shipping_date IS NULL;
```

#### ORDER BY

Sortuje rezultaty. Domyślna kolejność ASC.

```sql
SELECT *
FROM table1
ORDER BY shipping_date; -- ORDER BY shipping_date ASC|DESC;
```

### Joiny

Służą do łączenia danych znajdujących się w różnych tabelach.

#### INNER JOIN

Zwraca wiersze które matchują w obu tabelach.

```sql
SELECT *
FROM table1
JOIN table2 -- or INNER JOIN
    ON table1.column_name = table2.column_name;
```

#### LEFT JOIN

Zwraca wszystkie wiersze z lewej tabeli i matchujące wiersze z prawej tabeli.

```sql
SELECT *
FROM table1
LEFT JOIN table2 -- or LEFT OUTER JOIN
    ON table1.column_name = table2.column_name;
```

#### RIGHT JOIN

Zwraca wszystkie wiersze z prawej tabeli i matchujące wiersze z lewej tabeli.

```sql
SELECT *
FROM table1
RIGHT JOIN table2 -- or RIGHT OUTER JOIN
    ON table1.column_name = table2.column_name;
```

#### FULL JOIN

Zwraca wszystkie wiersze z obu tabel.

```sql
SELECT *
FROM table1
FULL JOIN table2 -- or FULL OUTER JOIN
    ON table1.column_name = table2.column_name;
```

#### CROSS JOIN

Zwraca wszystkie kombinacje wierszy z obu tabel.

```sql
SELECT *
FROM table1
CROSS JOIN table2;
```

#### NATURAL JOIN

Robi joina automatycznie dobierając kolumny po których joinuje.
Wybierze do tego kolumny o tych samych nazwach.

```sql
SELECT *
FROM table1
NATURAL [INNER, LEFT, RIGHT] JOIN table2; -- INNER by default
```

#### Self join

Robi joina z tą samą tabelą.
Trzeba nadać aliasy tabelom żeby odwołać się do ich kolumn.

Użyteczne np. gdy mamy tabele z "pracownikami" i chcemy pomatchować każdego pracownika z jego managerem.

```sql
SELECT *
FROM tab t1
JOIN tab t2
    ON t1.column_name = t2.column_name;
```

### Dodawanie, aktualizowanie i kasowanie danych

#### INSERT
 
Dodaje wiersze do tabeli.

```sql
-- Specify values for all columns in the order they appear in the table.
INSERT INTO table1
VALUES (col1_value, val2_value, ...); -- Use DEFAULT placeholder to fallback to default value.

-- Specify values only for columns that you care about in any order.
INSERT INTO table1 (col1_name,  col2_name,  col_4_name)
VALUES             (col1_value, col2_value, col4_value);

-- Insert multiple rows.

INSERT INTO table1 (col1_name,    col2_name)
VALUES             (col1_value_a, col2_value_a),
                   (col1_value_b, col2_value_b),
                   ...;
```

#### UPDATE

Aktualizuje wiersze w tabeli.

```sql
UPDATE table1
SET col1_name = col1_value, col2_name = col2_value
WHERE ...;
```

#### DELETE

Kasuje wiersze w tabeli.

```sql
DELETE FROM table1
WHERE ...;
```

### Grupowanie