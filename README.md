Сессионное задание по БД

Описание задач, скрипты создания и наполнения таблиц, скрипты решения задач и скриншоты результатов в файле СП-БД (Fortushin).docx

Для проверки скриптов решения задач:
Перейдите на сайт XAMPP, запустите службы Apache и MySQL
После этого откройте phpMyAdmin для взаимодействия с базами данных путем нажатия кнопки Admin службы MySQL.
В открывшемся в браузере приложении в верхнем левом углу нажмите «Создать БД». Введите название базы данных и нажмите «Далее».
Теперь в созданной БД выберите пункт SQL и скопируйте в него скрипты создания таблиц базы и их наполнения: 
```
CREATE TABLE Vehicle (
	maker VARCHAR(100) NOT NULL,
	model VARCHAR(100) NOT NULL,
	type VARCHAR(20) NOT NULL CHECK (type IN ('Car', 'Motorcycle', 'Bicycle')),
	PRIMARY KEY (model)
);

-- Создание таблицы Car
 
CREATE TABLE Car (
	vin VARCHAR(17) NOT NULL,
	model VARCHAR(100) NOT NULL,
	engine_capacity DECIMAL(4, 2) NOT NULL, -- объем двигателя в литрах
	horsepower INT NOT NULL,             	-- мощность в лошадиных силах
	price DECIMAL(10, 2) NOT NULL,       	-- цена в долларах
	transmission VARCHAR(20) NOT NULL CHECK (transmission IN ('Automatic', 'Manual')), -- тип трансмиссии
	PRIMARY KEY (vin),
	FOREIGN KEY (model) REFERENCES Vehicle(model)
);
 
-- Создание таблицы Motorcycle
 
CREATE TABLE Motorcycle (
	vin VARCHAR(17) NOT NULL,
	model VARCHAR(100) NOT NULL,
	engine_capacity DECIMAL(4, 2) NOT NULL, -- объем двигателя в литрах
	horsepower INT NOT NULL,             	-- мощность в лошадиных силах
	price DECIMAL(10, 2) NOT NULL,       	-- цена в долларах
	type VARCHAR(20) NOT NULL CHECK (type IN ('Sport', 'Cruiser', 'Touring')), -- тип мотоцикла
	PRIMARY KEY (vin),
	FOREIGN KEY (model) REFERENCES Vehicle(model)
);
 
-- Создание таблицы Bicycle
 
CREATE TABLE Bicycle (
	serial_number VARCHAR(20) NOT NULL,
	model VARCHAR(100) NOT NULL,
	gear_count INT NOT NULL,             	-- количество передач
	price DECIMAL(10, 2) NOT NULL,       	-- цена в долларах
	type VARCHAR(20) NOT NULL CHECK (type IN ('Mountain', 'Road', 'Hybrid')), -- тип велосипеда
	PRIMARY KEY (serial_number),
	FOREIGN KEY (model) REFERENCES Vehicle(model)
);

-- Создание таблицы Classes
CREATE TABLE Classes (
	class VARCHAR(100) NOT NULL,
	type VARCHAR(20) NOT NULL CHECK (type IN ('Racing', 'Street')), -- тип класса
	country VARCHAR(100) NOT NULL,
	numDoors INT NOT NULL,
	engineSize DECIMAL(3, 1) NOT NULL, -- размер двигателя в литрах
	weight INT NOT NULL,            	-- вес автомобиля в килограммах
	PRIMARY KEY (class)
);
 
-- Создание таблицы Cars
CREATE TABLE Cars (
	name VARCHAR(100) NOT NULL,
	class VARCHAR(100) NOT NULL,
	year INT NOT NULL,
	PRIMARY KEY (name),
	FOREIGN KEY (class) REFERENCES Classes(class)
);
 
-- Создание таблицы Races
CREATE TABLE Races (
	name VARCHAR(100) NOT NULL,
	date DATE NOT NULL,
	PRIMARY KEY (name)
);
 
-- Создание таблицы Results
CREATE TABLE Results (
	car VARCHAR(100) NOT NULL,
	race VARCHAR(100) NOT NULL,
	position INT NOT NULL,
	PRIMARY KEY (car, race),
	FOREIGN KEY (car) REFERENCES Cars(name),
	FOREIGN KEY (race) REFERENCES Races(name)
);

-- Создание таблицы Hotel
CREATE TABLE Hotel (
    ID_hotel INT PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    location VARCHAR(255) NOT NULL
);
-- Создание таблицы Room
CREATE TABLE Room (
    ID_room INT PRIMARY KEY,
    ID_hotel INT,
    room_type ENUM('Single', 'Double', 'Suite') NOT NULL,
    price DECIMAL(10, 2) NOT NULL,
    capacity INT NOT NULL,
    FOREIGN KEY (ID_hotel) REFERENCES Hotel(ID_hotel)
);
-- Создание таблицы Customer
CREATE TABLE Customer (
    ID_customer INT PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    phone VARCHAR(20) NOT NULL
);
-- Создание таблицы Booking
CREATE TABLE Booking (
    ID_booking INT PRIMARY KEY,
    ID_room INT,
    ID_customer INT,
    check_in_date DATE NOT NULL,
    check_out_date DATE NOT NULL,
    FOREIGN KEY (ID_room) REFERENCES Room(ID_room),
    FOREIGN KEY (ID_customer) REFERENCES Customer(ID_customer)
);

CREATE TABLE Departments (
    DepartmentID INT PRIMARY KEY,
    DepartmentName VARCHAR(100) NOT NULL
);

CREATE TABLE Roles (
    RoleID INT PRIMARY KEY,
    RoleName VARCHAR(100) NOT NULL
);

CREATE TABLE Employees (
    EmployeeID INT PRIMARY KEY,
    Name VARCHAR(100) NOT NULL,
    Position VARCHAR(100),
    ManagerID INT,
    DepartmentID INT,
    RoleID INT,
    FOREIGN KEY (ManagerID) REFERENCES Employees(EmployeeID),
    FOREIGN KEY (DepartmentID) REFERENCES Departments(DepartmentID),
    FOREIGN KEY (RoleID) REFERENCES Roles(RoleID)
);

CREATE TABLE Projects (
    ProjectID INT PRIMARY KEY,
    ProjectName VARCHAR(100) NOT NULL,
    StartDate DATE,
    EndDate DATE,
    DepartmentID INT,
    FOREIGN KEY (DepartmentID) REFERENCES Departments(DepartmentID)
);

CREATE TABLE Tasks (
    TaskID INT PRIMARY KEY,
    TaskName VARCHAR(100) NOT NULL,
    AssignedTo INT,
    ProjectID INT,
    FOREIGN KEY (AssignedTo) REFERENCES Employees(EmployeeID),
    FOREIGN KEY (ProjectID) REFERENCES Projects(ProjectID)
);

-- Вставка данных в таблицу Vehicle
INSERT INTO Vehicle (maker, model, type) VALUES
('Toyota', 'Camry', 'Car'),
('Honda', 'Civic', 'Car'),
('Ford', 'Mustang', 'Car'),
('Yamaha', 'YZF-R1', 'Motorcycle'),
('Harley-Davidson', 'Sportster', 'Motorcycle'),
('Kawasaki', 'Ninja', 'Motorcycle'),
('Trek', 'Domane', 'Bicycle'),
('Giant', 'Defy', 'Bicycle'),
('Specialized', 'Stumpjumper', 'Bicycle');

-- Вставка данных в таблицу Car
INSERT INTO Car (vin, model, engine_capacity, horsepower, price, transmission) VALUES
('1HGCM82633A123456', 'Camry', 2.5, 203, 24000.00, 'Automatic'),
('2HGFG3B53GH123456', 'Civic', 2.0, 158, 22000.00, 'Manual'),
('1FA6P8CF0J1234567', 'Mustang', 5.0, 450, 55000.00, 'Automatic');

-- Вставка данных в таблицу Motorcycle
INSERT INTO Motorcycle (vin, model, engine_capacity, horsepower, price, type) VALUES
('JYARN28E9FA123456', 'YZF-R1', 1.0, 200, 17000.00, 'Sport'),
('1HD1ZK3158K123456', 'Sportster', 1.2, 70, 12000.00, 'Cruiser'),
('JKBVNAF156A123456', 'Ninja', 0.9, 150, 14000.00, 'Sport');

-- Вставка данных в таблицу Bicycle
INSERT INTO Bicycle (serial_number, model, gear_count, price, type) VALUES
('SN123456789', 'Domane', 22, 3500.00, 'Road'),
('SN987654321', 'Defy', 20, 3000.00, 'Road'),
('SN456789123', 'Stumpjumper', 30, 4000.00, 'Mountain');

-- Вставка данных в таблицу Classes
INSERT INTO Classes (class, type, country, numDoors, engineSize, weight) VALUES
('SportsCar', 'Racing', 'USA', 2, 3.5, 1500),
('Sedan', 'Street', 'Germany', 4, 2.0, 1200),
('SUV', 'Street', 'Japan', 4, 2.5, 1800),
('Hatchback', 'Street', 'France', 5, 1.6, 1100),
('Convertible', 'Racing', 'Italy', 2, 3.0, 1300),
('Coupe', 'Street', 'USA', 2, 2.5, 1400),
('Luxury Sedan', 'Street', 'Germany', 4, 3.0, 1600),
('Pickup', 'Street', 'USA', 2, 2.8, 2000);

-- Вставка данных в таблицу Cars
INSERT INTO Cars (name, class, year) VALUES
('Ford Mustang', 'SportsCar', 2020),
('BMW 3 Series', 'Sedan', 2019),
('Toyota RAV4', 'SUV', 2021),
('Renault Clio', 'Hatchback', 2020),
('Ferrari 488', 'Convertible', 2019),
('Chevrolet Camaro', 'Coupe', 2021),
('Mercedes-Benz S-Class', 'Luxury Sedan', 2022),
('Ford F-150', 'Pickup', 2021),
('Audi A4', 'Sedan', 2018),
('Nissan Rogue', 'SUV', 2020);

-- Вставка данных в таблицу Races
INSERT INTO Races (name, date) VALUES
('Indy 500', '2023-05-28'),
('Le Mans', '2023-06-10'),
('Monaco Grand Prix', '2023-05-28'),
('Daytona 500', '2023-02-19'),
('Spa 24 Hours', '2023-07-29'),
('Bathurst 1000', '2023-10-08'),
('Nürburgring 24 Hours', '2023-06-17'),
('Pikes Peak International Hill Climb', '2023-06-25');

-- Вставка данных в таблицу Results
INSERT INTO Results (car, race, position) VALUES
('Ford Mustang', 'Indy 500', 1),
('BMW 3 Series', 'Le Mans', 3),
('Toyota RAV4', 'Monaco Grand Prix', 2),
('Renault Clio', 'Daytona 500', 5),
('Ferrari 488', 'Le Mans', 1),
('Chevrolet Camaro', 'Monaco Grand Prix', 4),
('Mercedes-Benz S-Class', 'Spa 24 Hours', 2),
('Ford F-150', 'Bathurst 1000', 6),
('Audi A4', 'Nürburgring 24 Hours', 8),
('Nissan Rogue', 'Pikes Peak International Hill Climb', 3);

-- Вставка данных в таблицу Hotel
INSERT INTO Hotel (ID_hotel, name, location) VALUES
(1, 'Grand Hotel', 'Paris, France'),
(2, 'Ocean View Resort', 'Miami, USA'),
(3, 'Mountain Retreat', 'Aspen, USA'),
(4, 'City Center Inn', 'New York, USA'),
(5, 'Desert Oasis', 'Las Vegas, USA'),
(6, 'Lakeside Lodge', 'Lake Tahoe, USA'),
(7, 'Historic Castle', 'Edinburgh, Scotland'),
(8, 'Tropical Paradise', 'Bali, Indonesia'),
(9, 'Business Suites', 'Tokyo, Japan'),
(10, 'Eco-Friendly Hotel', 'Copenhagen, Denmark');

-- Вставка данных в таблицу Room
INSERT INTO Room (ID_room, ID_hotel, room_type, price, capacity) VALUES
(1, 1, 'Single', 150.00, 1),
(2, 1, 'Double', 200.00, 2),
(3, 1, 'Suite', 350.00, 4),
(4, 2, 'Single', 120.00, 1),
(5, 2, 'Double', 180.00, 2),
(6, 2, 'Suite', 300.00, 4),
(7, 3, 'Double', 250.00, 2),
(8, 3, 'Suite', 400.00, 4),
(9, 4, 'Single', 100.00, 1),
(10, 4, 'Double', 150.00, 2),
(11, 5, 'Single', 90.00, 1),
(12, 5, 'Double', 140.00, 2),
(13, 6, 'Suite', 280.00, 4),
(14, 7, 'Double', 220.00, 2),
(15, 8, 'Single', 130.00, 1),
(16, 8, 'Double', 190.00, 2),
(17, 9, 'Suite', 360.00, 4),
(18, 10, 'Single', 110.00, 1),
(19, 10, 'Double', 160.00, 2);

-- Вставка данных в таблицу Customer
INSERT INTO Customer (ID_customer, name, email, phone) VALUES
(1, 'John Doe', 'john.doe@example.com', '+1234567890'),
(2, 'Jane Smith', 'jane.smith@example.com', '+0987654321'),
(3, 'Alice Johnson', 'alice.johnson@example.com', '+1122334455'),
(4, 'Bob Brown', 'bob.brown@example.com', '+2233445566'),
(5, 'Charlie White', 'charlie.white@example.com', '+3344556677'),
(6, 'Diana Prince', 'diana.prince@example.com', '+4455667788'),
(7, 'Ethan Hunt', 'ethan.hunt@example.com', '+5566778899'),
(8, 'Fiona Apple', 'fiona.apple@example.com', '+6677889900'),
(9, 'George Washington', 'george.washington@example.com', '+7788990011'),
(10, 'Hannah Montana', 'hannah.montana@example.com', '+8899001122');

-- Вставка данных в таблицу Booking с разнообразием клиентов
INSERT INTO Booking (ID_booking, ID_room, ID_customer, check_in_date, check_out_date) VALUES
(1, 1, 1, '2025-05-01', '2025-05-05'),  -- 4 ночи, John Doe
(2, 2, 2, '2025-05-02', '2025-05-06'),  -- 4 ночи, Jane Smith
(3, 3, 3, '2025-05-03', '2025-05-07'),  -- 4 ночи, Alice Johnson
(4, 4, 4, '2025-05-04', '2025-05-08'),  -- 4 ночи, Bob Brown
(5, 5, 5, '2025-05-05', '2025-05-09'),  -- 4 ночи, Charlie White
(6, 6, 6, '2025-05-06', '2025-05-10'),  -- 4 ночи, Diana Prince
(7, 7, 7, '2025-05-07', '2025-05-11'),  -- 4 ночи, Ethan Hunt
(8, 8, 8, '2025-05-08', '2025-05-12'),  -- 4 ночи, Fiona Apple
(9, 9, 9, '2025-05-09', '2025-05-13'),  -- 4 ночи, George Washington
(10, 10, 10, '2025-05-10', '2025-05-14'),  -- 4 ночи, Hannah Montana
(11, 1, 2, '2025-05-11', '2025-05-15'),  -- 4 ночи, Jane Smith
(12, 2, 3, '2025-05-12', '2025-05-14'),  -- 2 ночи, Alice Johnson
(13, 3, 4, '2025-05-13', '2025-05-15'),  -- 2 ночи, Bob Brown
(14, 4, 5, '2025-05-14', '2025-05-16'),  -- 2 ночи, Charlie White
(15, 5, 6, '2025-05-15', '2025-05-16'),  -- 1 ночь, Diana Prince
(16, 6, 7, '2025-05-16', '2025-05-18'),  -- 2 ночи, Ethan Hunt
(17, 7, 8, '2025-05-17', '2025-05-21'),  -- 4 ночи, Fiona Apple
(18, 8, 9, '2025-05-18', '2025-05-19'),  -- 1 ночь, George Washington
(19, 9, 10, '2025-05-19', '2025-05-22'),  -- 3 ночи, Hannah Montana
(20, 10, 1, '2025-05-20', '2025-05-22'), -- 2 ночи, John Doe
(21, 1, 2, '2025-05-21', '2025-05-23'),  -- 2 ночи, Jane Smith
(22, 2, 3, '2025-05-22', '2025-05-25'),  -- 3 ночи, Alice Johnson
(23, 3, 4, '2025-05-23', '2025-05-26'),  -- 3 ночи, Bob Brown
(24, 4, 5, '2025-05-24', '2025-05-25'),  -- 1 ночь, Charlie White
(25, 5, 6, '2025-05-25', '2025-05-27'),  -- 2 ночи, Diana Prince
(26, 6, 7, '2025-05-26', '2025-05-29');  -- 3 ночи, Ethan Hunt

-- Добавление отделов
INSERT INTO Departments (DepartmentID, DepartmentName) VALUES
(1, 'Отдел продаж'),
(2, 'Отдел маркетинга'),
(3, 'IT-отдел'),
(4, 'Отдел разработки'),
(5, 'Отдел поддержки');

-- Добавление ролей
INSERT INTO Roles (RoleID, RoleName) VALUES
(1, 'Менеджер'),
(2, 'Директор'),
(3, 'Генеральный директор'),
(4, 'Разработчик'),
(5, 'Специалист по поддержке'),
(6, 'Маркетолог');

-- Добавление сотрудников
INSERT INTO Employees (EmployeeID, Name, Position, ManagerID, DepartmentID, RoleID) VALUES
(1, 'Иван Иванов', 'Генеральный директор', NULL, 1, 3),
(2, 'Петр Петров', 'Директор по продажам', 1, 1, 2),
(3, 'Светлана Светлова', 'Директор по маркетингу', 1, 2, 2),
(4, 'Алексей Алексеев', 'Менеджер по продажам', 2, 1, 1),
(5, 'Мария Мариева', 'Менеджер по маркетингу', 3, 2, 1),
(6, 'Андрей Андреев', 'Разработчик', 1, 4, 4),
(7, 'Елена Еленова', 'Специалист по поддержке', 1, 5, 5),
(8, 'Олег Олегов', 'Менеджер по продукту', 2, 1, 1),
(9, 'Татьяна Татеева', 'Маркетолог', 3, 2, 6),
(10, 'Николай Николаев', 'Разработчик', 6, 4, 4),
(11, 'Ирина Иринина', 'Разработчик', 6, 4, 4),
(12, 'Сергей Сергеев', 'Специалист по поддержке', 7, 5, 5),
(13, 'Кристина Кристинина', 'Менеджер по продажам', 4, 1, 1),
(14, 'Дмитрий Дмитриев', 'Маркетолог', 3, 2, 6),
(15, 'Виктор Викторов', 'Менеджер по продажам', 4, 1, 1),
(16, 'Анастасия Анастасиева', 'Специалист по поддержке', 7, 5, 5),
(17, 'Максим Максимов', 'Разработчик', 6, 4, 4),
(18, 'Людмила Людмилова', 'Специалист по маркетингу', 3, 2, 6),
(19, 'Наталья Натальева', 'Менеджер по продажам', 4, 1, 1),
(20, 'Александр Александров', 'Менеджер по маркетингу', 3, 2, 1),
(21, 'Галина Галина', 'Специалист по поддержке', 7, 5, 5),
(22, 'Павел Павлов', 'Разработчик', 6, 4, 4),
(23, 'Марина Маринина', 'Специалист по маркетингу', 3, 2, 6),
(24, 'Станислав Станиславов', 'Менеджер по продажам', 4, 1, 1),
(25, 'Екатерина Екатеринина', 'Специалист по поддержке', 7, 5, 5),
(26, 'Денис Денисов', 'Разработчик', 6, 4, 4),
(27, 'Ольга Ольгина', 'Маркетолог', 3, 2, 6),
(28, 'Игорь Игорев', 'Менеджер по продукту', 2, 1, 1),
(29, 'Анастасия Анастасиевна', 'Специалист по поддержке', 7, 5, 5),
(30, 'Валентин Валентинов', 'Разработчик', 6, 4, 4);

-- Добавление проектов
INSERT INTO Projects (ProjectID, ProjectName, StartDate, EndDate, DepartmentID) VALUES
(1, 'Проект A', '2025-01-01', '2025-12-31', 1),
(2, 'Проект B', '2025-02-01', '2025-11-30', 2),
(3, 'Проект C', '2025-03-01', '2025-10-31', 4),
(4, 'Проект D', '2025-04-01', '2025-09-30', 5),
(5, 'Проект E', '2025-05-01', '2025-08-31', 3);

-- Добавление задач
INSERT INTO Tasks (TaskID, TaskName, AssignedTo, ProjectID) VALUES
(1, 'Задача 1: Подготовка отчета по продажам', 4, 1),
(2, 'Задача 2: Анализ рынка', 9, 2),
(3, 'Задача 3: Разработка нового функционала', 10, 3),
(4, 'Задача 4: Поддержка клиентов', 12, 4),
(5, 'Задача 5: Создание рекламной кампании', 5, 2),
(6, 'Задача 6: Обновление документации', 6, 3),
(7, 'Задача 7: Проведение тренинга для сотрудников', 8, 1),
(8, 'Задача 8: Тестирование нового продукта', 11, 3),
(9, 'Задача 9: Ответы на запросы клиентов', 12, 4),
(10, 'Задача 10: Подготовка маркетинговых материалов', 9, 2),
(11, 'Задача 11: Интеграция с новым API', 10, 3),
(12, 'Задача 12: Настройка системы поддержки', 7, 5),
(13, 'Задача 13: Проведение анализа конкурентов', 9, 2),
(14, 'Задача 14: Создание презентации для клиентов', 4, 1),
(15, 'Задача 15: Обновление сайта', 6, 3);
```
Транспортные средства
----------------
Задача 1
Найдите производителей (maker) и модели всех мотоциклов, которые имеют мощность более 150 лошадиных сил, стоят менее 20 тысяч долларов и являются спортивными (тип Sport). Также отсортируйте результаты по мощности в порядке убывания.
```
select v.maker, v.model 
from Vehicle v 
join motorcycle m on m.model = v.model 
where m.horsepower > 150 and m.price < 20000 and m.type = 'Sport' 
order by m.horsepower desc;
```
----------------
Задача 2
Найти информацию о производителях и моделях различных типов транспортных средств (автомобили, мотоциклы и велосипеды), которые соответствуют заданным критериям.
Автомобили:
Извлечь данные о всех автомобилях, которые имеют:
Мощность двигателя более 150 лошадиных сил.
Объем двигателя менее 3 литров.
Цену менее 35 тысяч долларов.
В выводе должны быть указаны производитель (maker), номер модели (model), мощность (horsepower), 
объем двигателя (engine_capacity) и тип транспортного средства, который будет обозначен как Car.
Мотоциклы:
Извлечь данные о всех мотоциклах, которые имеют:
Мощность двигателя более 150 лошадиных сил.
Объем двигателя менее 1,5 литров.
Цену менее 20 тысяч долларов.
В выводе должны быть указаны производитель (maker), номер модели (model), мощность (horsepower), 
объем двигателя (engine_capacity) и тип транспортного средства, который будет обозначен как Motorcycle
Велосипеды:
Извлечь данные обо всех велосипедах, которые имеют:
Количество передач больше 18.
Цену менее 4 тысяч долларов.
В выводе должны быть указаны производитель (maker), номер модели (model), а также NULL для мощности и объема двигателя, так как эти характеристики не применимы для велосипедов. Тип транспортного средства будет обозначен как Bicycle.
```
select v.maker, c.model, c.horsepower, c.engine_capacity, v.type 
from Vehicle v 
join Car c on c.model = v.model 
where c.horsepower > 150 and c.engine_capacity < 3 and c.price < 35000
union
select v.maker, m.model, m.horsepower, m.engine_capacity, v.type 
from Vehicle v 
join Motorcycle m on m.model = v.model 
where m.horsepower > 150 and m.engine_capacity < 1.5 and m.price < 20000
union
select v.maker, b.model, null, null, v.type 
from Vehicle v 
join Bicycle b on b.model = v.model 
where b.gear_count > 18 and b.price < 4000
order by horsepower desc;
```
Автомобильные гонки
----------------
Задача 1
Определить, какие автомобили из каждого класса имеют наименьшую среднюю позицию в гонках, и вывести информацию о каждом таком автомобиле для данного класса, включая его класс, среднюю позицию и количество гонок, в которых он участвовал. Также отсортировать результаты по средней позиции.
```
With avgPosition as (
Select c.name, c.class, avg(r.position) as avg_pos, count(r.race) as races_cnt 
from Cars c 
join Results r on c.name = r.car
group by c.class, c.name
),
minAvgPosition as (
	select class, min(avg_pos) as min_avg_pos 
from avgPosition
group by class
)
Select ap.name, ap.class, ap.avg_pos, ap.races_cnt from avgPosition ap
Join minAvgPosition map on ap.class = map.class and ap.avg_pos = map.min_avg_pos
Order by ap.avg_pos, ap.name;
```
----------------
Задача 2
Определить автомобиль, который имеет наименьшую среднюю позицию в гонках среди всех автомобилей, и вывести информацию об этом автомобиле, включая его класс, среднюю позицию, количество гонок, в которых он участвовал, и страну производства класса автомобиля. Если несколько автомобилей имеют одинаковую наименьшую среднюю позицию, выбрать один из них по алфавиту (по имени автомобиля).
```
With avgPosition as (
	Select c.name, c.class, avg(r.position) as avg_pos, count(r.race) as race_cnt
from Cars c 
join Results r on r.car = c.name
group by c.name, c.class
)
Select ap.name, ap.class, min(ap.avg_pos) as min_avg_pos, ap.race_cnt, cl.country
	From avgPosition ap
	Join Classes cl on cl.class = ap.class
Order by min_avg_pos, ap.name;
```
----------------
Задача 3

