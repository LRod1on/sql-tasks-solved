# sql-tasks-solved
Here the one can find SQL tasks solved by me from https://sql-academy.org/ru

## Задание 1

Вывести имена всех людей, которые есть в базе данных авиакомпаний
```
SELECT name FROM Passenger
```  

## Задание 2

Вывести названия всеx авиакомпаний
```
SELECT name FROM Company
```  
 
## Задание 3

Вывести все рейсы, совершенные из Москвы
```
SELECT * FROM Trip
WHERE town_from = "Moscow"
```
