# Data Base Command
### В подключенном MySQL репозитории создать базу данных “Друзья человека”
___
    CREATE DATABASE HumanFrends;
    USE  HumanFrends;

### Создать таблицы с иерархией из диаграммы в БД,
### Заполнить низкоуровневые таблицы именами(животных), командами которые они выполняют и датами рождения

___
    CREATE  TABLE Animal (
    id INT PRIMARY KEY AUTO_INCREMENT,
    type_animal VARCHAR(50) NOT NULL);
    INSERT INTO animal (type_animal)
    VALUES('Pets'),
    ('Pak Animals');

    CREATE TABLE Pets (
    id INT PRIMARY KEY AUTO_INCREMENT,
    type_animal INT NOT NULL,
    FOREIGN KEY (type_animal) REFERENCES animal (id),
    genus_animal VARCHAR(50) NOT NULL);

    INSERT INTO pets(type_animal, genus_animal)
    VALUES(1,'Dog'),
    (1,'Cat'),
    (1,'Hamster');

    CREATE TABLE PetAnimal (
    id INT PRIMARY KEY AUTO_INCREMENT,
    type_animal INT NOT NULL,
    FOREIGN KEY (type_animal) REFERENCES animal (id),
    genus_animal VARCHAR(50) NOT NULL);

    INSERT INTO petanimal (type_animal, genus_animal)
    VALUES(2,'Horse'),
    (2,'Camel'),
    (2,'Donkey');

    CREATE TABLE dog (
    id INT PRIMARY KEY AUTO_INCREMENT,
    genus_animal INT NOT NULL,
    FOREIGN KEY (genus_animal) REFERENCES pets (id),
    name VARCHAR(50) NOT NULL,
    birthday DATE NOT NULL,
    comands VARCHAR(255) NOT NULL
    )
 
    INSERT INTO dog (genus_animal,name,birthday,comands)
    VALUES (1,'Бадди','2020-01-02','seat guard run voice'),
    (1,'Лаки','2021-01-02','seat guard run voice'),
    (1,'Смайли','2023-01-02','seat guard run voice'),
    (1,'Спарки','2022-01-02','seat guard run voice'),
    (1,'Берти','2024-01-02','seat guard run voice');
 
    CREATE TABLE cat (
    id INT PRIMARY KEY AUTO_INCREMENT,
    genus_animal INT NOT NULL,
    FOREIGN KEY (genus_animal) REFERENCES pets (id),
    name VARCHAR(50) NOT NULL,
    birthday DATE NOT NULL,
    comands VARCHAR(255) NOT NULL
    )
 
    INSERT INTO cat (genus_animal,name,birthday,comands)
    VALUES (2,'Ася','2020-01-02','seat climb run voice'),
    (2,'Понка','2021-01-02','seat climb run voice'),
    (2,'Мурка','2023-01-02','seat climb run voice'),
    (2,'Буся','2022-01-02','seat climb run voice'),
    (2,'Чиколетта','2024-01-02','seat climb run voice');

    CREATE TABLE hamster (
    id INT PRIMARY KEY AUTO_INCREMENT,
    genus_animal INT NOT NULL,
    FOREIGN KEY (genus_animal) REFERENCES pets (id),
    name VARCHAR(50) NOT NULL,
    birthday DATE NOT NULL,
    comands VARCHAR(255) NOT NULL
    )
 
    INSERT INTO hamster (genus_animal,name,birthday,comands)
    VALUES (3,'Фливти','2020-01-02','seat run eat'),
    (3,'Бунтарь','2021-01-02','seat run eat'),
    (3,'Ролли','2023-01-02','seat run eat'),
    (3,'Пепперони','2022-01-02','seat run eat'),
    (3,'Джек','2024-01-02','seat run eat');

