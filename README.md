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
WITH uniq AS (
  SELECT DISTINCT model, speed, ram
  FROM PC
)
SELECT u1.model AS model_high,
       u2.model AS model_low,
       u1.speed,
       u1.ram
FROM uniq u1
JOIN uniq u2
  ON u1.speed = u2.speed
 AND u1.ram   = u2.ram
 AND u1.model > u2.model
ORDER BY u1.model DESC, u2.model ASC;
```

Задание 17
```
SELECT DISTINCT p.type, l.model, l.speed
FROM Laptop l
JOIN Product p ON p.model = l.model
WHERE p.type = 'Laptop'
  AND l.speed < (SELECT MIN(speed) FROM PC);
```

Задание 18
```
SELECT p.maker, MIN(pr.price) AS price
FROM Product p
JOIN Printer pr ON p.model = pr.model
WHERE p.type = 'Printer'
  AND pr.color = 'y'
GROUP BY p.maker
HAVING MIN(pr.price) = (SELECT MIN(price) FROM Printer WHERE color = 'y');
```

Задание 19
```
SELECT p.maker, AVG(l.screen) AS avg_screen
FROM Product p
JOIN Laptop l ON p.model = l.model
GROUP BY p.maker;
```

Задание 20
```
ошибка в задании
```

Задание 21
```
SELECT p.maker, MAX(pc.price) AS max_price
FROM Product p
JOIN PC pc ON p.model = pc.model
WHERE p.type = 'PC'
GROUP BY p.maker
ORDER BY p.maker;
```

Задание 22
```
SELECT speed, AVG(price) AS avg_price
FROM PC
WHERE speed > 600
GROUP BY speed
ORDER BY speed;
```

Задание 23
```
SELECT DISTINCT p.maker
FROM Product p
WHERE p.maker IN (
    SELECT p1.maker
    FROM Product p1
    JOIN PC pc ON p1.model = pc.model
    WHERE p1.type = 'PC' AND pc.speed >= 750
)
AND p.maker IN (
    SELECT p2.maker
    FROM Product p2
    JOIN Laptop l ON p2.model = l.model
    WHERE p2.type = 'Laptop' AND l.speed >= 750
);
```

Задание 24
```
SELECT model
FROM PC
WHERE price = (SELECT MAX(price) FROM (
    SELECT price FROM PC
    UNION ALL
    SELECT price FROM Laptop
    UNION ALL
    SELECT price FROM Printer
) AS all_products)
UNION
SELECT model
FROM Laptop
WHERE price = (SELECT MAX(price) FROM (
    SELECT price FROM PC
    UNION ALL
    SELECT price FROM Laptop
    UNION ALL
    SELECT price FROM Printer
) AS all_products)
UNION
SELECT model
FROM Printer
WHERE price = (SELECT MAX(price) FROM (
    SELECT price FROM PC
    UNION ALL
    SELECT price FROM Laptop
    UNION ALL
    SELECT price FROM Printer
) AS all_products);
```

Задание 25
```
SELECT DISTINCT p.maker
FROM Product p
JOIN PC pc ON p.model = pc.model
WHERE p.type = 'PC'
  AND pc.ram = (SELECT MIN(ram) FROM PC)
  AND pc.speed = (
        SELECT MAX(speed)
        FROM PC
        WHERE ram = (SELECT MIN(ram) FROM PC)
  )
  AND p.maker IN (
        SELECT DISTINCT maker
        FROM Product
        WHERE type = 'Printer'
  );
```

Задание 26
```
SELECT AVG(price) AS avg_price
FROM (
    SELECT price FROM PC
    WHERE model IN (SELECT model FROM Product WHERE maker = 'A')
    UNION ALL
    SELECT price FROM Laptop
    WHERE model IN (SELECT model FROM Product WHERE maker = 'A')
) AS all_products;
```

Задание 27
```
SELECT p.maker, AVG(pc.hd) AS avg_hd
FROM Product p
JOIN PC pc ON p.model = pc.model
WHERE p.type = 'PC'
  AND p.maker IN (
      SELECT DISTINCT maker
      FROM Product
      WHERE type = 'Printer'
  )
GROUP BY p.maker
ORDER BY p.maker;
```

Задание 28
```
SELECT COUNT(*) AS num_makers
FROM (
    SELECT maker
    FROM Product
    GROUP BY maker
    HAVING COUNT(model) = 1
) AS t;
```

Задание 29
```
SELECT 
    COALESCE(i.point, o.point) AS point,
    COALESCE(i.date, o.date) AS date,
    i.inc,
    o.out
FROM Income_o i
FULL OUTER JOIN Outcome_o o
    ON i.point = o.point AND i.date = o.date
ORDER BY point, date;
```

Задание 30
```
SELECT 
    COALESCE(i.point, o.point) AS point,
    COALESCE(i.date, o.date) AS date,
    o.total_out AS out,
    i.total_inc AS inc
FROM
    (SELECT point, date, SUM(inc) AS total_inc
     FROM Income
     GROUP BY point, date) i
FULL OUTER JOIN
    (SELECT point, date, SUM(out) AS total_out
     FROM Outcome
     GROUP BY point, date) o
