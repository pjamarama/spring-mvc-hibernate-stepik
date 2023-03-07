## Configuration
### Настройки БД
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

### Настройки applicationContext.xml
Указываем пакет, где будет проходить сканирование компонентов:
```
<context:component-scan base-package="com.agrokhotov.spring.mvc_hibernate_aop" />
```
"Активируем" MVC-аннотации:
```
<mvc:annotation-driven/>
```

В файле applicationContext.xml описываем бины:
- transactionManager. Позволяет не открывать и закрывать транзакии вручную. В параметре **ref="sessionFactory"** задается ссылка на бин sessionFactory.
- резолвер вьюшек с путем, префиксом и суффиксом InternalResourceViewResolver
- пул соединений с параметрами соединения с базой данных
- sessionFactory для получения сессий для подключения к БД. Параметр **ref="dataSource"** такой же, как bean id у бина с пулом соединений.

@Transactonal - аннотация над методом, которая показывает, что метод должен выполняться как транзакция. Spring берет на себя открытие и закрытие транзакий. Для "активации" этой аннотации мы используем тэг
```
<tx:annotation-driven transaction-manager="transactionManager" />
```
## Работа с кодом
### Отображение списка всех работников
Для обращения к БД используем DAO, вспомогательный компонент, отвественный за работу с БД. DAO, это такой подход к проектированию, когда код для работы с БД выделен в отдельный класс. Обойтись без ДАО можно, но испльзование этого компонента, хорошая практика.
![img.png](pics/img.png)

Создаем сущность Employee, создаем интерфейс EmployeeDao и его имплементацию EmployeeDaoImpl. В имплементацию с помощью @Autowired внедряем зависимость sessionFactory, класс помечаем аннотацией @Repository. 

Создаем контроллер, помечаем его аннотацией @Controller. С помощью @Autowired внедряем зависимость EmployeeDAO (интерфейс).

@Repository, как и @Controller, это специализированный @Component. Аннотация @Repository используется для DAO. При поиске компонентов Spring будет регистрировать в спринг-контейнере все DAO с аннотацией @Repository. 

Создаем вью all-employees.jsp.