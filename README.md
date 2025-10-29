# SQL-130

Задание 1
```
SELECT model, speed, hd
FROM PC
WHERE price < 500
```

Задание 2
```
SELECT DISTINCT maker
FROM Product
WHERE type = 'Printer'
```

Задание 3
```
SELECT model, ram, screen
FROM Laptop
WHERE price > 1000;
```

Задание 4
```
SELECT *
FROM Printer
WHERE color = 'y';
```

Задание 5
```
SELECT model, speed, hd
FROM PC
WHERE cd IN ('12x', '24x') AND price < 600;
```

Задание 6
```
SELECT DISTINCT p.maker, l.speed
FROM Product p
JOIN Laptop l ON p.model = l.model
WHERE p.type = 'Laptop' AND l.hd >= 10;
```

Задание 7
```
SELECT p.model, pr.price
FROM Product p
JOIN PC pr ON p.model = pr.model
WHERE p.maker = 'B'
UNION
SELECT p.model, l.price
FROM Product p
JOIN Laptop l ON p.model = l.model
WHERE p.maker = 'B'
UNION
SELECT p.model, pr.price
FROM Product p
JOIN Printer pr ON p.model = pr.model
WHERE p.maker = 'B';
```

Задание 8
```
SELECT DISTINCT p.maker
FROM Product p
WHERE p.type = 'PC'
AND p.maker NOT IN (
    SELECT DISTINCT p2.maker
    FROM Product p2
    WHERE p2.type = 'Laptop'
);
```

Задание 9
```
SELECT DISTINCT p.maker
FROM Product p
JOIN PC pc ON p.model = pc.model
WHERE p.type = 'PC' AND pc.speed >= 450;
```

Задание 10
```
SELECT model, price
FROM Printer
WHERE price = (SELECT MAX(price) FROM Printer);
```

Задание 11
```
SELECT AVG(speed) AS avg_speed
FROM PC;
```

Задание 12
```
SELECT AVG(speed) AS avg_speed
FROM Laptop
WHERE price > 1000;
```

Задание 13
```
SELECT AVG(pc.speed) AS avg_speed
FROM Product p
JOIN PC pc ON p.model = pc.model
WHERE p.maker = 'A';
```

Задание 14
```
SELECT s.class, s.name, c.country
FROM Ships s
JOIN Classes c ON s.class = c.class
WHERE c.numGuns >= 10;
```

Задание 15
```
SELECT hd
FROM PC
GROUP BY hd
HAVING COUNT(hd) >= 2;
```

Задание 16
```

```

Задание 17
```

```

Задание 18
```

```

Задание 19
```

```

Задание 20
```

```

Задание 21
```

```

Задание 22
```

```

Задание 23
```

```

Задание 24
```

```

Задание 25
```

```

Задание 26
```

```

Задание 27
```

```

Задание 28
```

```

Задание 29
```

```

Задание 30
```

```

Задание 31
```

```

Задание 32
```

```

Задание 33
```

```

Задание 50
```
SELECT DISTINCT Outcomes.battle
FROM Outcomes
JOIN Ships ON Outcomes.ship = Ships.name
WHERE Ships.class = 'Kongo';
```

Задание 51
```
WITH AllShips AS (
    SELECT s.name, c.numGuns, c.displacement
    FROM Ships s
    JOIN Classes c ON s.class = c.class
    
    UNION
    
    SELECT o.ship AS name, c.numGuns, c.displacement
    FROM Outcomes o
    LEFT JOIN Ships s ON o.ship = s.name
    JOIN Classes c ON COALESCE(s.class, o.ship) = c.class
)
SELECT a.name
FROM AllShips a
WHERE a.numGuns = (
    SELECT MAX(a2.numGuns)
    FROM AllShips a2
    WHERE a2.displacement = a.displacement
);
```

Задание 52
```

```

Задание 53
```
SELECT CAST(AVG(CAST(numGuns AS numeric)) AS numeric(10,2)) AS avg_numguns
FROM Classes
WHERE type = 'bb'
```

Задание 54
```
WITH AllBattleshipShips AS (
    SELECT DISTINCT Ships.name, Classes.numGuns
    FROM Ships
    JOIN Classes ON Ships.class = Classes.class
    WHERE Classes.type = 'bb'
    
    UNION
    
    SELECT DISTINCT Outcomes.ship, Classes.numGuns
    FROM Outcomes
    JOIN Classes ON Outcomes.ship = Classes.class
    WHERE Classes.type = 'bb'
      AND Outcomes.ship NOT IN (SELECT Ships.name FROM Ships)
    
    UNION
    
    SELECT DISTINCT Outcomes.ship, Classes.numGuns
    FROM Outcomes
    JOIN Ships ON Outcomes.ship = Ships.name
    JOIN Classes ON Ships.class = Classes.class
    WHERE Classes.type = 'bb'
      AND Outcomes.ship NOT IN (SELECT Classes.class FROM Classes WHERE Classes.type = 'bb')
)
SELECT CAST(AVG(CAST(AllBattleshipShips.numGuns AS numeric(10,2))) AS numeric(10,2)) AS avg_numguns
FROM AllBattleshipShips;
```

Задание 55
```
SELECT 
    Classes.class,
    MIN(Ships.launched) AS year
FROM Classes
LEFT JOIN Ships ON Classes.class = Ships.class
GROUP BY Classes.class;
```