ON i.point = o.point AND i.date = o.date
ORDER BY point, date;
```

Задание 31
```
SELECT class, country
FROM Classes
WHERE bore >= 16;
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
WITH DailyIncome AS (
    SELECT point, date, SUM(inc) AS inc
    FROM Income
    GROUP BY point, date
),
DailyOutcome AS (
    SELECT point, date, SUM(out) AS out
    FROM Outcome
    GROUP BY point, date
),
AllOperations AS (
    -- Приходы без расходов
    SELECT 
        DailyIncome.point,
        DailyIncome.date,
        'inc' AS operation_type,
        DailyIncome.inc AS amount
    FROM DailyIncome
    LEFT JOIN DailyOutcome ON DailyIncome.point = DailyOutcome.point AND DailyIncome.date = DailyOutcome.date
    WHERE DailyOutcome.point IS NULL
    
    UNION ALL
    
    -- Расходы без приходов
    SELECT 
        DailyOutcome.point,
        DailyOutcome.date,
        'out' AS operation_type,
        DailyOutcome.out AS amount
    FROM DailyOutcome
    LEFT JOIN DailyIncome ON DailyOutcome.point = DailyIncome.point AND DailyOutcome.date = DailyIncome.date
    WHERE DailyIncome.point IS NULL
)
SELECT *
FROM AllOperations
ORDER BY point, date
```

Задание 65
```
WITH SortedMakers AS (
    SELECT 
        maker,
        type,
        ROW_NUMBER() OVER (
            PARTITION BY maker 
            ORDER BY 
                maker,
                CASE type 
                    WHEN 'PC' THEN 1
                    WHEN 'Laptop' THEN 2
                    WHEN 'Printer' THEN 3
                END
        ) as rn
    FROM (
        SELECT DISTINCT maker, type
        FROM Product
    ) AS UniquePairs
),
NumberedMakers AS (
    SELECT 
        ROW_NUMBER() OVER (
            ORDER BY 
                maker,
                CASE type 
                    WHEN 'PC' THEN 1
                    WHEN 'Laptop' THEN 2
                    WHEN 'Printer' THEN 3
                END
        ) as num,
        CASE WHEN rn = 1 THEN maker ELSE '' END AS maker,
        type
    FROM SortedMakers
)
SELECT num, maker, type
FROM NumberedMakers
ORDER BY num;
```

Задание 66
```
WITH Dates AS (
    SELECT CAST('2003-04-01' AS datetime) AS date
    UNION ALL
    SELECT DATEADD(day, 1, date)
    FROM Dates
    WHERE date < '2003-04-07'
),
FlightsFromRostov AS (
    SELECT 
        P.date,
        COUNT(DISTINCT P.trip_no) as flight_count
    FROM Pass_in_trip P
    JOIN Trip T ON P.trip_no = T.trip_no
    WHERE T.town_from = 'Rostov'
      AND P.date BETWEEN '2003-04-01' AND '2003-04-07'
    GROUP BY P.date
)
SELECT 
    D.date,
    COALESCE(F.flight_count, 0) as flight_count
FROM Dates D
LEFT JOIN FlightsFromRostov F ON D.date = F.date;
```

Задание 67
```
WITH RouteFlights AS (
    SELECT 
        town_from,
        town_to,
        COUNT(*) as flight_count
    FROM Trip
    GROUP BY town_from, town_to
),
MaxFlights AS (
    SELECT MAX(flight_count) as max_count
    FROM RouteFlights
)
SELECT COUNT(*) as route_count
FROM RouteFlights
WHERE flight_count = (SELECT max_count FROM MaxFlights);
```

Задание 68
```
WITH RouteFlights AS (
    SELECT 
        CASE WHEN town_from < town_to THEN town_from ELSE town_to END as city1,
        CASE WHEN town_from < town_to THEN town_to ELSE town_from END as city2,
        COUNT(*) as flight_count
    FROM Trip
    GROUP BY 
        CASE WHEN town_from < town_to THEN town_from ELSE town_to END,
        CASE WHEN town_from < town_to THEN town_to ELSE town_from END
),
MaxFlights AS (
    SELECT MAX(flight_count) as max_count
    FROM RouteFlights
)
SELECT COUNT(*) as route_count
FROM RouteFlights
WHERE flight_count = (SELECT max_count FROM MaxFlights);
```

Задание 69
```
WITH AllOperations AS (
    SELECT 
        point,
        date,
        inc AS income,
        0 AS outcome
    FROM Income
    UNION ALL
    SELECT 
        point,
        date,
        0 AS income,
        out AS outcome
    FROM Outcome
),
DailyBalances AS (
    SELECT 
        point,
        date,
        SUM(income) - SUM(outcome) AS daily_balance
    FROM AllOperations
    GROUP BY point, date
),
CumulativeBalances AS (
    SELECT 
        point,
        date,
        SUM(daily_balance) OVER (
            PARTITION BY point 
            ORDER BY date 
            ROWS UNBOUNDED PRECEDING
        ) AS cumulative_balance
    FROM DailyBalances
)
SELECT 
    point,
    CONVERT(varchar, date, 103) AS day,
    cumulative_balance AS balance
