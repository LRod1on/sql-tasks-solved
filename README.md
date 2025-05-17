# sql-tasks-solved
Here the one can find SQL tasks solved by me from https://sql-academy.org/ru

## Задание 1

Вывести имена всех людей, которые есть в базе данных авиакомпаний
```sql
SELECT name FROM Passenger
```  

## Задание 2

Вывести названия всеx авиакомпаний
```sql
SELECT name FROM Company
```  
 
## Задание 3

Вывести все рейсы, совершенные из Москвы
```sql
SELECT * FROM Trip
WHERE town_from = "Moscow"
```

## Задание 4

Вывести имена людей, которые заканчиваются на "man"
```sql
SELECT name from Passenger
WHERE name LIKE "%man"
```  

## Задание 5

Вывести количество рейсов, совершенных на TU-134
```sql
SELECT COUNT(plane) as count FROM Trip 
WHERE plane = "TU-134"
```  
 
## Задание 6

Какие компании совершали перелеты на Boeing
```sql
SELECT name FROM Company
	INNER JOIN Trip ON Trip.company = Company.id
WHERE plane = "Boeing"
GROUP BY name
```

## Задание 7

Вывести все названия самолётов, на которых можно улететь в Москву (Moscow)
```sql
SELECT plane FROM Trip
WHERE town_to = 'Moscow'
GROUP BY plane
```  

## Задание 8

В какие города можно улететь из Парижа (Paris) и сколько времени это займёт?
```sql
SELECT town_to, TIMEDIFF(time_in, time_out) as flight_time FROM Trip
WHERE town_from = "Paris"
```  
 
## Задание 9

Какие компании организуют перелеты из Владивостока (Vladivostok)?
```sql
SELECT name FROM Company
    INNER JOIN Trip ON Company.id = Trip.company
WHERE Trip.town_from = "Vladivostok"
```

## Задание 10

Вывести вылеты, совершенные с 10 ч. по 14 ч. 1 января 1900 г.
```sql
SELECT * FROM Trip 
WHERE time_out >= "1900-01-01T11:00:00.000Z" AND time_out <= "1900-01-01T14:00:00.000Z"
ORDER BY time_out ASC 
```  

## Задание 11

Выведите пассажиров с самым длинным ФИО. Пробелы, дефисы и точки считаются частью имени.
```sql
SELECT name FROM Passenger
WHERE LENGTH(name) = (
		SELECT max(LENGTH(name)) FROM Passenger
)
```  
 
## Задание 12

Выведите идентификаторы всех рейсов и количество пассажиров на них. Обратите внимание, что на каких-то рейсах пассажиров может не быть. В этом случае выведите число "0".
```sql
SELECT Trip.id as id, COUNT(passenger) as count FROM Trip
	LEFT JOIN Pass_in_trip ON Trip.id = Pass_in_trip.trip
GROUP BY Trip.id
```

## Задание 13

Вывести имена людей, у которых есть полный тёзка среди пассажиров
```sql
SELECT name FROM Passenger
GROUP BY name 
HAVING COUNT(name) > 1 
```  

## Задание 14

В какие города летал Bruce Willis
```sql
SELECT Trip.town_to FROM Trip
    INNER JOIN Pass_in_trip ON Trip.id = Pass_in_trip.trip
    INNER JOIN Passenger ON Pass_in_trip.passenger = Passenger.id
WHERE Passenger.name = 'Bruce Willis'
```  
 
## Задание 15

Выведите идентификатор пассажира Стив Мартин (Steve Martin) и дату и время его прилёта в Лондон (London)
```sql
SELECT Trip.time_in, Passenger.id FROM Trip
	INNER JOIN Pass_in_trip ON Pass_in_trip.trip = Trip.id
	INNER JOIN Passenger ON Pass_in_trip.passenger = Passenger.id
WHERE Passenger.name = 'Steve Martin' AND Trip.town_to = 'London'
```

## Задание 16

Вывести отсортированный по количеству перелетов (по убыванию) и имени (по возрастанию) список пассажиров, совершивших хотя бы 1 полет.
```sql
SELECT Passenger.name, COUNT(passenger) as count FROM Passenger
    INNER JOIN Pass_in_trip ON Pass_in_trip.passenger = Passenger.id
GROUP BY Passenger.name  
ORDER BY count DESC, name ASC
```


## Задание 17

Определить, сколько потратил в 2005 году каждый из членов семьи. В результирующей выборке не выводите тех членов семьи, которые ничего не потратили.
```sql
SELECT FamilyMembers.member_name,
	FamilyMembers.status,
	SUM(Payments.unit_price * Payments.amount) AS costs
FROM FamilyMembers
	INNER JOIN Payments ON Payments.family_member = FamilyMembers.member_id
WHERE YEAR(Payments.date) = 2005
GROUP BY FamilyMembers.member_name, FamilyMembers.status
```

## Задание 18

Выведите имя самого старшего человека. Если таких несколько, то выведите их всех.
```sql
SELECT member_name FROM FamilyMembers
WHERE birthday = (
		SELECT MIN(birthday)
		FROM FamilyMembers
	)
```

## Задание 19