### Pat Animal
 
    CREATE TABLE horse (
    id INT PRIMARY KEY AUTO_INCREMENT,
    genus_animal INT NOT NULL,
    FOREIGN KEY (genus_animal) REFERENCES petanimal (id),
    name VARCHAR(50) NOT NULL,
    birthday DATE NOT NULL,
    comands VARCHAR(255) NOT NULL
    )

    INSERT INTO horse (genus_animal,name,birthday,comands)
    VALUES (1,'Вавилон','2020-01-02','seat run eat load-cargo'),
    (1,'Спирит','2021-01-02','seat run eat load-cargo'),
    (1,'Агат','2023-01-02','seat run eat load-cargo'),
    (1,'Гром','2022-01-02','seat run eat load-cargo'),
    (1,'Люцифер','2024-01-02','seat run eat load-cargo');

    CREATE TABLE camel (
    id INT PRIMARY KEY AUTO_INCREMENT,
    genus_animal INT NOT NULL,
    FOREIGN KEY (genus_animal) REFERENCES petanimal (id),
    name VARCHAR(50) NOT NULL,
    birthday DATE NOT NULL,
    comands VARCHAR(255) NOT NULL
    )

    INSERT INTO camel (genus_animal,name,birthday,comands)
    VALUES (2,'Ида','2020-01-02','seat run eat load-cargo'),
    (2,'Твист','2021-01-02','seat run eat load-cargo'),
    (2,'Ланцелот','2023-01-02','seat run eat load-cargo'),
    (2,'Вася','2022-01-02','seat run eat load-cargo'),
    (2,'Джаред','2024-01-02','seat run eat load-cargo');

    CREATE TABLE donkey  (
    id INT PRIMARY KEY AUTO_INCREMENT,
    genus_animal INT NOT NULL,
    FOREIGN KEY (genus_animal) REFERENCES petanimal (id),
    name VARCHAR(50) NOT NULL,
    birthday DATE NOT NULL,
    comands VARCHAR(255) NOT NULL
    )

    INSERT INTO donkey (genus_animal,name,birthday,comands)
    VALUES (3,'Альтаир','2020-01-02','seat run eat load-cargo'),
    (3,'Хэммонд','2021-01-02','seat run eat load-cargo'),
    (3,'Геката','2023-01-02','seat run eat load-cargo'),
    (3,'Рихтер','2022-01-02','seat run eat load-cargo'),
    (3,'Вудди','2024-01-02','seat run eat load-cargo');

### Удалив из таблицы верблюдов, т.к. верблюдов решили перевезти в другой питомник на зимовку.
### Объединить таблицы лошади, и ослы в одну таблицу. 
___
    DROP TABLE camel;

    CREATE TABLE allpetanimal
    AS( SELECT *
    FROM donkey
    UNION
    SELECT *
    FROM horse);
 