FROM CumulativeBalances
ORDER BY point, date;
```

Задание 70
```
WITH BattleShips AS (
    SELECT 
        Outcomes.battle,
        COALESCE(Ships.class, Outcomes.ship) AS ship_class,
        Classes.country
    FROM Outcomes
    LEFT JOIN Ships ON Outcomes.ship = Ships.name
    LEFT JOIN Classes ON COALESCE(Ships.class, Outcomes.ship) = Classes.class
    WHERE Outcomes.battle IS NOT NULL
),
CountryBattleCounts AS (
    SELECT 
        battle,
        country,
        COUNT(*) AS ship_count
    FROM BattleShips
    WHERE country IS NOT NULL
    GROUP BY battle, country
    HAVING COUNT(*) >= 3
)
SELECT DISTINCT battle
FROM CountryBattleCounts;
```

Задание 71
```
SELECT DISTINCT maker
FROM Product P1
WHERE type = 'PC'
  AND NOT EXISTS (
    SELECT model
    FROM Product P2
    WHERE P2.maker = P1.maker 
      AND P2.type = 'PC'
      AND P2.model NOT IN (SELECT model FROM PC)
);
```

Задание 72
```
WITH PassengerCompanyStats AS (
    SELECT 
        P.ID_psg,
        COUNT(*) AS flight_count,
        COUNT(DISTINCT T.ID_comp) AS company_count
    FROM Pass_in_trip PT
    JOIN Passenger P ON PT.ID_psg = P.ID_psg
    JOIN Trip T ON PT.trip_no = T.trip_no
    GROUP BY P.ID_psg
),
SingleCompanyPassengers AS (
    SELECT 
        ID_psg,
        flight_count
    FROM PassengerCompanyStats
    WHERE company_count = 1
),
MaxFlights AS (
    SELECT MAX(flight_count) AS max_flights
    FROM SingleCompanyPassengers
)
SELECT 
    P.name,
    SCP.flight_count
FROM SingleCompanyPassengers SCP
JOIN Passenger P ON SCP.ID_psg = P.ID_psg
WHERE SCP.flight_count = (SELECT max_flights FROM MaxFlights);
```

Задание 73
```
WITH AllCountries AS (
    SELECT DISTINCT country
    FROM Classes
),
AllBattles AS (
    SELECT name
    FROM Battles
),
AllCombinations AS (
    SELECT 
        AC.country,
        AB.name AS battle
    FROM AllCountries AC
    CROSS JOIN AllBattles AB
),
ParticipatingCountries AS (
    SELECT DISTINCT
        C.country,
        O.battle
    FROM Outcomes O
    LEFT JOIN Ships S ON O.ship = S.name
    LEFT JOIN Classes C ON COALESCE(S.class, O.ship) = C.class
    WHERE O.battle IS NOT NULL
      AND C.country IS NOT NULL
)
SELECT 
    AC.country,
    AC.battle
FROM AllCombinations AC
LEFT JOIN ParticipatingCountries PC ON AC.country = PC.country AND AC.battle = PC.battle
WHERE PC.country IS NULL
ORDER BY AC.country, AC.battle;
```

Задание 74
```
WITH RussianClasses AS (
    SELECT country, class
    FROM Classes
    WHERE country = 'Russia'
)
SELECT country, class
FROM RussianClasses

UNION ALL

SELECT country, class
FROM Classes
WHERE NOT EXISTS (SELECT 1 FROM RussianClasses)
    AND country IS NOT NULL;
```

Задание 75
```
WITH LaptopPrices AS (
    SELECT P.maker, L.price
    FROM Laptop L
    JOIN Product P ON L.model = P.model
    WHERE L.price IS NOT NULL
),
PCPrices AS (
    SELECT P.maker, PC.price
    FROM PC
    JOIN Product P ON PC.model = P.model
    WHERE PC.price IS NOT NULL
),
PrinterPrices AS (
    SELECT P.maker, Pr.price
    FROM Printer Pr
    JOIN Product P ON Pr.model = P.model
    WHERE Pr.price IS NOT NULL
),
AllMakers AS (
    SELECT DISTINCT maker
    FROM Product
    WHERE maker IN (
        SELECT maker FROM LaptopPrices
        UNION
        SELECT maker FROM PCPrices
        UNION
        SELECT maker FROM PrinterPrices
    )
)
SELECT 
    AM.maker,
    MAX(L.price) AS max_laptop,
    MAX(PC.price) AS max_pc,
    MAX(Pr.price) AS max_printer
FROM AllMakers AM
LEFT JOIN LaptopPrices L ON AM.maker = L.maker
LEFT JOIN PCPrices PC ON AM.maker = PC.maker
LEFT JOIN PrinterPrices Pr ON AM.maker = Pr.maker
GROUP BY AM.maker;
```

Задание 76
```
WITH PassengerSeatStats AS (
    SELECT 
        Passenger.ID_psg,
        Passenger.name
    FROM Passenger
    JOIN Pass_in_trip ON Passenger.ID_psg = Pass_in_trip.ID_psg
    GROUP BY Passenger.ID_psg, Passenger.name
    HAVING COUNT(*) = COUNT(DISTINCT Pass_in_trip.place)
),
FlightTimes AS (
    SELECT 
        Pass_in_trip.ID_psg,
        CASE 
            WHEN Trip.time_out <= Trip.time_in THEN 
                DATEDIFF(minute, Trip.time_out, Trip.time_in)
            ELSE 
                DATEDIFF(minute, Trip.time_out, DATEADD(day, 1, Trip.time_in))
        END AS flight_minutes
    FROM Pass_in_trip
    JOIN Trip ON Pass_in_trip.trip_no = Trip.trip_no
    WHERE Pass_in_trip.ID_psg IN (SELECT ID_psg FROM PassengerSeatStats)
)
SELECT 
    PassengerSeatStats.name,
    SUM(FlightTimes.flight_minutes) AS total_minutes
FROM PassengerSeatStats
JOIN FlightTimes ON PassengerSeatStats.ID_psg = FlightTimes.ID_psg
GROUP BY PassengerSeatStats.ID_psg, PassengerSeatStats.name;
```

Задание 77
```
WITH RostovFlights AS (
    SELECT 
        Pass_in_trip.date,
        COUNT(DISTINCT Pass_in_trip.trip_no) AS flight_count
    FROM Pass_in_trip
    JOIN Trip ON Pass_in_trip.trip_no = Trip.trip_no
    WHERE Trip.town_from = 'Rostov'
    GROUP BY Pass_in_trip.date
),
MaxFlights AS (
    SELECT MAX(flight_count) AS max_count
    FROM RostovFlights
)
SELECT 
    RostovFlights.flight_count,
    RostovFlights.date
