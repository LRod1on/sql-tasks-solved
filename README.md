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