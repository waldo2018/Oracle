# Oracle 学习笔记

## 第一章	Oracle 数据库

### Oracle 数据结构体系介绍

 1. 平时所说的Oracle 数据库其实是Oracle 数据库管理系统。**Oracle database manager system**

 2. 它是由Oracle 数据库和Oracle 实例组成的。

 3. Oracle 数据库指的是计算机系统文件的集合，这些文件组织在一起，成为一个逻辑整体，即为Oracle 数据库。

### Oracle 安装过程

1. [windows 10 安装手顺](https://blog.csdn.net/jffhy2017/article/details/54562385)  **注意：在选择安装的时候字符集最好选择通用的UTF8**

 	2. [Linux 安装手顺]
 	3. [Mac 安装手顺]

## 第二章	基本SQL语句

### Sql 语句分类

​	SQL 语句分为以下三种类型

 	1. DML : Data Manipulation Language  数据操纵语言
     - 用于查询或修改数据记录，包括以下SQL 语句
       - INSERT ： 添加数据到数据库中
       - UPDATE：修改数据库中的数据
       - DELETE：删除数据库中的数据
       - SELECT：选择查询数据
 	2. DDL：Data Definition Language 数据定义语言
     - 用于定义数据库的结构，比如创建，修改，或删除数据库对象。
       - CREATE TABLE：创建数据表
       - ALTER TABLE：更改表的结构，添加，删除，修改列长度
       - DROP TABLE：删除表
       - CREATE INDEX：在表上建立索引
       - DROP INDEX：删除索引
 	3. DCL：Data Control Language 数据控制语言
     - 用来控制数据库的访问
       - GRANT：授予访问权限
       - REVOKE：撤销访问权限
       - COMMIT：提交事务处理
       - ROLLBACK：事务处理回退
       - SAVEPOINT：设置保存点
       - LOCK：对数据库 特定部分进行锁定

### Select 语句

 1. ``` plsql
    SELECT	*|{[DISTINCT] column|expression [alias],...}
    FROM	table;
    ```

    - SELECT ：标识
    - FROM：从哪个表中选择

 2. 选择全部列

    ``` plsql
    SQL> select * from departments;
    
    DEPARTMENT_ID DEPARTMENT_NAME                MANAGER_ID LOCATION_ID
    ------------- ------------------------------ ---------- -----------
              190 Contracting                                      1700
               10 Administration                        200        1700
               20 Marketing                             201        1800
               50 Shipping                              124        1500
               60 IT                                    103        1400
               80 Sales                                 149        2500
               90 Executive                             100        1700
              110 Accounting                            205        1700
    
    8 rows selected
    ```

 3. 选择特定的列

    ``` plsql
    SQL> select department_id, department_name from departments;
    
    DEPARTMENT_ID DEPARTMENT_NAME
    ------------- ------------------------------
              190 Contracting
               10 Administration
               20 Marketing
               50 Shipping
               60 IT
               80 Sales
               90 Executive
              110 Accounting
    
    8 rows selected
    ```

    **注意**：SQL语言不区分大小写；SQL 可以写在一行或多行；关键字不能被缩写，也不能分行；各字句一般要分行写；使用缩进提高语言的可读性。

 4. 算数运算符

    | 操作符 | 描述 |
    | ------ | ---- |
    | +      | 加   |
    | -      | 减   |
    | x      | 乘   |
    | /      | 除   |

 5. 使用算数运算符

    ``` plsql
    SQL> select last_name, salary, salary + 300                  #在select过程中使用算数运算符
      2  from employees;
    
    LAST_NAME                     SALARY SALARY+300
    ------------------------- ---------- ----------
    King                        24000.00      24300
    Kochhar                     17000.00      17300
    De Haan                     17000.00      17300
    Hunold                       9000.00       9300
    Ernst                        6000.00       6300
    Lorentz                      4200.00       4500
    Mourgos                      5800.00       6100
    Rajs                         3500.00       3800
    Davies                       3100.00       3400
    Matos                        2600.00       2900
    Vargas                       2500.00       2800
    Zlotkey                     10500.00      10800
    Abel                        11000.00      11300
    Taylor                       8600.00       8900
    Grant                        7000.00       7300
    Whalen                       4400.00       4700
    Hartstein                   13000.00      13300
    Fay                          6000.00       6300
    Higgins                     12000.00      12300
    Gietz                        8300.00       8600
    
    20 rows selected
    ```

    **操作符的优先等级** 跟最基本的数学运算是一致的。

 6. 操作符运算有限等级的例子

    ``` plsql
    SQL> select last_name, salary, 12 * salary + 100, 12 * (salary + 100)
      2  from employees;
    
    LAST_NAME                     SALARY 12*SALARY+100 12*(SALARY+100)
    ------------------------- ---------- ------------- ---------------
    King                        24000.00        288100          289200
    Kochhar                     17000.00        204100          205200
    ```

 7. 定义空值，空值是无效的，未指定的，未知的或不可预知的值。不是空格或0

    包含空值的数学表达式的值都是空值 ，不论是加，还是乘，都为空值

    ``` plsql
    SQL> select last_name, job_id, salary, commission_pct, 12 * salary * commission_pct
      2  from employees;
    
    LAST_NAME                 JOB_ID         SALARY COMMISSION_PCT 12*SALARY*COMMISSION_PCT
    ------------------------- ---------- ---------- -------------- ------------------------
    King                      AD_PRES      24000.00                
    Kochhar                   AD_VP        17000.00                
    De Haan                   AD_VP        17000.00                
    Hunold                    IT_PROG       9000.00                
    Ernst                     IT_PROG       6000.00                
    Lorentz                   IT_PROG       4200.00                
    Mourgos                   ST_MAN        5800.00                
    Rajs                      ST_CLERK      3500.00                
    Davies                    ST_CLERK      3100.00                
    Matos                     ST_CLERK      2600.00                
    Vargas                    ST_CLERK      2500.00                
    Zlotkey                   SA_MAN       10500.00           0.20                    25200
    ```

 8. 列的别名

    - 定义

      - 重命名一个列。
      - 方便计算
      - 紧跟列名，**也可以在列名和别名之间加入AS，别名使用双引号，以便在别名中包含特殊的字符或空格并区分大小写。**

    - 实例

      ``` plsql
      SQL> select last_name "LastName", job_id as "job\&id"  #\标识转义字符，
        2  from employees;
      
      LastName                  job
      ------------------------- ----------
      King                      AD_PRES
      Kochhar                   AD_VP
      De Haan                   AD_VP
      ```

 9. 连接符

    - 定义

      - 把列与列，列于字符连接在一起。
      - 用'||'表示。，用来合成列

    - 实例

      ``` plsql
      SQL> select last_name || job_id, last_name, job_id
        2  from employees;
      
      LAST_NAME||JOB_ID                   LAST_NAME                 JOB_ID
      ----------------------------------- ------------------------- ----------
      KingAD_PRES                         King                      AD_PRES
      KochharAD_VP                        Kochhar                   AD_VP
      De HaanAD_VP                        De Haan                   AD_VP
      ```

 10. 字符串

     - 字符串可以是select语句中的一个字符，数字，日期

     - 日期和字符只能在单引号中出现，每当返回一行时字符串被输出一次。

       ``` plsql
       SQL> select last_name || ' is a ' || job_id as "Employee Details" from employees;
       
       Employee Details
       -----------------------------------------
       King is a AD_PRES
       Kochhar is a AD_VP
       ```

 11. 重复行

     - 默认的情况下，查询会返回全部行，包括重复行。但是可以在SELECT 子句中使用关键字`distinct` 删除重复行。

        ``` plsql
       SQL> select department_id from employees;
       
       DEPARTMENT_ID
       -------------
                  90
                  90
                  90
                  60
                  60
                  60
                  50
       ```

       ``` plsql
       SQL> select distinct department_id from employees;    # 使用DISTINCT删除掉重复行
       
       DEPARTMENT_ID
       -------------
       
                  90
                  20
                 110
                  50
                  80
                  60
                  10
       
       8 rows selected
       ```

 12. SQL 与SQL*PLUS 的区别

     - ![](F:\gitHub\Oracle\assets\sqlAndsqlplus.jpg)

       | SQL                                          | SQL*PLUS                                 |
       | -------------------------------------------- | ---------------------------------------- |
       | 一种语言                                     | 一种环境                                 |
       | ANSI 标准                                    | Oracle 的特性之一                        |
       | 关键字不能够缩写                             | 关键字可以缩写                           |
       | 使用语句空值数据库中表的定义信息和表中的数据 | 命令不能够改变数据库中数据的值，集中运行 |

     - 显示表的结构

       `DESC tablename`

     ---

     

## 第三章	过滤和排序数据

### 过滤

 1. 过滤，使用where 语句将不满足条件的行过滤掉。

 2. ``` plsql
    SELECT	*|{[DISTINCT] column|expression [alias],...}
    FROM	table
    [WHERE	condition(s)];
    ```

 3. 实例

    	- 搜索出部门为90的相关员工信息

    ``` plsql
    SQL> select employee_id, last_name, job_id, department_id 
      2  from employees
      3  where department_id = 90;
    
    EMPLOYEE_ID LAST_NAME                 JOB_ID     DEPARTMENT_ID
    ----------- ------------------------- ---------- -------------
            100 King                      AD_PRES               90
            101 Kochhar                   AD_VP                 90
            102 De Haan                   AD_VP                 90
    ```

    - 使用where 语句的时候要注意字符和日期的用法。
      - 字符和日期要包含在单引号中。
      - 字符大小写敏感，日期格式敏感
      - ~~默认的日期格式是DD-MON-RR~~ 

    ``` plsql
    SQL> select last_name, job_id, department_id 
      2  from employees
      3  where last_name = 'Whalen'
      4  ;
    
    LAST_NAME                 JOB_ID     DEPARTMENT_ID
    ------------------------- ---------- -------------
    Whalen                    AD_ASST               10
    ```

    ``` plsql
    SQL> select last_name, job_id, department_id
      2  from employees
      3  where hire_date = '1999/05/24';
    
    LAST_NAME                 JOB_ID     DEPARTMENT_ID
    ------------------------- ---------- -------------
    Grant                     SA_REP  
    
    ```

	4. 比较运算

    | 操作符 | 含义       |
    | ------ | ---------- |
    | =      | 等于       |
    | >      | 大于       |
    | >=     | 大于、等于 |
    | <      | 小于       |
    | <=     | 小于、等于 |
    | <>     | 不等于     |

    ``` plsql
    SQL> select last_name, salary 
      2  from employees
      3  where salary < 3000;
    
    LAST_NAME                     SALARY
    ------------------------- ----------
    Matos                        2600.00
    Vargas                       2500.00
    ```

    其他比较运算

    | 操作符            | 含义                     |
    | ----------------- | ------------------------ |
    | BETWEEN ...AND... | 在两个值之间（包含边界） |
    | IN(SET)           | 等于列表中的一个值       |
    | LIKE              | 模糊查询                 |
    | IS NULL           | 空值                     |

    ``` plsql
    SQL> select last_name, salary 
      2  from employees
      3  where salary between 2000 and 3000;
    
    LAST_NAME                     SALARY
    ------------------------- ----------
    Matos                        2600.00
    Vargas                       2500.00
    ```

    ``` plsql
    SQL> select last_name, salary
      2  from employees
      3  where salary in (2500, 2600);
    
    LAST_NAME                     SALARY
    ------------------------- ----------
    Matos                        2600.00
    Vargas                       2500.00
    ```

    ``` plsql
    SQL> select last_name, salary
      2  from employees
      3  where salary like '2%';				# % 代表0个或多个字符 _ 代表一个字符
    
    LAST_NAME                     SALARY
    ------------------------- ----------
    King                        24000.00
    Matos                        2600.00
    Vargas                       2500.00
    
    ```

    ``` plsql
    SELECT job_id
    FROM   jobs
    WHERE  job_id LIKE 'IT\_%' escape '\'      #escape 转义符
    
    SQL> /
    
    JOB_ID
    ----------
    IT_PROG                                
    ```

    ``` plsql
    SQL> select last_name, manager_id
      2  from employees
      3  where manager_id is null;
    
    LAST_NAME                 MANAGER_ID
    ------------------------- ----------
    King 
    ```

	5. 逻辑运算

    | 操作符 | 含义   |
    | ------ | ------ |
    | AMD    | 逻辑并 |
    | OR     | 逻辑或 |
    | NOT    | 逻辑否 |

    - And 练习

      ``` plsql
      select employee_id, last_name, job_id, salary
      from employees
      where salary >= 10000
      and job_id like '%MAN%'
      
      SQL> /
      
      EMPLOYEE_ID LAST_NAME                 JOB_ID         SALARY
      ----------- ------------------------- ---------- ----------
              149 Zlotkey                   SA_MAN       10500.00
              201 Hartstein                 MK_MAN       13000.00
      ```

    - Or 练习

      ``` plsql
      SQL> select employee_id, last_name, job_id, salary
        2  from employees
        3  where salary >= 10000
        4  or job_id like '%MAN%';
      
      EMPLOYEE_ID LAST_NAME                 JOB_ID         SALARY
      ----------- ------------------------- ---------- ----------
              100 King                      AD_PRES      24000.00
              101 Kochhar                   AD_VP        17000.00
              102 De Haan                   AD_VP        17000.00
              124 Mourgos                   ST_MAN        5800.00
              149 Zlotkey                   SA_MAN       10500.00
              174 Abel                      SA_REP       11000.00
              201 Hartstein                 MK_MAN       13000.00
              205 Higgins                   AC_MGR       12000.00
      
      8 rows selected
      ```

    - NOT 练习

      ``` plsql
      SQL> SELECT last_name, job_id
        2  FROM   employees
        3  WHERE  job_id
        4         NOT IN ('IT_PROG', 'ST_CLERK', 'SA_REP');
      
      LAST_NAME                 JOB_ID
      ------------------------- ----------
      King                      AD_PRES
      Kochhar                   AD_VP
      De Haan                   AD_VP
      Mourgos                   ST_MAN
      Zlotkey                   SA_MAN
      Whalen                    AD_ASST
      Hartstein                 MK_MAN
      Fay                       MK_REP
      Higgins                   AC_MGR
      Gietz                     AC_ACCOUNT
      
      10 rows selected
      ```

	6. 优先级

    | 优先级 |                     |
    | ------ | ------------------- |
    | 1      | 算术运算符          |
    | 2      | 连接符              |
    | 3      | 比较符              |
    | 4      | IS [NOT] , LIKE NOT |
    | 5      | NOT BETWEEN         |
    | 6      | NOT                 |
    | 7      | AND                 |
    | 8      | OR                  |

    可以使用括号来改变优先级。

### 排序

 1. Order by 子句，用在select 语句的结尾。

    - ASC(ascend)：升序

    - DESC(descend)：降序

      ``` plsql
      SQL> select last_name, job_id, department_id, hire_date
        2  from employees
        3  order by hire_date;
      
      LAST_NAME                 JOB_ID     DEPARTMENT_ID HIRE_DATE
      ------------------------- ---------- ------------- -----------
      King                      AD_PRES               90 1987/06/17
      Whalen                    AD_ASST               10 1987/09/17
      Kochhar                   AD_VP                 90 1989/09/21
      Hunold                    IT_PROG               60 1990/01/03
      ```

      ``` plsql
      SQL> select last_name, job_id, department_id, hire_date
        2  from employees
        3  order by hire_date desc
        4  ;
      
      LAST_NAME                 JOB_ID     DEPARTMENT_ID HIRE_DATE
      ------------------------- ---------- ------------- -----------
      Zlotkey                   SA_MAN                80 2000/01/29
      Mourgos                   ST_MAN                50 1999/11/16
      Grant                     SA_REP                   1999/05/24
      Lorentz                   IT_PROG               60 1999/02/07
      Vargas                    ST_CLERK              50 1998/07/09
      Taylor                    SA_REP                80 1998/03/24
      ```

      按别名排列

      ``` plsql
      SQL> select last_name, job_id, 12 *  salary as year_salary
        2  from employees
        3  order by year_salary;
      
      LAST_NAME                 JOB_ID     YEAR_SALARY
      ------------------------- ---------- -----------
      Vargas                    ST_CLERK         30000
      Matos                     ST_CLERK         31200
      Davies                    ST_CLERK         37200
      Rajs                      ST_CLERK         42000
      Lorentz                   IT_PROG          50400
      Whalen                    AD_ASST          52800
      Mourgos                   ST_MAN           69600
      Ernst                     IT_PROG          72000
      ```

      多个列排列

      ``` plsql
      SQL> SELECT last_name, department_id, salary
        2  FROM   employees
        3  ORDER BY department_id, salary DESC;
      
      LAST_NAME                 DEPARTMENT_ID     SALARY
      ------------------------- ------------- ----------
      Whalen                               10    4400.00
      Hartstein                            20   13000.00
      Fay                                  20    6000.00
      Mourgos                              50    5800.00
      Rajs                                 50    3500.00
      Davies                               50    3100.00
      Matos                                50    2600.00
      ```

      

## 第四章	函数

### 单行函数

 - 单行函数严格来讲并不属于SQL语法，但是针对不同的数据库，首先SQL这个标准一定会共同遵守的，但是每个数据库都有每一个数据库自己定义的函数，利用函数，可以完成一些指定的操作功能。那么在Oracle之中单行函数一共分为5类：

    - **字符串函数**

       - 大小写控制字符

          - LOWER ：将字符全部转换乘小写

          - UPPER：将字符全部转换成大写

          - INITCAP：首字母转换乘大写

            ``` plsql
            SQL> select last_name, lower(last_name) as "lastname", upper(last_name) as "LASTNAME", initcap(last_name) as "LastName"
              2  from employees;
            
            LAST_NAME                 lastname                  LASTNAME                  LastName
            ------------------------- ------------------------- ------------------------- -------------------------
            King                      king                      KING                      King
            Kochhar                   kochhar                   KOCHHAR                   Kochhar
            De Haan                   de haan                   DE HAAN                   De Haan
            ```

            

       - 字符控制函数

          - CONCAT：连接两个单词。作用跟“||”类似

            ``` plsql
            SQL> select last_name, job_id, concat(last_name, job_id) as str1      # 将两个字符串拼接起来
              2  from employees;
            
            LAST_NAME                 JOB_ID     STR1
            ------------------------- ---------- -----------------------------------
            King                      AD_PRES    KingAD_PRES
            Kochhar                   AD_VP      KochharAD_VP
            ```

            

          - SUBSTR：截取字符串。

            ``` plsql
            SQL> select 'helloworld', substr('helloworld',1,5) from dual;  # 从字符串中截取1-5位
            
            'HELLOWORLD' SUBSTR('HELLOWORLD',1,5)
            ------------ ------------------------
            helloworld   hello
            ```

            

          - LENGTH：字符串长度

            ``` plsql
            SQL> select length('helloworld') as length from dual;
            
                LENGTH
            ----------
                    10
            ```

            

          - INSTR：在字符串中的位置。

            ``` plsql
            SQL> select instr('helloworld', 'o') from dual;    # 在字符串中第一次出现的位置
            
            INSTR('HELLOWORLD','O')
            -----------------------
                                  5
            ```

            

          - LPAD | RPAD：在左边添加字符 | 在右边添加字符

            ``` plsql
            SQL> select last_name, salary, lpad(salary, 10, '*') as lpad_salary, rpad(salary, 10, '*') as rpad_salary
              2  from employees;
            
            LAST_NAME                     SALARY LPAD_SALARY                              RPAD_SALARY
            ------------------------- ---------- ---------------------------------------- ----------------------------------------
            King                        24000.00 *****24000                               24000*****
            Kochhar                     17000.00 *****17000                               17000*****
            ```

            

          - TRIM：从字符串中去掉某个字符

            ``` plsql
            SQL> select trim('h' from 'helloworld') from dual;
            
            TRIM('H'FROM'HELLOWORLD')
            -------------------------
            elloworld
            ```

            

          - REPLACE：从字符串中替换某个字符

            ``` plsql
            SQL> select replace('abcd', 'b', 'm') from dual;
            
            REPLACE('ABCD','B','M')
            -----------------------
            amcd
            ```

            

    - **数字函数**

       - ROUND：四舍五入

       - TRUNC：截断

       - MOD：求余

         ``` plsql
         SQL> select round(45.926, 2), trunc(45.926, 2), MOD(1600, 300) from dual;
         
         ROUND(45.926,2) TRUNC(45.926,2) MOD(1600,300)
         --------------- --------------- -------------
                   45.93           45.92           100
         ```

         

    - **日期函数** 数据库中的日期实际上含有两个值，日期和时间

       - 日期的数学运算

          - 在日期上加上或减去一个日期，其结果仍为日期。
          - 两个日期相减返回日期之间相差的天数。
          - 可以用数字除以24来向日期中加上或减去天数。

         ``` plsql
         SQL> select last_name, hire_date, (sysdate - hire_date)/7 as weeks        # 两个数据相减仍为日期，天数除以7 ，显示星期的个数
           2  from employees
           3  where department_id = 90;
         
         LAST_NAME                 HIRE_DATE        WEEKS
         ------------------------- ----------- ----------
         King                      1987/06/17  1724.82611
         Kochhar                   1989/09/21  1606.68326
         De Haan                   1993/01/13  1433.82611
         ```

         | 函数           | 描述                             |
         | -------------- | -------------------------------- |
         | MONTHS_BETWEEN | 两个日期相差的月数               |
         | ADD_MONTHS     | 向日期中加上若干的月数           |
         | NEXT_DAY       | 指定日期的下一个星期，对应的日期 |
         | LAST_DAY       | 本月的最后一天                   |
         | ROUND          | 日期四舍五入                     |
         | TRUNC          | 日期截断                         |

         - 两个日期相差的月数的例子

           ``` plsql
           SQL> select months_between(sysdate, '2019/01/12') from dual;
           
           MONTHS_BETWEEN(SYSDATE,'2019/01/12')
           ------------------------------------
                               17.8318499850657
           ```

         - 向日期中加上若干个月数

           ``` plsql
           SQL> select add_months(sysdate, 5) from dual;
           
           ADD_MONTHS(SYSDATE,5)
           ---------------------
           2020/12/06 18:54:54
           ```

         - 指定日期的下一个星期，对应的日期

           ``` plsql
           SQL> select next_day(sysdate, '金曜日') from dual;　　　　　　# 从sysdate日期开始，下一个星期五是几号
           
           NEXT_DAY(SYSDATE,'金曜日')
           -----------------------
           2020/07/10 18:58:11
           ```

         - 本月最后一天

           ``` plsql
           SQL> select last_day(sysdate) from dual;
           
           LAST_DAY(SYSDATE)
           -----------------
           2020/07/31 19:00:
           ```

         - 日期的四舍五入

           ``` plsql
           SQL> select round(sysdate, 'YEAR') from dual;
           
           ROUND(SYSDATE,'YEAR')
           ---------------------
           2021/01/01
           ```

         - 日期截断

           ``` plsql
           SQL> select trunc(sysdate, 'MONTH') from dual;       # 从month之后的日期和时间就不在需要
           
           TRUNC(SYSDATE,'MONTH')
           ----------------------
           2020/07/01
           
           SQL> select sysdate from dual;
           
           SYSDATE
           -----------
           2020/07/06
           ```

           

    - **转换函数** 

       - 隐性转换

         ![](assets\2020-07-06 190800.jpg)

       - 显性转换

         ![](assets\2020-07-06 190931.jpg)

         - TO_CHAR 函数对日期的转换

           - 格式

             - 必须包含在单引号中，而且大小写敏感。
             - 可以包含任意的有效的日期格式。
             - 日期之间用逗号隔开。

           - 实例

             ``` plsql
             SQL> select to_char(sysdate,'yyyy-mm-dd hh:mi:ss') from dual;    
             
             TO_CHAR(SYSDATE,'YYYY-MM-DDHH:MI:SS')
             -------------------------------------
             2020-07-06 07:13:31
             ```

             

    - **通用函数**

### 多行函数

## 第五章	多表查询

## 第六章	分组函数

## 第七章	子查询

## 第八章	创建和管理表

## 第九章	数据处理

## 第十章	约束

## 第十一章	视图

## 第十二章	其他数据对象