FROM RostovFlights
WHERE RostovFlights.flight_count = (SELECT max_count FROM MaxFlights);
```

Задание 78
```
SELECT
    Battles.name AS battle,
    CONVERT(varchar, DATEADD(day, 1, EOMONTH(Battles.date, -1)), 120) AS first_day,
    CONVERT(varchar, EOMONTH(Battles.date), 120) AS last_day
FROM Battles;
```

Задание 79
```
WITH FlightDurations AS (
    SELECT 
        Passenger.ID_psg,
        Passenger.name,
        SUM(
            CASE 
                WHEN Trip.time_out <= Trip.time_in THEN 
                    DATEDIFF(minute, Trip.time_out, Trip.time_in)
                ELSE 
                    DATEDIFF(minute, Trip.time_out, DATEADD(day, 1, Trip.time_in))
            END
        ) AS total_minutes
    FROM Passenger
    JOIN Pass_in_trip ON Passenger.ID_psg = Pass_in_trip.ID_psg
    JOIN Trip ON Pass_in_trip.trip_no = Trip.trip_no
    GROUP BY Passenger.ID_psg, Passenger.name
),
MaxFlightTime AS (
    SELECT MAX(total_minutes) AS max_minutes
    FROM FlightDurations
)
SELECT 
    FlightDurations.name,
    FlightDurations.total_minutes
FROM FlightDurations
WHERE FlightDurations.total_minutes = (SELECT max_minutes FROM MaxFlightTime);
```

Задание 80
```
SELECT DISTINCT Product.maker
FROM Product
WHERE Product.type IN ('PC', 'Laptop', 'Printer')
  AND Product.maker NOT IN (
      SELECT Product.maker
      FROM Product
      LEFT JOIN PC ON Product.model = PC.model
      WHERE Product.type = 'PC'
        AND PC.model IS NULL
  )
```

Задание 81
```
WITH MonthlyOutcome AS (
    SELECT 
        YEAR(date) AS year,
        MONTH(date) AS month,
        SUM(out) AS total_out
    FROM Outcome
    GROUP BY YEAR(date), MONTH(date)
),
MaxOutcome AS (
    SELECT MAX(total_out) AS max_total
    FROM MonthlyOutcome
)
SELECT Outcome.*
FROM Outcome
JOIN MonthlyOutcome ON YEAR(Outcome.date) = MonthlyOutcome.year 
                    AND MONTH(Outcome.date) = MonthlyOutcome.month
WHERE MonthlyOutcome.total_out = (SELECT max_total FROM MaxOutcome)
```

Задание 82
```
WITH NumberedPC AS (
    SELECT code, price, 
           ROW_NUMBER() OVER (ORDER BY code) AS rn
    FROM PC
)
SELECT 
    n1.code AS first_code,
    AVG(n2.price) AS avg_price
FROM NumberedPC n1
JOIN NumberedPC n2 ON n2.rn BETWEEN n1.rn AND n1.rn + 5
GROUP BY n1.code, n1.rn
HAVING COUNT(n2.code) = 6
ORDER BY n1.rn;
```

Задание 83
```
SELECT Ships.name
FROM Ships
JOIN Classes ON Ships.class = Classes.class
WHERE 
    CASE WHEN Classes.numGuns = 8 THEN 1 ELSE 0 END +
    CASE WHEN Classes.bore = 15 THEN 1 ELSE 0 END +
    CASE WHEN Classes.displacement = 32000 THEN 1 ELSE 0 END +
    CASE WHEN Classes.type = 'bb' THEN 1 ELSE 0 END +
    CASE WHEN Ships.launched = 1915 THEN 1 ELSE 0 END +
    CASE WHEN Ships.class = 'Kongo' THEN 1 ELSE 0 END +
    CASE WHEN Classes.country = 'USA' THEN 1 ELSE 0 END >= 4;
```

Задание 84
```
WITH AprilDecades AS (
    SELECT 
        Company.ID_comp,
        Company.name,
        CASE 
            WHEN DAY(Pass_in_trip.date) BETWEEN 1 AND 10 THEN 1
            WHEN DAY(Pass_in_trip.date) BETWEEN 11 AND 20 THEN 2
            ELSE 3
        END AS decade,
        COUNT(*) AS passenger_count
    FROM Company
    JOIN Trip ON Company.ID_comp = Trip.ID_comp
    JOIN Pass_in_trip ON Trip.trip_no = Pass_in_trip.trip_no
    WHERE YEAR(Pass_in_trip.date) = 2003 
        AND MONTH(Pass_in_trip.date) = 4
    GROUP BY 
        Company.ID_comp, 
        Company.name,
        CASE 
            WHEN DAY(Pass_in_trip.date) BETWEEN 1 AND 10 THEN 1
            WHEN DAY(Pass_in_trip.date) BETWEEN 11 AND 20 THEN 2
            ELSE 3
        END
)
SELECT 
    name,
    COALESCE(SUM(CASE WHEN decade = 1 THEN passenger_count END), 0) AS decade1,
    COALESCE(SUM(CASE WHEN decade = 2 THEN passenger_count END), 0) AS decade2,
    COALESCE(SUM(CASE WHEN decade = 3 THEN passenger_count END), 0) AS decade3