Задание 56
```
SELECT 
    Classes.class,
    COUNT(DISTINCT CASE WHEN Outcomes.result = 'sunk' THEN Outcomes.ship END) AS sunk_count
FROM Classes
LEFT JOIN Ships ON Classes.class = Ships.class
LEFT JOIN Outcomes ON Ships.name = Outcomes.ship OR Classes.class = Outcomes.ship
GROUP BY Classes.class;
```

Задание 57
```
WITH AllShips AS (
    -- Все корабли из таблицы Ships
    SELECT class, name AS ship_name
    FROM Ships
    
    UNION
    
    -- Головные корабли из Outcomes, которых нет в Ships
    SELECT Outcomes.ship AS class, Outcomes.ship AS ship_name
    FROM Outcomes
    WHERE Outcomes.ship IN (SELECT class FROM Classes)
      AND Outcomes.ship NOT IN (SELECT name FROM Ships)
),
SunkShips AS (
    -- Потопленные корабли
    SELECT Outcomes.ship
    FROM Outcomes
    WHERE Outcomes.result = 'sunk'
)
SELECT 
    Classes.class,
    COUNT(DISTINCT CASE WHEN SunkShips.ship IS NOT NULL THEN AllShips.ship_name END) AS sunk_count
FROM Classes
JOIN AllShips ON Classes.class = AllShips.class
LEFT JOIN SunkShips ON AllShips.ship_name = SunkShips.ship
GROUP BY Classes.class
HAVING 
    -- Не менее 3 кораблей в базе данных
    COUNT(DISTINCT AllShips.ship_name) >= 3
    -- Имеет потери в виде потопленных кораблей
    AND COUNT(DISTINCT CASE WHEN SunkShips.ship IS NOT NULL THEN AllShips.ship_name END) > 0;
```

Задание 58
```
WITH AllMakers AS (
    SELECT DISTINCT maker FROM Product
),
AllTypes AS (
    SELECT DISTINCT type FROM Product
),
MakerTypes AS (
    SELECT maker, type 
    FROM AllMakers 
    CROSS JOIN AllTypes
),
ModelCounts AS (
    SELECT 
        maker,
        type,
        COUNT(model) AS type_count
    FROM Product
    GROUP BY maker, type
),
TotalModels AS (
    SELECT 
        maker,
        COUNT(model) AS total_count
    FROM Product
    GROUP BY maker
)
SELECT 
    MakerTypes.maker,
    MakerTypes.type,
    CAST(COALESCE(ROUND((ModelCounts.type_count * 100.0 / TotalModels.total_count), 2), 0.00) AS NUMERIC(10,2)) AS percentage
FROM MakerTypes
LEFT JOIN ModelCounts ON MakerTypes.maker = ModelCounts.maker AND MakerTypes.type = ModelCounts.type
JOIN TotalModels ON MakerTypes.maker = TotalModels.maker
ORDER BY MakerTypes.maker, MakerTypes.type;
```

Задание 59
```
WITH PointBalances AS (
    SELECT 
        COALESCE(Income_o.point, Outcome_o.point) AS point,
        COALESCE(Income_o.inc, 0) - COALESCE(Outcome_o.out, 0) AS daily_balance
    FROM Income_o
    FULL JOIN Outcome_o ON Income_o.point = Outcome_o.point AND Income_o.date = Outcome_o.date
)
SELECT 
    point,
    SUM(daily_balance) AS remainder
FROM PointBalances
GROUP BY point;
```

Задание 60
```
WITH PointBalances AS (
    SELECT 
        COALESCE(Income_o.point, Outcome_o.point) AS point,
        COALESCE(Income_o.inc, 0) - COALESCE(Outcome_o.out, 0) AS daily_balance,
        COALESCE(Income_o.date, Outcome_o.date) AS operation_date
    FROM Income_o
    FULL JOIN Outcome_o ON Income_o.point = Outcome_o.point AND Income_o.date = Outcome_o.date
    WHERE COALESCE(Income_o.date, Outcome_o.date) < '2001-04-15'
)
SELECT 
    point,
    SUM(daily_balance) AS remainder
FROM PointBalances
GROUP BY point
HAVING SUM(daily_balance) IS NOT NULL;
```

Задание 61
```
WITH PointBalances AS (
    SELECT 
        COALESCE(Income_o.point, Outcome_o.point) AS point,
        COALESCE(Income_o.inc, 0) - COALESCE(Outcome_o.out, 0) AS daily_balance
    FROM Income_o
    FULL JOIN Outcome_o ON Income_o.point = Outcome_o.point AND Income_o.date = Outcome_o.date
)
SELECT 
    SUM(daily_balance) AS total_remainder
FROM PointBalances;
```

Задание 62
```
SELECT 
    SUM(COALESCE(inc, 0)) - SUM(COALESCE(out, 0)) AS total_remainder
FROM (
    SELECT point, date, inc, 0 AS out
    FROM Income_o
    WHERE date < '2001-04-15'
    UNION ALL
    SELECT point, date, 0 AS inc, out
    FROM Outcome_o
    WHERE date < '2001-04-15'
) AS Combined;
```


Задание 63
```

```


Задание 64
```

```


Задание 65
```

```
