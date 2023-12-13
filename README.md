# Базы данных и SQL (семинары)

## SQL – Транзакции. Временные таблицы, управляющие конструкции, циклы

### Задание 1

Создайте функцию, которая принимает кол-во сек и форматирует их в кол-во дней, часов, минут и секунд.

Пример: **123456 ->'1 days 10 hours 17 minutes 36 seconds '**

```sql
DELIMITER $$

CREATE FUNCTION HW_06.sec_to_day(seconds int) 
  RETURNS varchar(250)
  DETERMINISTIC
BEGIN
  DECLARE result varchar(250);
  DECLARE days_value int DEFAULT 0;
  DECLARE hours_value int DEFAULT 0;
  DECLARE minutes_value int DEFAULT 0;

  -- считаем дни
  IF seconds >= 86400 THEN
    SET days_value = seconds DIV 86400;
    SET seconds = seconds % 86400;
  END IF;

  -- считаем часы
  IF seconds >= 3600 THEN
    SET hours_value = seconds DIV 3600;
    SET seconds = seconds % 3600; 
  END IF;

  -- считаем минуты / секунды
  IF seconds >=60 THEN
    SET minutes_value = seconds DIV 60;
    SET seconds = seconds % 60;
  END IF;
  
  SET result = CONCAT(
                    CAST(days_value AS CHAR), ' дней ',
                    CAST(hours_value AS CHAR), ' час ',
                    CAST(minutes_value AS CHAR), 'мин.');

  SET result = CONCAT(result, CAST(seconds AS CHAR), ' сек.');
  
  RETURN result;
END$$

DELIMITER ;
```

Вызов функции:

![](image/img_01.png)

![](image/img_02.png)

![](image/img_03.png)

![](image/img_04.png)


### Задание 2

Выведите только четные числа от 1 до 10 включительно. (Через функцию / процедуру)

Пример: **2,4,6,8,10** (*можно сделать через шаг +  2: х = 2, х+=2*)

Построим ХП с возвращаемым параметром.

```sql
DELIMITER $$ 

CREATE PROCEDURE even_numbers (IN count_number int, OUT result varchar(255))
BEGIN
  DECLARE i int DEFAULT 2;

  SET result = '';

  WHILE i < count_number DO
    SET result = CONCAT(result, CAST(i AS char), ' '); 
    SET i = i + 2;
  END WHILE;

END
$$

DELIMITER ;
```

Выполнение процедуры:

![](image/img_05.png)