FROM AprilDecades
GROUP BY ID_comp, name
ORDER BY name;
```

Задание 85
```
SELECT maker
FROM Product
GROUP BY maker
HAVING COUNT(DISTINCT type) = 1 
   AND (
        (MAX(type) = 'Printer') 
        OR 
        (MAX(type) = 'PC' AND COUNT(model) >= 3)
   );
```


Задание 86
```
SELECT 
    maker,
    STRING_AGG(type, '/') WITHIN GROUP (ORDER BY type) AS product_types
FROM (
    SELECT DISTINCT maker, type
    FROM Product
) AS UniqueProducts
GROUP BY maker;
```

Задание 87
```
WITH FirstFlight AS (
    SELECT 
        Pass_in_trip.ID_psg,
        MIN(CAST(Pass_in_trip.date AS datetime) + CAST(Trip.time_out AS datetime)) AS first_flight_datetime
    FROM Pass_in_trip
    JOIN Trip ON Pass_in_trip.trip_no = Trip.trip_no
    GROUP BY Pass_in_trip.ID_psg
),
Hometown AS (
    SELECT 
        Pass_in_trip.ID_psg,
        Trip.town_from AS hometown
    FROM Pass_in_trip
    JOIN Trip ON Pass_in_trip.trip_no = Trip.trip_no
    JOIN FirstFlight ON Pass_in_trip.ID_psg = FirstFlight.ID_psg
    WHERE CAST(Pass_in_trip.date AS datetime) + CAST(Trip.time_out AS datetime) = FirstFlight.first_flight_datetime
),
MoscowArrivals AS (
    SELECT 
        Pass_in_trip.ID_psg,
        COUNT(*) AS moscow_count
    FROM Pass_in_trip
    JOIN Trip ON Pass_in_trip.trip_no = Trip.trip_no
    WHERE Trip.town_to = 'Moscow'
    GROUP BY Pass_in_trip.ID_psg
    HAVING COUNT(*) > 1
)
SELECT 
    Passenger.name,
    MoscowArrivals.moscow_count
FROM MoscowArrivals
JOIN Hometown ON MoscowArrivals.ID_psg = Hometown.ID_psg
JOIN Passenger ON MoscowArrivals.ID_psg = Passenger.ID_psg
WHERE Hometown.hometown <> 'Moscow';
```

Задание 88
```
WITH PassengerCompanyStats AS (
    SELECT 
        Passenger.ID_psg,
        Passenger.name,
        Company.ID_comp,
        Company.name AS company_name,
        COUNT(*) AS flight_count
    FROM Passenger
    JOIN Pass_in_trip ON Passenger.ID_psg = Pass_in_trip.ID_psg
    JOIN Trip ON Pass_in_trip.trip_no = Trip.trip_no
    JOIN Company ON Trip.ID_comp = Company.ID_comp
    GROUP BY Passenger.ID_psg, Passenger.name, Company.ID_comp, Company.name
),
SingleCompanyPassengers AS (
    SELECT 
        ID_psg,
        name,
        company_name,
        flight_count
    FROM PassengerCompanyStats
    WHERE ID_psg IN (
        SELECT ID_psg
        FROM PassengerCompanyStats
        GROUP BY ID_psg
        HAVING COUNT(DISTINCT ID_comp) = 1
    )
),
MaxFlights AS (
    SELECT MAX(flight_count) AS max_flight_count
    FROM SingleCompanyPassengers
)
SELECT 
    SingleCompanyPassengers.name,
    SingleCompanyPassengers.flight_count,
    SingleCompanyPassengers.company_name
FROM SingleCompanyPassengers
WHERE SingleCompanyPassengers.flight_count = (SELECT max_flight_count FROM MaxFlights);
```

Задание 89
```
WITH ModelCounts AS (
    SELECT 
        maker,
        COUNT(model) AS model_count
    FROM Product
    GROUP BY maker
),
MaxCount AS (
    SELECT MAX(model_count) AS max_count
    FROM ModelCounts
),
MinCount AS (
    SELECT MIN(model_count) AS min_count
    FROM ModelCounts
)
SELECT 
    maker,
    model_count
FROM ModelCounts
WHERE model_count = (SELECT max_count FROM MaxCount)
   OR model_count = (SELECT min_count FROM MinCount)
ORDER BY model_count DESC;
```

Задание 90
```
WITH OrderedProducts AS (
    SELECT *,
           ROW_NUMBER() OVER (ORDER BY model) AS asc_rank,
           ROW_NUMBER() OVER (ORDER BY model DESC) AS desc_rank
    FROM Product
)
SELECT maker, model, type
FROM OrderedProducts
WHERE asc_rank > 3 
  AND desc_rank > 3;
```



Задание 128
```
WITH AllData AS (
    SELECT 
        point, date, SUM(out) AS total_out, 'multiple' AS type
    FROM Outcome 
    GROUP BY point, date
    UNION ALL
    SELECT 
        point, date, out AS total_out, 'once' AS type
    FROM Outcome_o
),
PointsWithBothTypes AS (
    SELECT point
    FROM AllData
    GROUP BY point
    HAVING COUNT(DISTINCT type) = 2
),
Comparison AS (
    SELECT 
        a.point,
        a.date,
        SUM(CASE WHEN a.type = 'once' THEN a.total_out ELSE 0 END) AS once_total,
        SUM(CASE WHEN a.type = 'multiple' THEN a.total_out ELSE 0 END) AS multiple_total
    FROM AllData a
    WHERE a.point IN (SELECT point FROM PointsWithBothTypes)
    GROUP BY a.point, a.date
)
SELECT 
    point,
    date,
    CASE 
        WHEN once_total > multiple_total THEN 'once a day'
        WHEN multiple_total > once_total THEN 'more than once a day'
        ELSE 'both'
    END AS leader
