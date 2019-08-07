# mysql学习1-3天总结（踩坑）

## Navicat

1. 创建连接时出现1251错误  
参考 [此解决方案](https://blog.csdn.net/qq_36068954/article/details/80175755)  
**具体步骤：** 
   - cmd打开mysql
   -  输入![cannotfind](./1251solution.png)  
  >>>1.'root'   为你自己定义的用户名
2.'localhost' 指的是用户开放的IP，可以是'localhost'(仅本机访问，相当于127.0.0.1)，可以是具体的'*.*.*.*'(具体某一IP)，也可以是 '%' (所有IP均可访问)
3.'password' 是你想使用的用户密码

## 查询语句  
1. **select**   

```mysql
SELECT column_name
FROM certain_table;
```

> 检索所有列时，可用通配符*，e.g: SELECT * FROM certain_table;

+ 去重语句(DISTINCT)

```mysql
SELECT DISTINCT certain_column
FROM certain_table;
```

> 去重后，所选列中相同的内容只显示一次



+ 选择前n个语句(LIMIT)

```mysql
SELECT certain_colomn
FROM certain_table
LIMIT 5 OFFSET 5;/*LIMIT 5,5;*/
```

> 上述代码返回从第五行开始的五行数据。注：行数从0开始

2. **case  when… then …end**

参考： [https://www.jianshu.com/p/113b21734353 ](https://www.jianshu.com/p/113b21734353 )

```mysql
CASE <单值表达式>
    WHEN <表达式值> THEN <SQL语句或返回值>
    WHEN <表达式值> THEN <SQL语句或返回值>
   ...
    WHEN <表达式值> THEN <SQL语句或返回值>
    ELSE <SQL语句或返回值>
END
```

 用法一：

```mysql
SELECT *,
       (CASE grade  
            WHEN 'A' THEN '优秀'
            WHEN 'B' THEN '良好'
        	when 'C' THEN '及格'
            ELSE '不及格'
        END) AS grade_result
FROM certain_table;
```

用法二：

```mysql
SELECT *,
       (CASE  
            WHEN grade='A' THEN '优秀'
            WHEN grade='B' THEN '良好'
        	when grade='C' THEN '及格'
            ELSE '不及格'
        END) AS grade_result
FROM certain_table;
```

3. **where**

| 操作符  |            说明            |
| :-----: | :------------------------: |
|    =    |            等于            |
|   <>    |           不等于           |
|   !=    |           不等于           |
|    <    |            小于            |
|   <=    |          小于等于          |
|   !<    |           不小于           |
|    >    |            大于            |
|   \>=   |          大于等于          |
|   !>    |           不大于           |
| BETWEEN |     在指定的两个值之间     |
| IS NULL |          为NULL值          |
|   AND   |  检索满足所有给定条件的行  |
|   OR    |  检索匹配任意给定条件的行  |
|   IN    | 指定要匹配值的清单的关键字 |
|   NOT   |    否定其后条件的关键字    |

e.g

```mysql
SELECT expression1, expression2
FROM certain_table
WHERE expression1 < 10;
```

+ 通配符  

  + %

  ```mysql
  SELECT expression1, expression2
  FROM certain_table
  WHERE expression1 LIKE 'Hello%';
  ```

  > 检查任意以Hello起头的词

  + _

  > 用法同（%），但只可匹配一个字符

  + []

  ```mysql
  SELECT expression1, expression2
  FROM certain_table
  WHERE expression1 REGEXP '^[AB]';
  ```

  > 匹配以A或B开头的词

4. **GROUP BY**

   SQL聚集函数

   |  函数   |      说明      |
   | :-----: | :------------: |
   |  AVG()  | 返回某列平均值 |
   | COUNT() |  返回某列行数  |
   |  MAX()  | 返回某列最大值 |
   |  MIN()  | 返回某列最小值 |
   |  SUM()  |  返回某列之和  |

   ```mysql
   SELECT expression1, COUNT(*) AS nums
   FROM certain_table
   GROUP BY expression1
   HAVING COUNT(*) >= 2;
   ```

   > 筛选出行数大于2的列，且按expression1排序     

5. **ORDER BY**

  ```mysql
SELECT expression1, expression2
  FROM certain_table
ORDER BY expression1;
  ```

  > 可用相对列位置排序，若降序排序需指定DESC关键字   
  
## 项目一

   创建 email表，并插入如下三行数据

+----+---------+

| Id | Email   |

+----+---------+

| 1  | a@b.com |

| 2  | c@d.com |

| 3  | a@b.com |

+----+---------+



编写一个 SQL 查询，查找 Email 表中所有重复的电子邮箱。

根据以上输入，你的查询应返回以下结果：

+---------+

| Email   |

+---------+

| a@b.com |

+---------+

说明：所有电子邮箱都是小写字母。

![结果](C:\Users\lenovo\Desktop\mysql 1-3days\项目一.png)

   ## 项目二

创建如下 World 表

+-----------------+------------+------------+--------------+---------------+

| name            | continent  | area       | population   | gdp           |

+-----------------+------------+------------+--------------+---------------+

| Afghanistan     | Asia       | 652230     | 25500100     | 20343000      |

| Albania         | Europe     | 28748      | 2831741      | 12960000      |

| Algeria         | Africa     | 2381741    | 37100000     | 188681000     |

| Andorra         | Europe     | 468        | 78115        | 3712000       |

| Angola          | Africa     | 1246700    | 20609294     | 100990000     |

+-----------------+------------+------------+--------------+---------------+

如果一个国家的面积超过300万平方公里，或者(人口超过2500万并且gdp超过2000万)，那么这个国家就是大国家。

编写一个SQL查询，输出表中所有大国家的名称、人口和面积。

例如，根据上表，我们应该输出:

+--------------+-------------+--------------+

| name         | population  | area         |

+--------------+-------------+--------------+

| Afghanistan  | 25500100    | 652230       |

| Algeria      | 37100000    | 2381741      |

+--------------+-------------+--------------+

![结果](C:\Users\lenovo\Desktop\mysql 1-3days\项目二.png)

   

