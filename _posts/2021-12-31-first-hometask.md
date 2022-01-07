---
title: First Hometask. Skillbox SQL course
date: 2021-12-31T22:00:00.000+00:00
categories:
- programming
layout: post

---

# Домашнее задание студента курса "Язык запросов SQL"
--------------------------------------------

## Домашнее задание №1 - Выбор и фильтрация данных, запрос SELECT

### 1. Написать запрос для выбора студентов в порядке их регистрации.

`mysql> SELECT * FROM Students ORDER BY registration_date;`

Результат:

![Imgur](https://i.imgur.com/qbnEevR.png)


### 2. Написать запрос для выбора 5 самых дорогих курсов, на которых более 4 студентов, и которые длятся более 10 часов.

`mysql> SELECT name, duration, students_count, price FROM Courses WHERE students_count > 4 AND duration > 10 ORDER BY price DESC LIMIT 5;`

Результат: 

![](https://i.imgur.com/etaz8Kb.png)


### 3. Написать один (!) запрос, который выведет одновременно список из имен трёх самых молодых студентов, имен трёх самых старых учителей и названий трёх самых продолжительных курсов.

`mysql> (SELECT name FROM Students ORDER BY age LIMIT 3)
    -> UNION
    -> (SELECT name FROM Teachers ORDER BY age DESC LIMIT 3)
    -> UNION
    -> (SELECT name FROM Courses ORDER BY duration DESC LIMIT 3);`

Результат:

![](https://i.imgur.com/5QAUYrB.png)


## Домашнее задание №2 - Функции и выражения, агрегация данных

### 1. Написать запрос для выбора среднего возраста всех учителей с зарплатой более 10 000.

`mysql> SELECT AVG(age) AS average_age_of_teachers_whose_salary_is_higher_than_10k FROM Teachers WHERE salary > 10000;`

Результат:

![](https://i.imgur.com/MThNycx.png)

### 2. Написать запрос для расчета суммы, сколько будет стоить купить все курсы по дизайну.

`mysql> SELECT SUM(price) AS total_price_for_design_courses FROM Courses WHERE type = "DESIGN";`

Результат:

![](https://i.imgur.com/47OpV0X.png)

### 3. Написать запрос для расчёта, сколько минут (!) длятся все курсы по программированию.

`mysql> SELECT SUM(duration * 60) AS duration_of_all_prog_courses_in_minutes
    -> FROM Courses WHERE type = "PROGRAMMING";`

Результат:

![](https://i.imgur.com/Dj0x7gn.png)



## Домашнее задание №3 - Группировка, соединение таблиц (JOIN)

### 1. Напишите запрос, который выводит сумму, сколько часов должен в итоге проучиться каждый студент (сумма длительности всех курсов на которые он подписан). 
#### В результате запрос возвращает две колонки: Имя Студента — Количество часов.

`mysql> SELECT Students.name AS student_name,
    -> SUM(duration) AS duration_of_all_courses
    -> FROM Courses
    -> JOIN Subscriptions ON Courses.id = Subscriptions.course_id
    -> JOIN Students ON Students.id = Subscriptions.student_id
    -> GROUP BY Students.name;`

Результат:

![](https://i.imgur.com/x1AZQId.png)

### 2. Напишите запрос, который посчитает для каждого учителя средний возраст его учеников. 
#### В результате запрос возвращает две колонки: Имя Учителя — Средний Возраст Учеников.

`mysql> SELECT Teachers.name AS Teacher_name,
    -> AVG(Students.age) AS average_age_of_students
    -> FROM Courses
    -> JOIN Teachers ON Teachers.id = Courses.teacher_id
    -> JOIN Subscriptions ON Courses.id = Subscriptions.course_id
    -> JOIN Students ON Students.id = Subscriptions.student_id
    -> GROUP BY Teacher_name;`

Результат:

![](https://i.imgur.com/kujeH4p.png)

### 3. Напишите запрос, который выводит среднюю зарплату учителей для каждого типа курса (Дизайн/Программирование/Маркетинг и т.д.). 
#### В результате запрос возвращает две колонки: Тип Курса — Средняя зарплата.

`mysql> SELECT Courses.type AS course_type,
    -> AVG(salary) AS average_teachers_salary
    -> FROM Courses
    -> JOIN Teachers ON Teachers.id = Courses.teacher_id
    -> GROUP BY course_type;`

Результат:

![](https://i.imgur.com/KJybet7.png)