FROM Comparison
WHERE once_total > 0 OR multiple_total > 0
ORDER BY point, date
```

Задание 129
```
WITH AllIDs AS (
    SELECT Q_ID FROM utQ
),
SortedIDs AS (
    SELECT Q_ID, 
           LAG(Q_ID) OVER (ORDER BY Q_ID) as prev_id,
           LEAD(Q_ID) OVER (ORDER BY Q_ID) as next_id
    FROM AllIDs
),
Gaps AS (
    -- Пропуски после предыдущего ID
    SELECT prev_id + 1 as gap_start, Q_ID - 1 as gap_end
    FROM SortedIDs
    WHERE prev_id IS NOT NULL AND Q_ID > prev_id + 1
    
    UNION ALL
    
    -- Пропуски перед следующим ID  
    SELECT Q_ID + 1 as gap_start, next_id - 1 as gap_end
    FROM SortedIDs
    WHERE next_id IS NOT NULL AND next_id > Q_ID + 1
),
AllMissingIDs AS (
    SELECT gap_start as missing_id FROM Gaps WHERE gap_start <= gap_end
    UNION ALL
    SELECT gap_end FROM Gaps WHERE gap_start < gap_end
)
SELECT 
    MIN(missing_id) as min_missing,
    MAX(missing_id) as max_missing
FROM AllMissingIDs
```

Задание 130
```
WITH NumberedBattles AS (
    SELECT 
        ROW_NUMBER() OVER (ORDER BY date, name) as rn,
        name,
        date
    FROM Battles
),
TotalCount AS (
    SELECT COUNT(*) as total FROM Battles
),
FirstColCount AS (
    SELECT CEILING(total / 2.0) as cnt FROM TotalCount
)
SELECT 
    CASE WHEN a.rn <= fc.cnt THEN a.rn END as num1,
    CASE WHEN a.rn <= fc.cnt THEN a.name END as name1,
    CASE WHEN a.rn <= fc.cnt THEN a.date END as date1,
    b.rn as num2,
    b.name as name2,
    b.date as date2
FROM NumberedBattles a
CROSS JOIN (SELECT cnt FROM FirstColCount) fc
LEFT JOIN NumberedBattles b ON b.rn = a.rn + fc.cnt
WHERE a.rn <= fc.cnt
ORDER BY a.rn
```
***

# Ивано-романовский SQL

Задание 16
```
SELECT DISTINCT 
    p1.model AS model1,
    p2.model AS model2, 
    p1.speed, 
    p1.ram
FROM PC p1
JOIN PC p2 ON p1.speed = p2.speed AND p1.ram = p2.ram
WHERE p1.model > p2.model;
```

Задание 17
```
SELECT DISTINCT 'Laptop' AS type, model, speed
FROM Laptop
WHERE speed < ALL (SELECT speed FROM PC);
```

Задание 18
```
SELECT DISTINCT Product.maker, Printer.price
FROM Product
JOIN Printer ON Product.model = Printer.model
WHERE Printer.color = 'y'
  AND Printer.price = (SELECT MIN(price) FROM Printer WHERE color = 'y');
```

Задание 19
```
SELECT Product.maker, AVG(Laptop.screen) AS average_screen
FROM Product
JOIN Laptop ON Product.model = Laptop.model
GROUP BY Product.maker;
```

Задание 20
```
SELECT maker, COUNT(model) AS model_count
FROM Product
WHERE type = 'PC'
GROUP BY maker
HAVING COUNT(model) >= 3;
```

Задание 21
```
SELECT Product.maker, MAX(PC.price) AS max_price
FROM Product
JOIN PC ON Product.model = PC.model
GROUP BY Product.maker;
```

Задание 22
```
SELECT speed, AVG(price) AS average_price
FROM PC
WHERE speed > 600
GROUP BY speed;
```

Задание 23
```
SELECT DISTINCT maker
FROM Product
JOIN PC ON Product.model = PC.model
WHERE PC.speed >= 750
AND maker IN (
    SELECT maker
    FROM Product
    JOIN Laptop ON Product.model = Laptop.model
    WHERE Laptop.speed >= 750
);
```

Задание 24
```
SELECT model
FROM (
    SELECT model, price FROM PC
    UNION
    SELECT model, price FROM Laptop
    UNION
    SELECT model, price FROM Printer
) AS all_products
WHERE price = (
    SELECT MAX(price)
    FROM (
        SELECT price FROM PC
        UNION
        SELECT price FROM Laptop
        UNION
        SELECT price FROM Printer
    ) AS all_prices
);
```

Задание 25
```
SELECT DISTINCT maker
FROM Product
JOIN PC ON Product.model = PC.model
WHERE PC.ram = (SELECT MIN(ram) FROM PC)
  AND PC.speed = (
      SELECT MAX(speed) 
      FROM PC 
      WHERE ram = (SELECT MIN(ram) FROM PC)
  )
  AND maker IN (SELECT maker FROM Product WHERE type = 'Printer');