Определить классы автомобилей, которые имеют наименьшую среднюю позицию в гонках, и вывести информацию о каждом автомобиле из этих классов, включая его имя, среднюю позицию, количество гонок, в которых он участвовал, страну производства класса автомобиля, а также общее количество гонок, в которых участвовали автомобили этих классов. Если несколько классов имеют одинаковую среднюю позицию, выбрать все из них.
```
With avgPosition as (
	Select c.name, c.class, avg(r.position) as avg_pos, count(r.race) as race_cnt
from Cars c 
join Results r on r.car = c.name
group by c.name, c.class
),
minAvg as (
	select min(avg_pos) as min_avg_pos from avgPosition
)
Select ap.name, ap.class, ap.avg_pos, ap.race_cnt, cl.country
	From avgPosition ap
	Join Classes cl on cl.class = ap.class
	Where ap.avg_pos = (select min_avg_pos from minAvg)
Order by ap.avg_pos, ap.name;
```
----------------
Задача 4
Определить, какие автомобили имеют среднюю позицию лучше (меньше) средней позиции всех автомобилей в своем классе (то есть автомобилей в классе должно быть минимум два, чтобы выбрать один из них). Вывести информацию об этих автомобилях, включая их имя, класс, среднюю позицию, количество гонок, в которых они участвовали, и страну производства класса автомобиля. Также отсортировать результаты по классу и затем по средней позиции в порядке возрастания.
```
With carAvg as 
( 
Select c.name, c.class, avg(r.position) as car_avg_pos, count(r.race) as race_count 
From Cars c join Results r on c.name = r.car 
Group by c.name, c.class 
), 
classAvg as ( 
select c.class, avg(r.position) as class_avg_pos, count(distinct c.name) as car_count 
from Cars c join Results r on c.name = r.car 
group by c.class 
having count(distinct c.name) >= 2 
) 
Select ca.name, ca.class, ca.car_avg_pos, ca.race_count, cl.country 
From carAvg ca join classAvg cla on ca.class = cla.class 
Join Classes cl on ca.class = cl.class 
Where ca.car_avg_pos < cla.class_avg_pos 
Order by ca.class, ca.car_avg_pos;
```
----------------
Задача 5
Определить, какие классы автомобилей имеют наибольшее количество автомобилей с низкой средней позицией (больше 3.0) и вывести информацию о каждом автомобиле из этих классов, включая его имя, класс, среднюю позицию, количество гонок, в которых он участвовал, страну производства класса автомобиля, а также общее количество гонок для каждого класса. Отсортировать результаты по количеству автомобилей с низкой средней позицией.
--- не решена

