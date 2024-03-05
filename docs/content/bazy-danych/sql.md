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


## Joiny

Służą do łączenia danych znajdujących się w różnych tabelach.

### INNER JOIN

Zwraca wiersze które matchują w obu tabelach.

```sql
SELECT *
FROM table1
JOIN table2 -- or INNER JOIN
    ON table1.column_name = table2.column_name;
```

### LEFT JOIN

Zwraca wszystkie wiersze z lewej tabeli i matchujące wiersze z prawej tabeli.

```sql
SELECT *
FROM table1
LEFT JOIN table2 -- or LEFT OUTER JOIN
    ON table1.column_name = table2.column_name;
```

### RIGHT JOIN

Zwraca wszystkie wiersze z prawej tabeli i matchujące wiersze z lewej tabeli.

```sql
SELECT *
FROM table1
RIGHT JOIN table2 -- or RIGHT OUTER JOIN
    ON table1.column_name = table2.column_name;
```

### FULL JOIN

Zwraca wszystkie wiersze z obu tabel.

```sql
SELECT *
FROM table1
FULL JOIN table2 -- or FULL OUTER JOIN
    ON table1.column_name = table2.column_name;
```

### CROSS JOIN

Zwraca wszystkie kombinacje wierszy z obu tabel.

```sql
SELECT *
FROM table1
CROSS JOIN table2;
```

### NATURAL JOIN

Robi joina automatycznie dobierając kolumny po których joinuje.
Wybierze do tego kolumny o tych samych nazwach.

```sql
SELECT *
FROM table1
NATURAL [INNER, LEFT, RIGHT] JOIN table2; -- INNER by default
```

### Self join

Robi joina z tą samą tabelą.
Trzeba nadać aliasy tabelom żeby odwołać się do ich kolumn.

Użyteczne np. gdy mamy tabele z "pracownikami" i chcemy pomatchować każdego pracownika z jego managerem.

```sql
SELECT *
FROM table1 t1
JOIN table2 t2
    ON t1.column_name = t2.column_name;
```