```

Задание 26
```
SELECT AVG(price) AS average_price
FROM (
    SELECT price FROM PC
    JOIN Product ON PC.model = Product.model
    WHERE Product.maker = 'A'
    UNION ALL
    SELECT price FROM Laptop
    JOIN Product ON Laptop.model = Product.model
    WHERE Product.maker = 'A'
) AS all_products;
```

Задание 27
```
SELECT Product.maker, AVG(PC.hd) AS average_hd
FROM Product
JOIN PC ON Product.model = PC.model
WHERE Product.maker IN (SELECT maker FROM Product WHERE type = 'Printer')
GROUP BY Product.maker;
```

Задание 28
```
SELECT COUNT(*) AS maker_count
FROM (
    SELECT maker
    FROM Product
    GROUP BY maker
    HAVING COUNT(model) = 1
) AS single_model_makers;
```

Задание 29
```
SELECT 
    COALESCE(i.point, o.point) AS point,
    COALESCE(i.date, o.date) AS date,
    i.inc,
    o.out
FROM Income_o i
FULL JOIN Outcome_o o ON i.point = o.point AND i.date = o.date;
```

Задание 30
```
SELECT 
    point,
    date,
    SUM(out) AS out,
    SUM(inc) AS inc
FROM (
    SELECT point, date, out, NULL AS inc FROM Outcome
    UNION ALL
    SELECT point, date, NULL AS out, inc FROM Income
) AS combined
GROUP BY point, date;
```

Задание 41
```
SELECT start_pair
FROM Timepair
WHERE id = 4;
```

Задание 42
```
SELECT 
    (end_pair - start_pair) as time
FROM 
    (SELECT 
        (SELECT start_pair FROM Timepair WHERE id = 2) as start_pair,
        (SELECT end_pair FROM Timepair WHERE id = 4) as end_pair
    ) as time_diff;
```

Задание 43
```
SELECT DISTINCT t.last_name
FROM Teacher t
JOIN Schedule sch ON t.id = sch.teacher
JOIN Subject s ON sch.subject = s.id
WHERE s.name = 'Physical Culture'
ORDER BY t.last_name;
```

Задание 44
```
SELECT MAX(
    EXTRACT(YEAR FROM AGE(NOW(), s.birthday))
) as max_year
FROM Student s
JOIN Student_in_class sic ON s.id = sic.student
JOIN Class c ON sic.class = c.id
WHERE c.name LIKE '10%';
```

Задание 45
```
SELECT classroom
FROM Schedule
GROUP BY classroom
HAVING COUNT(*) = (
    SELECT MAX(classroom_count)
    FROM (
        SELECT COUNT(*) as classroom_count
        FROM Schedule
        GROUP BY classroom
    ) as counts
);
```

Задание 46
```
SELECT DISTINCT c.name
FROM Class c
WHERE c.id IN (
    SELECT sch.class
    FROM Schedule sch
    JOIN Teacher t ON sch.teacher = t.id
    WHERE t.last_name = 'Krauze'
);
```

Задание 47
```
SELECT COUNT(*) as count
FROM Schedule sch
JOIN Teacher t ON sch.teacher = t.id
WHERE t.last_name = 'Krauze'
AND sch.date = '2019-08-30';
```

Задание 48
```
SELECT c.name, COUNT(sic.student) as count
FROM Class c
JOIN Student_in_class sic ON c.id = sic.class
GROUP BY c.id, c.name
ORDER BY count DESC;
```

Задание 49
```
SELECT 
    ROUND(
        (COUNT(CASE WHEN c.name = '10 A' THEN 1 END) * 100.0 / COUNT(*)),
        4
    ) as percent
FROM Student_in_class sic
JOIN Class c ON sic.class = c.id;
```

Задание 50
```
SELECT 
    FLOOR(
        (COUNT(CASE WHEN EXTRACT(YEAR FROM birthday) = 2000 THEN 1 END) * 100.0 / COUNT(*))
    ) as percent
FROM Student;
```

Задание 51
```
INSERT INTO Goods (good_id, good_name, type)
SELECT 
    (SELECT COUNT(*) FROM Goods) + 1,
    'Cheese',
    (SELECT good_type_id FROM GoodTypes WHERE good_type_name = 'food');
```

Задание 52
```
INSERT INTO GoodTypes (good_type_id, good_type_name)
VALUES ((SELECT COUNT(*) FROM GoodTypes) + 1, 'auto');
```

Задание 53
```
UPDATE FamilyMembers
SET member_name = 'Andie Anthony'
WHERE member_name = 'Andie Quincey';
```

Задание 54
```
DELETE FROM FamilyMembers
WHERE member_name LIKE '%Quincey';
```

Задание 55
```
DELETE FROM Company
WHERE id IN (
    SELECT company
    FROM Trip
    GROUP BY company
    HAVING COUNT(*) = (
        SELECT MIN(flight_count)
        FROM (
            SELECT COUNT(*) as flight_count
            FROM Trip
            GROUP BY company
        ) as counts
    )
);
```

Задание 56
```
DELETE FROM Trip
WHERE town_from = 'Moscow';
```

Задание 57
```
UPDATE Timepair
SET start_pair = start_pair + INTERVAL '30 minutes',
    end_pair = end_pair + INTERVAL '30 minutes';
```

Задание 58
```
INSERT INTO Reviews (id, reservation_id, rating)
SELECT 
    (SELECT COUNT(*) FROM Reviews) + 1,
    r.id,
    5
FROM Reservations r
JOIN Rooms rm ON r.room_id = rm.id
JOIN Users u ON r.user_id = u.id
WHERE rm.address = '11218, Friel Place, New York'
AND u.name = 'George Clooney';
```

Задание 59
```
SELECT *
FROM Users
WHERE phone_number LIKE '+375%';
```

Задание 60
```
SELECT teacher
FROM Schedule sch
JOIN Class c ON sch.class = c.id
WHERE c.name LIKE '11%'
GROUP BY teacher
HAVING COUNT(DISTINCT c.name) = (
    SELECT COUNT(DISTINCT name)
    FROM Class
    WHERE name LIKE '11%'
);
```

Задание 61
```
SELECT DISTINCT r.*
FROM Rooms r
JOIN Reservations res ON r.id = res.room_id
WHERE (res.start_date <= '2020-03-24' AND res.end_date >= '2020-03-18')
   OR (res.start_date BETWEEN '2020-03-18' AND '2020-03-24')
   OR (res.end_date BETWEEN '2020-03-18' AND '2020-03-24');