Бронирование отелей
----------------
Задача 1
Определить, какие клиенты сделали более двух бронирований в разных отелях, и вывести информацию о каждом таком клиенте, включая его имя, электронную почту, телефон, общее количество бронирований, а также список отелей, в которых они бронировали номера (объединенные в одно поле через запятую). Также подсчитать среднюю длительность их пребывания (в днях) по всем бронированиям. Отсортировать результаты по количеству бронирований в порядке убывания.
```
Select c.name, c.email, c.phone, count(b.ID_booking) as total_bookings,
Group_concat(distinct h.name separator ', ') as hotels, avg(datediff(b.check_out_date, b.check_in_date)) as avg_stay_days
from Customer c
join Booking b on c.ID_customer = b.ID_customer
join Room r on b.ID_room = r.ID_room
join Hotel h on r.ID_hotel = h.ID_hotel
group by c.ID_customer, c.name, c.email, c.phone
having count(b.ID_booking) > 2 and count(distinct h.ID_hotel) > 1
order by total_bookings desc;
```
----------------
Задача 2
Необходимо провести анализ клиентов, которые сделали более двух бронирований в разных отелях и потратили более 500 долларов на свои бронирования. Для этого:
•	Определить клиентов, которые сделали более двух бронирований и забронировали номера в более чем одном отеле. Вывести для каждого такого клиента следующие данные: ID_customer, имя, общее количество бронирований, общее количество уникальных отелей, в которых они бронировали номера, и общую сумму, потраченную на бронирования.
•	Также определить клиентов, которые потратили более 500 долларов на бронирования, и вывести для них ID_customer, имя, общую сумму, потраченную на бронирования, и общее количество бронирований.
•	В результате объединить данные из первых двух пунктов, чтобы получить список клиентов, которые соответствуют условиям обоих запросов. Отобразить поля: ID_customer, имя, общее количество бронирований, общую сумму, потраченную на бронирования, и общее количество уникальных отелей.
•	Результаты отсортировать по общей сумме, потраченной клиентами, в порядке возрастания.

