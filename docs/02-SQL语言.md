# 02-SQL语言

创建表
```sql
CREATE TABLE weather (
    city            varchar(80),
    temp_lo         int,           -- low temperature
    temp_hi         int,           -- high temperature
    prcp            real,          -- precipitation
    date            date
);
CREATE TABLE cities (
    name            varchar(80),
    location        point
);
drop table weather;
drop table cities;
```

插入数据
```sql
INSERT INTO weather VALUES ('San Francisco', 46, 50, 0.25, '1994-11-27');
INSERT INTO cities VALUES ('San Francisco', '(-194.0, 53.0)');

-- 指定列
INSERT INTO weather (city, temp_lo, temp_hi, prcp, date)
    VALUES ('San Francisco', 43, 57, 0.0, '1994-11-29');
```

查询数据
```sql
SELECT * FROM weather;
SELECT city, prcp from weather;
SELECT city, (temp_hi+temp_lo)/2 AS temp_avg, date FROM weather;
SELECT * FROM weather WHERE city = 'San Francisco' AND prcp > 0.0;
SELECT * FROM weather ORDER BY city;
SELECT * FROM weather ORDER BY city, temp_lo;
SELECT DISTINCT city FROM weather ORDER BY city;
```

Join 查询
```sql
SELECT * FROM weather JOIN cities ON city = name;
SELECT city, temp_lo, temp_hi, prcp, date, location FROM weather JOIN cities ON city = name;
SELECT weather.city, weather.temp_lo, weather.temp_hi, weather.prcp, weather.date, cities.location FROM weather JOIN cities ON weather.city = cities.name;
SELECT * FROM weather, cities WHERE city = name;
SELECT * FROM weather LEFT OUTER JOIN cities ON weather.city = cities.name;
-- self join
SELECT w1.city, w1.temp_lo AS low, w1.temp_hi AS high,
       w2.city, w2.temp_lo AS low, w2.temp_hi AS high
    FROM weather w1 JOIN weather w2
        ON w1.temp_lo < w2.temp_lo AND w1.temp_hi > w2.temp_hi;
```

聚合查询
一般的聚合查询：count,sum,avg,max,min
```sql
SELECT max(temp_lo) FROM weather;
SELECT city FROM weather WHERE temp_lo = (SELECT max(temp_lo) FROM weather);
SELECT city,max(temp_lo) FROM weather GROUP BY city HAVING max(temp_lo)<40;
```

更新
```sql
UPDATE weather SET temp_hi = temp_hi - 2,  temp_lo = temp_lo - 2 WHERE date > '1994-11-28';
```

删除
```sql
DELETE FROM weather WHERE city='Hayward';
DELETE FROM weather; -- delete all
```