```

Задание 62
```
SELECT 
    last_name  '.'  SUBSTRING(first_name FROM 1 FOR 1) || '.' as name
FROM Student
ORDER BY name ASC;
```

Задание 63
```
none
```

Задание 64
```
SELECT 
    EXTRACT(YEAR FROM start_date) as year,
    EXTRACT(MONTH FROM start_date) as month,
    COUNT(*) as amount
FROM Reservations
GROUP BY EXTRACT(YEAR FROM start_date), EXTRACT(MONTH FROM start_date)
HAVING COUNT(*) >= 1
ORDER BY year ASC, month ASC;
```

Задание 65
```
SELECT 
    r.room_id,
    FLOOR(AVG(rev.rating)) as rating
FROM Reservations r
JOIN Reviews rev ON r.id = rev.reservation_id
GROUP BY r.room_id
ORDER BY r.room_id;
```

Задание 66
```
SELECT 
    r.home_type,
    r.address,
    COALESCE(SUM(DATE_PART('day', res.end_date - res.start_date)), 0) as days,
    COALESCE(SUM(res.total), 0) as total_fee
FROM Rooms r
LEFT JOIN Reservations res ON r.id = res.room_id
WHERE r.has_tv = TRUE 
  AND r.has_internet = TRUE 
  AND r.has_kitchen = TRUE 
  AND r.has_air_con = TRUE
GROUP BY r.id, r.home_type, r.address
ORDER BY r.id;
```

Задание 67
```
SELECT 
    TO_CHAR(time_out, 'HH24:MI')  ', '  
    EXTRACT(DAY FROM time_out)  '.'  EXTRACT(MONTH FROM time_out) || 
    ' - ' || 
    TO_CHAR(time_in, 'HH24:MI')  ', '  
    EXTRACT(DAY FROM time_in)  '.'  EXTRACT(MONTH FROM time_in) as flight_time
FROM Trip
ORDER BY id;
```

Задание 68
```
SELECT 
    r.id as room_id,
    u.name as name,
    res.end_date as end_date
FROM Rooms r
JOIN Reservations res ON r.id = res.room_id
JOIN Users u ON res.user_id = u.id
WHERE res.end_date = (
    SELECT MAX(res2.end_date)
    FROM Reservations res2
    WHERE res2.room_id = r.id
)
ORDER BY r.id;
```

Задание 69
```
SELECT 
    r.owner_id as owner_id,
    COALESCE(SUM(res.total), 0) as total_earn
FROM Rooms r
LEFT JOIN Reservations res ON r.id = res.room_id
GROUP BY r.owner_id
ORDER BY r.owner_id;
```

Задание 70
```
SELECT 
    category,
    COUNT(*) as count
FROM (
    SELECT 
        CASE 
            WHEN price <= 100 THEN 'economy'
            WHEN price > 100 AND price < 200 THEN 'comfort'
            WHEN price >= 200 THEN 'premium'
        END as category
    FROM Rooms
) as categorized
GROUP BY category
ORDER BY 
    CASE 
        WHEN category = 'economy' THEN 1
        WHEN category = 'comfort' THEN 2
        WHEN category = 'premium' THEN 3
    END;
```

Задание 71
```
none
```

Задание 72
```
SELECT 
    room_id,
    CEIL(AVG(total / DATEDIFF(end_date, start_date))) as avg_price
FROM Reservations
GROUP BY room_id
HAVING COUNT(*) >= 1
ORDER BY room_id;
```

Задание 73
```
SELECT 
    room_id,
    COUNT(*) as count
FROM Reservations
GROUP BY room_id
HAVING COUNT(*) % 2 = 1
ORDER BY room_id;
```

Задание 74
```
SELECT 
    id,
    CASE 
        WHEN has_internet = TRUE THEN 'YES'
        ELSE 'NO'
    END as has_internet
FROM Rooms
ORDER BY id;
```

Задание 75
```
SELECT 
    last_name,
    first_name,
    birthday
FROM Student
WHERE MONTH(birthday) = 5
ORDER BY last_name, first_name;
```

Задание 76
```
SELECT 
    u.name,
    CASE WHEN EXISTS (SELECT 1 FROM Rooms r WHERE r.owner_id = u.id) THEN 1 ELSE 0 END as is_owner,
    CASE WHEN EXISTS (SELECT 1 FROM Reservations res WHERE res.user_id = u.id) THEN 1 ELSE 0 END as is_tenant
FROM Users u
ORDER BY u.name;
```

Задание 77
```
CREATE VIEW People AS
SELECT first_name, last_name FROM Student
UNION
SELECT first_name, last_name FROM Teacher;
```

Задание 78
```
SELECT *
FROM Users
WHERE email LIKE '%@hotmail.com';
```

Задание 79
```
SELECT 
    id,
    home_type,
    CASE 
        WHEN has_tv = TRUE AND has_internet = TRUE THEN price * 0.9
        ELSE price
    END as price
FROM Rooms
ORDER BY id;
```

Задание 99
```
SELECT 
    SUM(price * items) as income_from_female
FROM Purchases
WHERE user_gender IN ('female', 'f');
```