```
with client_stats as (
    select c.ID_customer, c.name, count(b.ID_booking) as total_bookings, count(distinct h.ID_hotel) AS total_hotels,
        sum(r.price) as total_spent
    from Customer c
    join Booking b on c.ID_customer = b.ID_customer
    join Room r on b.ID_room = r.ID_room
    join Hotel h on r.ID_hotel = h.ID_hotel
    group by c.ID_customer, c.name
),
frequent_clients as (
    select *
    from client_stats
    where total_bookings > 2 and total_hotels > 1
),
big_spenders as (
    select *
    from client_stats
    where total_spent > 500
)
Select f.ID_customer, f.name, f.total_bookings, f.total_spent, f.total_hotels 
From frequent_clients f
Join big_spenders b on f.ID_customer = b.ID_customer
Order by f.total_spent asc;
```
----------------
Задача 3
Вам необходимо провести анализ данных о бронированиях в отелях и определить предпочтения клиентов по типу отелей. Для этого выполните следующие шаги:
1.	Категоризация отелей. 
Определите категорию каждого отеля на основе средней стоимости номера:
o	«Дешевый»: средняя стоимость менее 175 долларов.
o	«Средний»: средняя стоимость от 175 до 300 долларов.
o	«Дорогой»: средняя стоимость более 300 долларов.
2.	Анализ предпочтений клиентов. 
Для каждого клиента определите предпочитаемый тип отеля на основании условия ниже:
o	Если у клиента есть хотя бы один «дорогой» отель, присвойте ему категорию «дорогой».
o	Если у клиента нет «дорогих» отелей, но есть хотя бы один «средний», присвойте ему категорию «средний».
o	Если у клиента нет «дорогих» и «средних» отелей, но есть «дешевые», присвойте ему категорию предпочитаемых отелей «дешевый».
3.	Вывод информации. 
Выведите для каждого клиента следующую информацию:
o	ID_customer: уникальный идентификатор клиента.
o	name: имя клиента.
o	preferred_hotel_type: предпочитаемый тип отеля.
o	visited_hotels: список уникальных отелей, которые посетил клиент.
4.	Сортировка результатов. 
Отсортируйте клиентов так, чтобы сначала шли клиенты с «дешевыми» отелями, затем со «средними» и в конце — с «дорогими».
```
with hotel_category as (
    select h.ID_hotel, h.name, avg(r.price) as avg_price,
 CASE
            when avg(r.price) < 175 then 'Дешевый'
            when avg(r.price) <= 300 then 'Средний'
            else 'Дорогой'
 END as category
    from Hotel h
    join Room r on h.ID_hotel = r.ID_hotel
    group by h.ID_hotel, h.name
),

client_preferences as (
    select 
        c.ID_customer,
        c.name,
        max(CASE when hc.category = 'Дорогой' then 1 else 0 END) as has_expensive,
        max(CASE when hc.category = 'Средний' then 1 else 0 END) as has_medium,
        max(CASE when hc.category = 'Дешевый' then 1 else 0 END) as has_cheap,
        group_concat(distinct hc.name separator ', ') as visited_hotels
    from Customer c
    join Booking b on c.ID_customer = b.ID_customer
    join Room r on b.ID_room = r.ID_room
    join hotel_category hc on r.ID_hotel = hc.ID_hotel
    group by c.ID_customer, c.name
)
select ID_customer, name,
 CASE
        When has_expensive = 1 then 'Дорогой'
        when has_medium = 1 then 'Средний'
        else 'Дешевый'
    END as preferred_hotel_type,
    visited_hotels
from client_preferences
order by
    CASE
        when has_cheap = 1 and has_medium = 0 and has_expensive = 0 then 1
        when has_medium = 1 and has_expensive = 0 then 2
        else 3
    END;
```
Структура организации
----------------
Задача 1
Найти всех сотрудников, подчиняющихся Ивану Иванову (с EmployeeID = 1), включая их подчиненных и подчиненных подчиненных, а также самого Ивана Иванова. Для каждого сотрудника вывести следующую информацию:
1.	EmployeeID: идентификатор сотрудника.
2.	Имя сотрудника.
3.	ManagerID: Идентификатор менеджера.
4.	Название отдела, к которому он принадлежит.
5.	Название роли, которую он занимает.
6.	Название проектов, к которым он относится (если есть, конкатенированные в одном столбце через запятую).
7.	Название задач, назначенных этому сотруднику (если есть, конкатенированные в одном столбце через запятую).
8.	Если у сотрудника нет назначенных проектов или задач, отобразить NULL.
Требования:
•	Рекурсивно извлечь всех подчиненных сотрудников Ивана Иванова и их подчиненных.
•	Для каждого сотрудника отобразить информацию из всех таблиц.
•	Результаты должны быть отсортированы по имени сотрудника.
•	Решение задачи должно представлять из себя один sql-запрос и задействовать ключевое слово RECURSIVE.
```
with recursive subordinates as (
select e.EmployeeID, e.Name, e.ManagerID, e.DepartmentID, e.RoleID
from Employees e
where e.EmployeeID = 1
union all
select e.EmployeeID, e.Name, e.ManagerID, e.DepartmentID, e.RoleID
from Employees e
join subordinates s on e.ManagerID = s.EmployeeID
),
employee_projects as (
select e.EmployeeID, group_concat(distinct p.ProjectName separator ', ') AS ProjectNames
from subordinates e
left join Projects p on e.DepartmentID = p.DepartmentID
group by e.EmployeeID
),
employee_tasks as (
select t.AssignedTo as EmployeeID, group_concat(t.TaskName separator ', ') as TaskNames
from Tasks t
group by t.AssignedTo
)
select s.EmployeeID, s.Name as EmployeeName, s.ManagerID, d.DepartmentName, r.RoleName, ep.ProjectNames,
    et.TaskNames
from subordinates s
left join Departments d on s.DepartmentID = d.DepartmentID
left join Roles r on s.RoleID = r.RoleID
left join employee_projects ep on s.EmployeeID = ep.EmployeeID
left join employee_tasks et on s.EmployeeID = et.EmployeeID
order by s.Name;
```
----------------
Задача 2
Найти всех сотрудников, подчиняющихся Ивану Иванову с EmployeeID = 1, включая их подчиненных и подчиненных подчиненных, а также самого Ивана Иванова. Для каждого сотрудника вывести следующую информацию:
1.	EmployeeID: идентификатор сотрудника.
2.	Имя сотрудника.
3.	Идентификатор менеджера.
4.	Название отдела, к которому он принадлежит.
5.	Название роли, которую он занимает.
6.	Название проектов, к которым он относится (если есть, конкатенированные в одном столбце).
7.	Название задач, назначенных этому сотруднику (если есть, конкатенированные в одном столбце).
8.	Общее количество задач, назначенных этому сотруднику.
9.	Общее количество подчиненных у каждого сотрудника (не включая подчиненных их подчиненных).
10.	Если у сотрудника нет назначенных проектов или задач, отобразить NULL.
```
With recursive subordinates as (
select e.EmployeeID, e.Name, e.ManagerID, e.DepartmentID, e.RoleID
from Employees e
where e.EmployeeID = 1
union all
select e.EmployeeID, e.Name, e.ManagerID, e.DepartmentID, e.RoleID
from Employees e
join subordinates s on e.ManagerID = s.EmployeeID
),
employee_projects as (
select s.EmployeeID, group_concat(distinct p.ProjectName separator ', ') as ProjectNames
from subordinates s
left join Projects p on s.DepartmentID = p.DepartmentID
group by s.EmployeeID
),
employee_tasks as (
select s.EmployeeID, group_concat(t.TaskName separator ', ') as TaskNames, count(t.TaskID) as TotalTasks
from subordinates s
left join Tasks t on s.EmployeeID = t.AssignedTo
group by s.EmployeeID
),
employee_subordinates_count as (
select ManagerID AS EmployeeID, count(*) as TotalSubordinates
from Employees
group by ManagerID
)
Select s.EmployeeID, s.Name as EmployeeName, s.ManagerID, d.DepartmentName, r.RoleName, ep.ProjectNames, et.TaskNames,
coalesce(et.TotalTasks, 0) as TotalTasks,
coalesce(esc.TotalSubordinates, 0) as TotalSubordinates
from subordinates s
left join Departments d on s.DepartmentID = d.DepartmentID
left join Roles r on s.RoleID = r.RoleID
left join employee_projects ep on s.EmployeeID = ep.EmployeeID
left join employee_tasks et on s.EmployeeID = et.EmployeeID
left join employee_subordinates_count esc on s.EmployeeID = esc.EmployeeID
order by s.Name;
```
----------------
Задача 3
Найти всех сотрудников, которые занимают роль менеджера и имеют подчиненных (то есть число подчиненных больше 0). Для каждого такого сотрудника вывести следующую информацию:
1.	EmployeeID: идентификатор сотрудника.
2.	Имя сотрудника.
3.	Идентификатор менеджера.
4.	Название отдела, к которому он принадлежит.
5.	Название роли, которую он занимает.
6.	Название проектов, к которым он относится (если есть, конкатенированные в одном столбце).
7.	Название задач, назначенных этому сотруднику (если есть, конкатенированные в одном столбце).
8.	Общее количество подчиненных у каждого сотрудника (включая их подчиненных).
9.	Если у сотрудника нет назначенных проектов или задач, отобразить NULL.
```
With recursive hierarchy as (
Select e.EmployeeID as manager_id, e.EmployeeID AS subordinate_id
From Employees e
Union all
select h.manager_id, e.EmployeeID
from hierarchy h
join Employees e on e.ManagerID = h.subordinate_id
),
subordinate_count as (
select manager_id as EmployeeID, count(*) - 1 as TotalSubordinates
from hierarchy
group by manager_id
),
employee_projects as (
select e.EmployeeID, group_concat(distinct p.ProjectName separator ', ') as ProjectNames
from Employees e
left join Projects p on e.DepartmentID = p.DepartmentID
group by e.EmployeeID
),
employee_tasks as (
select e.EmployeeID, group_concat(t.TaskName separator ', ') as TaskNames
from Employees e
left join Tasks t on e.EmployeeID = t.AssignedTo
group by e.EmployeeID
)
select e.EmployeeID, e.Name, e.ManagerID, d.DepartmentName, r.RoleName, ep.ProjectNames, et.TaskNames,
sc.TotalSubordinates
from Employees e
join Roles r on e.RoleID = r.RoleID
left join Departments d on e.DepartmentID = d.DepartmentID
left join subordinate_count sc on e.EmployeeID = sc.EmployeeID
left join employee_projects ep on e.EmployeeID = ep.EmployeeID
left join employee_tasks et on e.EmployeeID = et.EmployeeID
where r.RoleName = 'Менеджер' and sc.TotalSubordinates > 0 
order by e.Name;
```