Определить, кто из членов семьи покупал картошку (potato)
```sql
SELECT FamilyMembers.status FROM FamilyMembers
    INNER JOIN Payments ON FamilyMembers.member_id = Payments.family_member
    INNER JOIN Goods ON Goods.good_id = Payments.good
WHERE Goods.good_name = 'potato'
GROUP BY status
```

## Задание 20

Сколько и кто из семьи потратил на развлечения (entertainment). Вывести статус в семье, имя, сумму
```sql
SELECT FamilyMembers.status, FamilyMembers.member_name, SUM(Payments.unit_price * Payments.amount) as costs FROM FamilyMembers
    INNER JOIN Payments ON Payments.family_member = FamilyMembers.member_id
    INNER JOIN Goods ON Goods.good_id = Payments.good
    INNER JOIN GoodTypes ON GoodTypes.good_type_id = Goods.type
WHERE GoodTypes.good_type_name = 'entertainment'
GROUP BY FamilyMembers.member_name, FamilyMembers.status
```

## Задание 21

Определить товары, которые покупали более 1 раза
```sql
SELECT Goods.good_name FROM Goods
    INNER JOIN Payments ON Payments.good = Goods.good_id
GROUP BY Goods.good_name
HAVING COUNT(Payments.good) > 1
```

## Задание 22

Найти имена всех матерей (mother)
```sql
SELECT member_name FROM FamilyMembers
WHERE status = 'mother'
```

## Задание 23

Найдите самый дорогой деликатес (delicacies) и выведите его цену
```sql
SELECT Goods.good_name, Payments.unit_price FROM Goods
    INNER JOIN Payments ON Payments.good = Goods.good_id
    INNER JOIN GoodTypes ON Goods.type = GoodTypes.good_type_id
WHERE good_type_name = 'delicacies'
LIMIT 1
```

## Задание 24

Определить кто и сколько потратил в июне 2005
```sql
SELECT FamilyMembers.member_name, SUM(Payments.amount * Payments.unit_price) as costs FROM FamilyMembers
    LEFT JOIN Payments ON FamilyMembers.member_id = Payments.family_member
WHERE YEAR(Payments.date) = '2005' AND MONTH(Payments.date) = '06'
GROUP BY FamilyMembers.member_name
```

## Задание 25

Определить, какие товары не покупались в 2005 году
```sql
SELECT DISTINCT good_name FROM Goods
WHERE good_name NOT IN (
		SELECT good_name FROM Payments
			INNER JOIN Goods ON good = good_id
		WHERE date LIKE '2005%')
```

## Задание 26

Определить группы товаров, которые не приобретались в 2005 году
```sql
SELECT DISTINCT good_type_name FROM GoodTypes
    WHERE good_type_name NOT IN (
        SELECT GoodTypes.good_type_name FROM GoodTypes
            INNER JOIN Goods ON GoodTypes.good_type_id = Goods.type
            INNER JOIN Payments ON Goods.good_id = Payments.good
        WHERE YEAR(Payments.date) = 2005)
```

## Задание 27

Узнайте, сколько было потрачено на каждую из групп товаров в 2005 году. Выведите название группы и потраченную на неё сумму. Если потраченная сумма равна нулю, т.е. товары из этой группы не покупались в 2005 году, то не выводите её.
```sql
SELECT good_type_name, SUM(Payments.amount * Payments.unit_price) as costs FROM GoodTypes
    INNER JOIN Goods ON Goods.type = GoodTypes.good_type_id
    INNER JOIN Payments ON Payments.good = Goods.good_id
WHERE YEAR(Payments.date) = 2005
GROUP BY good_type_name
```

## Задание 28

Сколько рейсов совершили авиакомпании из Ростова (Rostov) в Москву (Moscow) ?
```sql
SELECT COUNT(*) as count FROM Trip
WHERE town_from = 'Rostov' AND town_to = 'Moscow'
```

## Задание 29

Выведите имена пассажиров улетевших в Москву (Moscow) на самолете TU-134. В ответе не должно быть дубликатов.
```sql
SELECT DISTINCT Passenger.name FROM Passenger
    INNER JOIN Pass_in_trip ON Passenger.id = Pass_in_trip.passenger
    INNER JOIN Trip ON Trip.id = Pass_in_trip.trip
WHERE Trip.plane = 'TU-134' AND Trip.town_to = 'Moscow'
```

## Задание 30

Выведите нагруженность (число пассажиров) каждого рейса (trip). Результат вывести в отсортированном виде по убыванию нагруженности.
```sql
SELECT COUNT(passenger) as count, trip FROM Pass_in_trip
GROUP BY trip
ORDER BY count DESC
```

## Задание 31

Вывести всех членов семьи с фамилией Quincey.
```sql
SELECT * FROM FamilyMembers
WHERE member_name LIKE '%Quincey'
```

## Задание 32

Вывести средний возраст людей (в годах), хранящихся в базе данных. Результат округлите до целого в меньшую сторону.
```sql
SELECT FLOOR(AVG(TIMESTAMPDIFF(YEAR, birthday, CURRENT_TIMESTAMP))) AS age
FROM FamilyMembers;
```