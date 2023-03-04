# Configuration
```sql
CREATE TABLE employees (
id serial PRIMARY KEY,
name varchar(15),
surname varchar(25),
department varchar(20),
salary int
);

INSERT INTO employees (name, surname, department, salary)
VALUES ('Zaur', 'Tregulov', 'IT', 500),
       ('Oleg', 'Ivanov', 'Sales', 700),
       ('Nina', 'Sidorova', 'HR', 800);
```
Создаем новый проект из maven-артефакта maven web app. В конфигурации добавляем Tomcat.
Добавляем зависимость Spring Web MVC.