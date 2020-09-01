# Оконные функции

Мы создадим тестовую базу с продажами горячительных напитков laugh, и покажем на ней основные возможности оконных функций.

1. Создаем базу и и добавляем тестовые данные

```sql
USE [master];
GO

IF DB_ID('sbase') IS NOT NULL 
DROP DATABASE sbase;

CREATE DATABASE sbase;
GO

USE sbase;
```
### Признаки:
* id - номер заказа,
* product - товар,
* qty - количество,
* cost - цена

```sql
CREATE TABLE sales(
       id SMALLINT,
       product VARCHAR(MAX),
       qty SMALLINT,
       cost SMALLINT);

INSERT INTO sales 
VALUES       (1,        'wine',        10,        30)
            ,(1,    'beer',        5,        15)
            ,(1,    'cognac',    4,        50)
            ,(2,    'cognac',    8,        40)
            ,(2,    'vodka',    2,        30)
            ,(3,    'beer',        15,        12)
            ,(3,    'cognac',    12,        46)
            ,(3,    'vodka',    10,        25)
            ,(4,    'vodka',    1,        30);
```

---

С помощью **PARTITION BY** мы делим результат запроса на окна
Общее количество товара и его стоимость по заказам, детально по товару
```sql
SELECT id AS N'Заказ', product AS N'Товар',
       SUM(qty) OVER (PARTITION BY  id, product) AS N'Общее кол-во в заказе',
       SUM(qty * cost) OVER (PARTITION BY id, product) AS N'Общая с-мость в заказе',
        SUM(qty * cost) OVER () AS N'Общая с-мость'
FROM sales
```

---

С помощью **ORDER BY** мы можем задать сортировку
```sql
SELECT id, product, qty,
       SUM(qty) OVER (PARTITION BY product) AS allQtyProduct,
       SUM(qty) OVER (ORDER BY id DESC) AS allQtyId
FROM sales
```

---
### Описание
* Запрос имеет 9 строк и 4 диапазона
* ROWS отвечпет за строки.
* RANGE отвечпет за диапазон.
* CURRENT ROW - текущая строка или диапазон
```sql
SELECT id, product, qty,
       SUM(qty*cost) OVER (ORDER BY id DESC ROWS CURRENT ROW) AS IdQtyCost
FROM sales
```
```sql
SELECT id, product, qty,
       SUM(qty*cost) OVER (ORDER BY id DESC RANGE CURRENT ROW) AS IdAllQtyCost
FROM sales
``

---
### Описание
* UNBOUNDED PRECEDING - указывает, что надо учитывать все строки/диапазоны с первого и по текущий
* Будет суммировать каждую следующую строку

```sql
SELECT id, product, qty,
       SUM(qty) OVER (ORDER BY id ROWS UNBOUNDED PRECEDING) AS SumQty
FROM sales
```

* Будет суммировать каждый следующий диапазон
```sql
SELECT id, product, qty,
       SUM(qty) OVER (ORDER BY id RANGE UNBOUNDED PRECEDING) AS SumQty
FROM sales
```
---
### Описание
* UNBOUNDED FOLLOWING - указывает, что надо учитывать все строки/диапазоны с текущего и по последний. 
* Может быть указанным только в предложении BETWEEN как конечная точка.
* BETTWEEN - используется для указания границ.
* Будет суммировать каждую следующую строку между указанными значениями
```sql
SELECT id, product, qty,
       SUM(qty) OVER (ORDER BY id ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING) AS SumQty
FROM sales
```

---
### Описание
* Будет суммировать каждый следующий диапазон между указанными значениями

```sql
SELECT id, product, qty,
       SUM(qty) OVER (ORDER BY id RANGE BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING) AS SumQty
FROM sales
```


--PRECEDING - указывает, что нужно учитывать текущую строку и кол-во строк до нее. 
--Не допускается в предложении RANGE.

```sql
SELECT id, product, qty,
       SUM(qty) OVER (ORDER BY id ROWS 1 PRECEDING) AS SumQty
FROM sales
```


--FOLLOWING - указывает, что нужно учитывать диапазон кол-во строк после текущей строчки. 
--Может быть использовано только в предложении BETWEEN. Не допускается в предложении RANGE.
```sql
SELECT id, product, qty,
       SUM(qty) OVER (ORDER BY id ROWS BETWEEN 1 FOLLOWING AND 2 FOLLOWING) AS SumQty
FROM sales
```

---
### Описание

* ROW_NUMBER() - задает каждой строчке окна уникальный, последовательный номер, начиная с единицы.
* Функция "ROW_NUMBER" должна содержать предложение OVER вместе с предложением ORDER BY

```sql
SELECT id, product, qty,
       ROW_NUMBER() OVER (PARTITION BY id ORDER BY id) AS R
FROM sales
```

---
### Описание
* Пример вывода с "постраничной навигацией" используя ROW_NUMBER()
```sql
DECLARE
  @pagenum  AS INT = 3,
  @pagesize AS INT = 2;

WITH C AS
(
  SELECT ROW_NUMBER() OVER( ORDER BY product, id ) AS rownum,
    id, product
  FROM sales
)
SELECT id, product
FROM C
WHERE rownum BETWEEN (@pagenum - 1) * @pagesize + 1 --4
                 AND @pagenum * @pagesize; --6
```

---
### Описание

* RANK() - Возвращает ранг каждой строки в окне. 
* Ранг для каждого уникального значения столбца или столбцов указанных в ORDER BY вычисляется лишь единожды, при первом нахождении оного. 
* По формуле единица плюс количество строк до строки от начала окна.
```sql
SELECT id, product, qty,
       RANK() OVER (ORDER BY id) AS R
FROM sales
```

---
### Описание
* DENSE_RANK() - возвращает ранг строк в окне без прыжков через значения. 
* Ранг строки равен количеству уникальных значений указанных в ODER BY, предшествующих строке, увеличенному на единицу.
```sql
SELECT id, product, qty,
       DENSE_RANK() OVER (ORDER BY id) AS R
FROM sales
```

---
### Описание
* NTILE - Распределяет строки в окне на заданное количество групп. 
* Группы нумеруются, начиная с единицы. 
* Для каждой строки функция NTILE возвращает номер группы, которой принадлежит строка.

```sql
SELECT id, product, qty,
       NTILE(2) OVER (PARTITION BY id ORDER BY id) AS R,
       NTILE(2) OVER (ORDER BY id) AS RO
FROM sales
``
---
### Описание
* LAG            - возвращает предыдущее значение для указанного столбца, сгруппированого по некому столбцу
* LEAD            - возвращает следующее значение для указанного столбца, сгруппированого по некому столбцу
* FIRST_VALUE    - возвращает первое значение для указанного столбца, сгруппированого по некому столбцу
* LAST_VALUE    - возвращает последнее значение для указанного столбца, сгруппированого по некому столбцу

```sql
SELECT id, product, qty,
       LAG(qty) OVER (PARTITION BY id ORDER BY id) AS [prev]
       ,LEAD(qty) OVER (PARTITION BY id ORDER BY id) AS [next]
       ,FIRST_VALUE(qty) OVER (PARTITION BY id ORDER BY id) AS [first]
       ,LAST_VALUE(qty) OVER (PARTITION BY id ORDER BY id) AS [last]
FROM sales
```