### Создать новую таблицу “молодые животные” в которую попадут все животные старше 1 года, но младше 3 лет 
### и в отдельном столбце с точностью до месяца подсчитать возраст животных в новой таблице
___
      CREATE TABLE younganimal
      AS (SELECT id, genus_animal,name, comands, (EXTRACT(YEAR FROM curdate())  - EXTRACT(YEAR FROM donkey.birthday)) 
      * 12 +
      (CASE WHEN  EXTRACT(MONTH FROM curdate()) < EXTRACT(MONTH FROM donkey.birthday)
      THEN EXTRACT(MONTH FROM donkey.birthday) - EXTRACT(MONTH FROM curdate())
      WHEN  EXTRACT(MONTH FROM curdate()) > EXTRACT(MONTH FROM donkey.birthday)
      THEN EXTRACT(MONTH FROM curdate()) - EXTRACT(MONTH FROM donkey.birthday)
      WHEN EXTRACT(MONTH FROM curdate()) = EXTRACT(MONTH FROM donkey.birthday)
      THEN 0

  	END) AS age_mounth
  	FROM donkey
  	WHERE EXTRACT(YEAR FROM curdate()) - EXTRACT(YEAR FROM  donkey.birthday) BETWEEN 1 AND 3
  	
  	UNION 
  	
  	SELECT id, genus_animal,name, comands, (EXTRACT(YEAR FROM curdate())  - EXTRACT(YEAR FROM horse.birthday)) * 12 +
  	(CASE WHEN  EXTRACT(MONTH FROM curdate()) < EXTRACT(MONTH FROM horse.birthday)
  			THEN EXTRACT(MONTH FROM horse.birthday) - EXTRACT(MONTH FROM curdate())
      			WHEN  EXTRACT(MONTH FROM curdate()) > EXTRACT(MONTH FROM horse.birthday)
      			THEN EXTRACT(MONTH FROM curdate()) - EXTRACT(MONTH FROM horse.birthday)
      			WHEN EXTRACT(MONTH FROM curdate()) = EXTRACT(MONTH FROM horse.birthday)
      			THEN 0
      		
  	END) AS age_mounth
  	FROM horse
  	WHERE EXTRACT(YEAR FROM curdate()) - EXTRACT(YEAR FROM  horse.birthday) BETWEEN 1 AND 3
  	
  	UNION
  	
  	SELECT id, genus_animal,name, comands, (EXTRACT(YEAR FROM curdate())  - EXTRACT(YEAR FROM cat.birthday)) * 12 +
  	(CASE WHEN  EXTRACT(MONTH FROM curdate()) < EXTRACT(MONTH FROM cat.birthday)
  			THEN EXTRACT(MONTH FROM cat.birthday) - EXTRACT(MONTH FROM curdate())
      			WHEN  EXTRACT(MONTH FROM curdate()) > EXTRACT(MONTH FROM cat.birthday)
      			THEN EXTRACT(MONTH FROM curdate()) - EXTRACT(MONTH FROM cat.birthday)
      			WHEN EXTRACT(MONTH FROM curdate()) = EXTRACT(MONTH FROM cat.birthday)
      			THEN 0
      		
  	END) AS age_mounth
  	FROM cat
  	WHERE EXTRACT(YEAR FROM curdate()) - EXTRACT(YEAR FROM  cat.birthday) BETWEEN 1 AND 3
  	
  	UNION

  	SELECT id, genus_animal,name, comands, (EXTRACT(YEAR FROM curdate())  - EXTRACT(YEAR FROM dog.birthday)) * 12 +
  	(CASE WHEN  EXTRACT(MONTH FROM curdate()) < EXTRACT(MONTH FROM dog.birthday)
  			THEN EXTRACT(MONTH FROM dog.birthday) - EXTRACT(MONTH FROM curdate())
      			WHEN  EXTRACT(MONTH FROM curdate()) > EXTRACT(MONTH FROM dog.birthday)
      			THEN EXTRACT(MONTH FROM curdate()) - EXTRACT(MONTH FROM dog.birthday)
      			WHEN EXTRACT(MONTH FROM curdate()) = EXTRACT(MONTH FROM dog.birthday)
      			THEN 0
      		
  	END) AS age_mounth
  	FROM dog
  	WHERE EXTRACT(YEAR FROM curdate()) - EXTRACT(YEAR FROM  dog.birthday) BETWEEN 1 AND 3
  	
  	UNION
  	
  	SELECT id, genus_animal,name, comands, (EXTRACT(YEAR FROM curdate())  - EXTRACT(YEAR FROM hamster.birthday)) * 12 +
  	(CASE WHEN  EXTRACT(MONTH FROM curdate()) < EXTRACT(MONTH FROM hamster.birthday)
  			THEN EXTRACT(MONTH FROM hamster.birthday) - EXTRACT(MONTH FROM curdate())
      			WHEN  EXTRACT(MONTH FROM curdate()) > EXTRACT(MONTH FROM hamster.birthday)
      			THEN EXTRACT(MONTH FROM curdate()) - EXTRACT(MONTH FROM hamster.birthday)
      			WHEN EXTRACT(MONTH FROM curdate()) = EXTRACT(MONTH FROM hamster.birthday)
      			THEN 0
      		
  	END) AS age_mounth
  	FROM hamster
  	WHERE EXTRACT(YEAR FROM curdate()) - EXTRACT(YEAR FROM  hamster.birthday) BETWEEN 1 AND 3
  	
  	);
### Объединить все таблицы в одну, при этом сохраняя поля, указывающие на прошлую принадлежность к старым таблицам.
    CREATE TABLE all_table
    AS(SELECT * FROM cat UNION
    SELECT * FROM dog UNION
    SELECT * FROM horse UNION
    SELECT * FROM hamster UNION
    SELECT * FROM donkey);

