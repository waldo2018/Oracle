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

 1. 单行函数严格来讲并不属于SQL语法，但是针对不同的数据库，首先SQL这个标准一定会共同遵守的，但是每个数据库都有每一个数据库自己定义的函数，利用函数，可以完成一些指定的操作功能。那么在Oracle之中单行函数一共分为5类：

    #### **字符串函数**

       1. 大小写控制字符

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

            

       2. 字符控制函数

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

            

  #### **数字函数**

       - ROUND：四舍五入
    
       - TRUNC：截断
    
       - MOD：求余
    
         ``` plsql
         SQL> select round(45.926, 2), trunc(45.926, 2), MOD(1600, 300) from dual;
         
         ROUND(45.926,2) TRUNC(45.926,2) MOD(1600,300)
         --------------- --------------- -------------
                   45.93           45.92           100
         ```


​         

    #### **日期函数** 数据库中的日期实际上含有两个值，日期和时间
    
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


​           

    #### **转换函数** 
    
       - 隐性转换
    
         <img src="assets\2020-07-06 190800.jpg" style="zoom: 80%;" />
    
       - 显性转换
    
         <img src="assets\2020-07-06 190931.jpg" style="zoom:80%;" />
    
         1. TO_CHAR 函数对日期的转换
    
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
    
         - 日期的格式元素
    
           | YYYY  | 2014                  |
           | ----- | --------------------- |
           | YEAR  | TWO THOUSAND AND FOUR |
           | MM    | 02                    |
           | MONTH | JULY                  |
           | MON   | JUL                   |
           | DY    | MON                   |
           | DAY   | MONDAY                |
           | DD    | 02                    |
    
           ```plsql
           SQL> select to_char(sysdate, 'HH24:MI:SS AM') FROM DUAL;
           
           TO_CHAR(SYSDATE,'HH24:MI:SSAM')
           -------------------------------
           10:15:38 午前
           ```
    
           ``` plsql
           SQL> select last_name, to_char(hire_date, 'dd month yyyy') as "HIREDATE" 
             2  FROM EMPLOYEES;
           
           LAST_NAME                 HIREDATE
           ------------------------- ----------------
           King                      17 6月  1987
           Kochhar                   21 9月  1989	
           ```
    
       2. TO_DATE 函数对字符的转换
    
       3. TO_CHAR 函数对数字的转换
    
         - ``` plsql
           TO_CHAR(NUMBER,'FORMAT_MODEL')	
           ```
    
         - 经常使用到的几种格式
    
           | 9    | 数字         |
           | ---- | ------------ |
           | 0    | 零           |
           | $    | 美元符号     |
           | L    | 本地货币符号 |
           | .    | 小数点       |
           | ,    | 千位符       |
    
         - 使用实例
    
           ``` plsql
           SQL> select to_char(salary, '$99,999,00') SALARY
             2  from employees
             3  where last_name = 'Ernst';
           
           SALARY
           -----------
                $60,00
           ```
    
       4. TO_NUMBER 函数对字符的转换
    
         - ``` plsql
           TO_NUMBER(CHAR, 'FORMAT_MODEL');
           ```


​    
​           

   #### **通用函数**  
    - 这些函数适用与任何数据类型，同时也适用与空值
    
         - NVL(expr1， expr2)
         - NVL2(expr1, expr2, expr3)
         - NULLIF(expr1, expr2)
         - COALESCE(expr1, expr2,... , exprn)

 1. NVL 函数

   - 将空值转化乘一个已知的值：可以是数字，日期，字符。

     ```plsql
     SQL> select last_name, salary, commission_pct, nvl(commission_pct, 0) 
       2  from employees;
     
     LAST_NAME                     SALARY COMMISSION_PCT NVL(COMMISSION_PCT,0)
     ------------------------- ---------- -------------- ---------------------
     King                        24000.00                                    0
     Kochhar                     17000.00                                    0
     De Haan                     17000.00                                    0
     Hunold                       9000.00                                    0
     Ernst                        6000.00                                    0
     Lorentz                      4200.00                                    0
     Mourgos                      5800.00                                    0
     Rajs                         3500.00                                    0
     Davies                       3100.00                                    0
     Matos                        2600.00                                    0
     Vargas                       2500.00                                    0
     Zlotkey                     10500.00           0.20                   0.2
     ```

 2. NVL2函数 NVL2(expr1, expr2, expr3)

   - 如果expr1 不为NULL,则执行expr2， 反之则执行 expr3

     ``` plsql
     SQL> select last_name, department_id, commission_pct, nvl2(commission_pct, 'SAL+COMM', 'SAL') income
       2  from employees
       3  where department_id between 50 and 80
       4  ;
     
     LAST_NAME                 DEPARTMENT_ID COMMISSION_PCT INCOME
     ------------------------- ------------- -------------- --------
     Hunold                               60                SAL
     Ernst                                60                SAL
     Lorentz                              60                SAL
     Mourgos                              50                SAL
     Rajs                                 50                SAL
     Davies                               50                SAL
     Matos                                50                SAL
     Vargas                               50                SAL
     Zlotkey                              80           0.20 SAL+COMM
     Abel                                 80           0.30 SAL+COMM
     ```

 - NULLIF函数 NULLIF(expr1, expr2) 

   - 如果expr1 和 expr2 相等，则返回null，否则返回expr1.

     ``` plsql
     SQL> select first_name, LENGTH(first_name) "expr1", last_name, LENGTH(last_name) "expr2", NULLIF(length(first_name), length(last_name))
       2  from employees
       3  ;
     
     FIRST_NAME                expr1 LAST_NAME                      expr2 NULLIF(LENGTH(FIRST_NAME),LENGTH(LAST_NAME))
     -------------------- ---------- ------------------------- ---------- --------------------------------------------
     Steven                        6 King                               4                                            6
     Neena                         5 Kochhar                            7                                            5
     Lex                           3 De Haan                            7                                            3
     Alexander                     9 Hunold                             6                                            9
     Bruce                         5 Ernst                              5 
     Diana                         5 Lorentz                            7                                            5
     ```

 3. COALESCE 函数

   - 与NVL相比，可以处理交替的多个值。如果第一个表达式 为空，则返回下一个表达式。

     ``` plsql
     SQL> SELECT   last_name,
       2           COALESCE(commission_pct, salary, 10) comm
       3  FROM     employees
       4  ORDER BY commission_pct;
     
     LAST_NAME                       COMM
     ------------------------- ----------
     Grant                           0.15
     Taylor                           0.2
     ```

#### 条件表达式

  - 在sql 中使用if-then-else 表达式

    1. CASE 表达式

      - ``` plsql
        CASE expr1 WHEN comparison_expr1 THEN return_expr1
        	[WHEN comparison_expr2 THEN return_expr2
            WHEN comparison_expr3 THEN return_expr3
            ELSE else_expr]
        END
        ```

      - ``` PLSQL
        SQL> select last_name, department_id,
          2  case department_id
          3       when 10 then salary * 1.1
          4       when 20 then salary * 1.2
          5       when 30 then salary * 1.3
          6       else salary
          7  end  AS "NEWSALARY"
          8  from employees
          9  ;
        
        LAST_NAME                 DEPARTMENT_ID  NEWSALARY
        ------------------------- ------------- ----------
        King                                 90      24000
        Kochhar                              90      17000
        De Haan                              90      17000
        ```

    2. DECODE 函数

      - ``` plsql
        DECODE(col|expression, search1, result1 ,
              			   [, search2, result2,...,]
              			   [, default])
        
        ```

      - ``` plsql
        SQL> select last_name, job_id,
          2  decode(department_id, 10, salary *1.1,
          3                        20, salary *1.2,
          4                        30, salary *1.3,
          5                            salary)
          6  from employees;
        
        LAST_NAME                 JOB_ID     DECODE(DEPARTMENT_ID,10,SALARY*1.1,20,SALARY*1.2,30,SALARY*1.3,SALARY)
        ------------------------- ---------- ----------------------------------------------------------------------
        King                      AD_PRES                                                                     24000
        Kochhar                   AD_VP                                                                       17000
        De Haan                   AD_VP                                                                       17000
        Hunold                    IT_PROG                                                                      9000
        Ernst                     IT_PROG                                                                      6000
        ```

#### 嵌套函数

  - 单行函数是可以嵌套的
  - 嵌套函数的执行顺序是从内部往外部开始的。

### 多行函数

## 第五章	多表查询

从多个表中获取数据

<img src="assets\2020-07-07 145519.jpg" style="zoom:80%;" />

笛卡尔集错误

由于省略了连接条件，或是连接条件无效，导致表中的所有的行互相连接。 解决办法：**添加有效的连接条件**

### Oracle 连接

 - ``` plsql
   SELECT	table1.column, table2.column
   FROM	table1, table2
   WHERE	table1.column1 = table2.column2;
   ```

- 在where中写入连接条件。

 - 在表中有相同的列时，在列名之前加上**表名前缀**。

    - 表名前缀：用来区分表中相同的列，在不同的表中具有相同的列名的时候可以用**表的别名**来区分。
   - 表的别名：使用表的别名可以简化查询，提供查询的执行效率。

 - where 中多个条件连接的时候，要用and 操作符。

   	- 连接n 个表时，至少需要n-1 个连接条件。

- ```plsq
  -- 查出公司员工的last_name, department_name, city
  select e.last_name, d.department_name, l.city
  from employees e, departments d, locations l
  where e.department_id = d.department_id and d.location_id = l.location_id
  
  LAST_NAME                 DEPARTMENT_NAME                CITY
  ------------------------- ------------------------------ ------------------------------
  King                      Executive                      Seattle
  Kochhar                   Executive                      Seattle
  De Haan                   Executive                      Seattle
  Hunold                    IT                             Southlake
  Ernst                     IT                             Southlake
  Lorentz                   IT                             Southlake
  Mourgos                   Shipping                       South San Francisco
  Rajs                      Shipping                       South San Francisco
  Davies                    Shipping                       South San Francisco
  Matos                     Shipping                       South San Francisco
  Vargas                    Shipping                       South San Francisco
  Zlotkey                   Sales                          Oxford
  Abel                      Sales                          Oxford
  Taylor                    Sales                          Oxford
  Whalen                    Administration                 Seattle
  Hartstein                 Marketing                      Toronto
  Fay                       Marketing                      Toronto
  Higgins                   Accounting                     Seattle
  Gietz                     Accounting                     Seattle
  
  19 rows selected
  ```



### 非等值连接

 - ``` plsql
   SQL> select e.last_name, e.salary, j.grade_level
     2  from employees e, job_grades j
     3  where e.salary
     4        between j.lowest_sal and j.highest_sal;
   
   LAST_NAME                     SALARY GRADE_LEVEL
   ------------------------- ---------- -----------
   Vargas                       2500.00 A
   Matos                        2600.00 A
   Vargas                       2500.00 A
   ```



### 外连接

 - 内连接：合并具有同一列的两个以上的表的行，结果集中不包含一个表与另一个表不匹配的行。

 - 外连接：两个表在连接过程中除了返回满足连接条件的行以外**还返回左或右表中不满足条件的行**，这种连接称为左或右外连接。没有匹配的行时，结果表中相应的列为空（null），外连接的where字句条件类似于内连接，但是连接条件中没有匹配行的表的列后面要加上外连接符，即用**圆括号括起来的加号**。

 - 外连接语法 **加号在左边，右连接；加号在右边，左连接**。

    - 右外连接

      ``` plsql
      SELECT	table1.column, table2.column
      FROM	table1, table2
      WHERE	table1.column(+) = table2.column;
      ```

   - ``` plsql
     SQL> select e.last_name, e.department_id, d.department_name
       2  from employees e, departments d
       3  where e.department_id(+) = d.department_id;
     
     LAST_NAME                 DEPARTMENT_ID DEPARTMENT_NAME
     ------------------------- ------------- ------------------------------
     King                                 90 Executive
     Kochhar                              90 Executive
     De Haan                              90 Executive
     Hunold                               60 IT
     Ernst                                60 IT
     Lorentz                              60 IT
     Mourgos                              50 Shipping
     Rajs                                 50 Shipping
     Davies                               50 Shipping
     Matos                                50 Shipping
     Vargas                               50 Shipping
     Zlotkey                              80 Sales
     Abel                                 80 Sales
     Taylor                               80 Sales
     Whalen                               10 Administration
     Hartstein                            20 Marketing
     Fay                                  20 Marketing
     Higgins                             110 Accounting
     Gietz                               110 Accounting
                                             Contracting
     
     20 rows selected
     ```

   - 左外连接

     ``` plsql
     SELECT	table1.column, table2.column
     FROM	table1, table2
     WHERE	table1.column = table2.column(+);
     ```

   - ``` plsql
     SQL> select e.last_name, e.department_id, d.department_name
       2  from employees e, departments d
       3  where e.department_id = d.department_id(+);
     
     LAST_NAME                 DEPARTMENT_ID DEPARTMENT_NAME
     ------------------------- ------------- ------------------------------
     Whalen                               10 Administration
     Fay                                  20 Marketing
     Hartstein                            20 Marketing
     Vargas                               50 Shipping
     Matos                                50 Shipping
     Davies                               50 Shipping
     Rajs                                 50 Shipping
     Mourgos                              50 Shipping
     Lorentz                              60 IT
     Ernst                                60 IT
     Hunold                               60 IT
     Taylor                               80 Sales
     Abel                                 80 Sales
     Zlotkey                              80 Sales
     De Haan                              90 Executive
     Kochhar                              90 Executive
     King                                 90 Executive
     Gietz                               110 Accounting
     Higgins                             110 Accounting
     Grant                                   
     
     20 rows selected
     ```

### 自连接

​	<img src="F:\gitHub\Oracle\assets\2020-07-07 153721.jpg" style="zoom:80%;" />

``` plsq;
SQL> select worker.last_name || 'works for' || manager.last_name
  2  from employees worker, employees manager
  3  where worker.manager_id = manager.employee_id;

WORKER.LAST_NAME||'WORKSFOR'||MANAGER.LAST_NAME
-----------------------------------------------------------
Hartsteinworks forKing
Zlotkeyworks forKing
Mourgosworks forKing
De Haanworks forKing
Kochharworks forKing
Higginsworks forKochhar
```

### 1999语法连接

#### CROSS JOIN 叉集 

``` plsql
select last_name, department_name
from employees
cross join departments;
```

#### NATURAL JOIN 

 - 会以两个表中具有相同名字的列为条件创建等值连接。

 - 在表中查询满足等值条件的数据

 - 如果只是列名相同而数据类型不同，会产生错误。

 - ``` plsql
   SQL> select department_id, department_name
     2  from employees
     3  natural join departments;
   
   DEPARTMENT_ID DEPARTMENT_NAME
   ------------- ------------------------------
              90 Executive
              90 Executive
              60 IT
              60 IT
              50 Shipping
              50 Shipping
              50 Shipping
              50 Shipping
              80 Sales
              80 Sales
              20 Marketing
             110 Accounting
   
   12 rows selected
   
   ```

#### USING

 - 在使用natural join 字句创建等值连接时，可以使用**using**字句指定等值连接中需要用到的列。

 - 使用using 可以在有多个列满足条件时进行选择。

 - 不要给选中的列加上表名前缀或是别名

 - join 和 using 字句经常搭配使用。

 - ``` plsql
   SQL> select last_name, department_id
     2  from employees
     3  join departments using (department_id)
     4  ;
   
   LAST_NAME                 DEPARTMENT_ID
   ------------------------- -------------
   King                                 90
   Kochhar                              90
   De Haan                              90
   Hunold                               60
   ```

#### ON 常用

 - 自然连接中具有相同名字的列为连接条件

 - 可以使用on字句指定额外的连接条件

 - 这个连接条件与其他连接条件分开

 - ON 子句使语句具有更高的易读性。

 - ``` plsql
   SQL> select last_name, city, department_name
     2  from employees e
     3  join departments d
     4  on d.department_id = e.department_id        
     5  join locations l
     6  on l.location_id = d.location_id;
   
   LAST_NAME                 CITY                           DEPARTMENT_NAME
   ------------------------- ------------------------------ ------------------------------
   King                      Seattle                        Executive
   Kochhar                   Seattle                        Executive
   De Haan                   Seattle                        Executive
   Hunold                    Southlake                      IT
   Ernst                     Southlake                      IT
   Lorentz                   Southlake                      IT
   Mourgos                   South San Francisco            Shipping
   Rajs                      South San Francisco            Shipping
   Davies                    South San Francisco            Shipping
   Matos                     South San Francisco            Shipping
   ```

#### 内连接和外连接

 - 在sql：1999中，内连接只返回满足条件的数据。

 - 两个表在连接的过程中除了返回满足条件的行以外，还返回左或右不满足条件的行，成为**左或右外连接**。

    - 左外连接

      ``` plsql
      select e.last_name, e.department_id, d.department_name
      from employees e
      left outer join departments d
      on d.department_id = e.department_id
      
      SQL> /
      
      LAST_NAME                 DEPARTMENT_ID DEPARTMENT_NAME
      ------------------------- ------------- ------------------------------
      Whalen                               10 Administration
      Fay                                  20 Marketing
      Hartstein                            20 Marketing
      Vargas                               50 Shipping
      Matos                                50 Shipping
      Davies                               50 Shipping
      Rajs                                 50 Shipping
      Mourgos                              50 Shipping
      Lorentz                              60 IT
      Ernst                                60 IT
      Hunold                               60 IT
      Taylor                               80 Sales
      Abel                                 80 Sales
      Zlotkey                              80 Sales
      De Haan                              90 Executive
      Kochhar                              90 Executive
      King                                 90 Executive
      Gietz                               110 Accounting
      Higgins                             110 Accounting
      ```

   - 右外连接

     ``` plsql
     select e.last_name, e.department_id, d.department_name
     from employees e
     right outer join departments d
     on d.department_id = e.department_id
     
     SQL> /
     
     LAST_NAME                 DEPARTMENT_ID DEPARTMENT_NAME
     ------------------------- ------------- ------------------------------
     King                                 90 Executive
     Kochhar                              90 Executive
     De Haan                              90 Executive
     Hunold                               60 IT
     Ernst                                60 IT
     Lorentz                              60 IT
     Mourgos                              50 Shipping
     Rajs                                 50 Shipping
     Davies                               50 Shipping
     Matos                                50 Shipping
     Vargas                               50 Shipping
     Zlotkey                              80 Sales
     Abel                                 80 Sales
     Taylor                               80 Sales
     Whalen                               10 Administration
     Hartstein                            20 Marketing
     Fay                                  20 Marketing
     Higgins                             110 Accounting
     Gietz                               110 Accounting
                                             Contracting
     
     ```

     

 - 两个表在连接的过程中返回不满足条件的连接，成为**满外连接**。

    - 满外连接

      ``` sql
      select e.last_name, e.department_id, d.department_name
      from employees e
      full outer join departments d
      on d.department_id = e.department_id
      
      LAST_NAME                 DEPARTMENT_ID DEPARTMENT_NAME
      ------------------------- ------------- ------------------------------
      King                                 90 Executive
      Kochhar                              90 Executive
      De Haan                              90 Executive
      Hunold                               60 IT
      Ernst                                60 IT
      Lorentz                              60 IT
      Mourgos                              50 Shipping
      Rajs                                 50 Shipping
      Davies                               50 Shipping
      Matos                                50 Shipping
      Vargas                               50 Shipping
      Zlotkey                              80 Sales
      Abel                                 80 Sales
      Taylor                               80 Sales
      Grant                                   
      Whalen                               10 Administration
      Hartstein                            20 Marketing
      Fay                                  20 Marketing
      Higgins                             110 Accounting
      Gietz                               110 Accounting		
      
      LAST_NAME                 DEPARTMENT_ID DEPARTMENT_NAME
      ------------------------- ------------- ------------------------------
                                              Contracting
      ```

      

## 第六章	分组函数

分组函数作用于一个数据，并对一组数据返回一个值。**组函数忽略空值**

### 组函数类型

	- AVG：平均值，可以对数值型数据
	- COUNT：数量，可以对任意数据类型使用
	- MAX：最大值，可以对任意数据类型使用
	- MIN：最小值，可以对任意数据类型使用
	- SUM：求和，对数值型数据使用

### 组函数语法

   ``` sql
SELECT	[column,] group_function(column), ...
FROM		table
[WHERE	condition]
[GROUP BY	column]
[ORDER BY	column];
   ```

### 组函数实例

``` sql
SQL> select avg(salary), max(salary), min(salary), sum(salary)
  2  from employees
  3  where job_id like '%REP%';

AVG(SALARY) MAX(SALARY) MIN(SALARY) SUM(SALARY)
----------- ----------- ----------- -----------
       8150       11000        6000       32600
```

``` sql
SQL> select count(*)
  2  from employees
  3  where department_id = 50;

  COUNT(*)
----------
         5
```

在组函数中使用nvl函数，无法忽略空值。

``` sql
SQL> select avg(nvl(commission_pct, 0))          
  2  from employees;

AVG(NVL(COMMISSION_PCT,0))
--------------------------
                    0.0425

SQL> select avg(commission_pct)
  2  from employees;

AVG(COMMISSION_PCT)
-------------------
             0.2125
```

``` sql
SQL> select count(distinct(department_id))     #distinct 去重
  2  from employees;

COUNT(DISTINCT(DEPARTMENT_ID))
------------------------------
                             7

SQL> select count(department_id)
  2  from employees;

COUNT(DEPARTMENT_ID)
--------------------
                  19
```

### 分组数据

group by

可以使用 group by字句将表中的数据分成若干组。

``` sql
SELECT	column, group_function(column)
FROM		table
[WHERE	condition]
[GROUP BY	group_by_expression]
[ORDER BY	column];
# 明确：where一定放在from 后面
```

``` sql
SQL> select department_id, avg(salary)           #一共有8个组，在部门中使用组函数
  2  from employees
  3  group by department_id;

DEPARTMENT_ID AVG(SALARY)
------------- -----------
                     7000
           90 19333.33333
           20        9500
          110       10150
           50        3500
           80 10033.33333
           60        6400
           10        4400

8 rows selected


SQL> select distinct department_id
  2  from employees;

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

使用多个列进行分组

```sql
SQL> select department_id dept_id, job_id, sum(salary)
  2  from employees
  3  group by department_id, job_id
  4  ;

DEPT_ID JOB_ID     SUM(SALARY)
------- ---------- -----------
    110 AC_ACCOUNT        8300
     90 AD_VP            34000
     50 ST_CLERK         11700
     80 SA_REP           19600
     50 ST_MAN            5800
     80 SA_MAN           10500
    110 AC_MGR           12000
     90 AD_PRES          24000
     60 IT_PROG          19200
     20 MK_MAN           13000
        SA_REP            7000
     10 AD_ASST           4400
     20 MK_REP            6000
     
13 rows selected
```

<img src="F:\gitHub\Oracle\assets\2020-07-07 221150.jpg" style="zoom:80%;" />

非法使用组函数

1. 所有包含于select 列表中，而未包含于组函数的列都必须包含于group by 中

``` sql
select department_id, count(last_name)
from employees

ORA-00937: 単一グループのグループ関数ではありません。

SQL> select department_id, count(last_name)
  2  from employees
  3  group by department_id;

DEPARTMENT_ID COUNT(LAST_NAME)
------------- ----------------
                             1
           90                3
           20                2
          110                2
           50                5
           80                3
           60                3
           10                1
```

2. 不能够在where 中使用组函数，可以在having 子句中使用组函数。

``` sql
SQL> select department_id, avg(salary)
  2  from employees
  3  group by department_id
  4  having avg(salary) > 8000
  5  ;

DEPARTMENT_ID AVG(SALARY)
------------- -----------
           90 19333.33333
           20        9500
          110       10150
           80 10033.33333
```

过滤分组：HAVING字句

1. 行已经被分组

2. 使用了组函数

3. 满足having 字句中条件的分组将被显示。

4. ``` sql
   SELECT	column, group_function
   FROM		table
   [WHERE	condition]
   [GROUP BY	group_by_expression]
   [HAVING	group_condition]
   [ORDER BY	column];
   ```

嵌套组函数

​	显示各部门平均工资的最大值

``` sql
SQL> select max(avg(salary))
  2  from employees
  3  group by department_id;

MAX(AVG(SALARY))
----------------
19333.3333333333
```

----

## 第七章	子查询

在进行主查询之前，先进性的查询被称为子查询或内查询。

- 子查询要包含在括号中

- 将子查询放在比较条件的右侧

- 单行操作符对应单行子查询，多行操作符对应多行子查询。

  - 单行操作符
    - 只返回一行结果，使用单行比较操作符。
    - 等于(=)，大于(>)，大于等于(>=)，小于(<)，小于等于(<=)，不等于(<>)
  - 多行操作符
    - 返回多行结果，使用多行比较操作符。
    - in 等于列表中任意一个， any 和子查询返回的某一个值比较， all 和子查询返回的所有值比较。
  - 非法子查询  多行子查询不能够使用单行子查询比较符

- ``` sql
  --返回job_id 和141号员工相同，salary 比143好员工多的员工的姓名，job_id和工资
  select last_name, job_id, salary
  from employees
  where job_id = (select job_id
                 from employees
                 where employee_id = 141)
  and salary > ( select salary
                 from employees
                 where employee_id = 143)  
                 
  SQL> /
  
  LAST_NAME                 JOB_ID         SALARY
  ------------------------- ---------- ----------
  Rajs                      ST_CLERK      3500.00
  Davies                    ST_CLERK      3100.00
  
  ```

- ``` sql
  --返回公司工资最少的员工的last_name ,job_id, salary
  select last_name, job_id, salary
  from employees
  where salary = ( select min(salary)
                   from employees)         
  SQL> /
  
  LAST_NAME                 JOB_ID         SALARY
  ------------------------- ---------- ----------
  Vargas                    ST_CLERK      2500.00                 
  ```

- ``` sql
  -- 查询最低工资大于50号部门最低工资的部门id和其最低工资
  select department_id, min(salary)
  from employees
  group by department_id
  having min(salary) > ( select min(salary)
                         from employees
                         where department_id = 50)    
  SQL> /
  
  DEPARTMENT_ID MIN(SALARY)
  ------------- -----------
                       7000
             90       17000
             20        6000
            110        8300
             80        8600
             60        4200
             10        4400
  
  7 rows selected                       
  ```

- ```sql
  --返回其它部门中比job_id为‘IT_PROG’部门任一工资低的员工的员工号、姓名，job_id，salary
  select employee_id, last_name, job_id, salary
  from employees
  where salary < any ( select salary
                       from employees
                       where job_id = 'IT_PROG')
  and job_id <> 'IT_PROG'
  SQL> /
  
  EMPLOYEE_ID LAST_NAME                 JOB_ID         SALARY
  ----------- ------------------------- ---------- ----------
          144 Vargas                    ST_CLERK      2500.00
          143 Matos                     ST_CLERK      2600.00
          142 Davies                    ST_CLERK      3100.00
          141 Rajs                      ST_CLERK      3500.00
          200 Whalen                    AD_ASST       4400.00
          124 Mourgos                   ST_MAN        5800.00
          202 Fay                       MK_REP        6000.00
          178 Grant                     SA_REP        7000.00
          206 Gietz                     AC_ACCOUNT    8300.00
          176 Taylor                    SA_REP        8600.00
  
  10 rows selected
  ```

- ``` sql
  --返回其它部门中比job_id为‘IT_PROG’部门所有工资低的员工的员工号、姓名，job_id，salary
  select employee_id, last_name, job_id, salary
  from employees
  where salary < all ( select salary
                       from employees
                       where job_id = 'IT_PROG')
  and job_id <> 'IT_PROG'
  SQL> /
  
  EMPLOYEE_ID LAST_NAME                 JOB_ID         SALARY
  ----------- ------------------------- ---------- ----------
          141 Rajs                      ST_CLERK      3500.00
          142 Davies                    ST_CLERK      3100.00
          143 Matos                     ST_CLERK      2600.00
          144 Vargas                    ST_CLERK      2500.00
  ```

  ---
  
  

## 第八章	创建和管理表

### 常见的数据类型

| 对象   | 描述                             |
| ------ | -------------------------------- |
| 表     | 基本数据存储集合，由行和列组成。 |
| 视图   | 从表中抽出的逻辑相关的数据集合。 |
| 序列   | 提供有规律的数值。               |
| 索引   | 提供查询的效率。                 |
| 同义词 | 给对象其别名。                   |

### Oracle 数据库中的表

 	1. 用户自定义的表
     - 用户自己创建并维护的一组表
     - 包含了用户所需的信息
 	2. 数据字典
     - 由 Oracle Server 创建的一组表
     - 包含数据库信息

查询数据字典

 - 查看用户自定义的表

   ```sql
   SQL> select table_name 
     2  from user_tables;
   
   TABLE_NAME
   ------------------------------
   DEPT
   EMP
   BONUS
   SALGRADE
   JOB_GRADES
   REGIONS
   LOCATIONS
   DEPARTMENTS
   JOBS
   EMPLOYEES
   JOB_HISTORY
   COUNTRIES
   
   12 rows selected
   ```

- 查看用户定义的各种数据库对象

  ```sql
  SQL> select distinct object_type
    2  from user_objects;
  
  OBJECT_TYPE
  -------------------
  SEQUENCE
  INDEX
  TABLE
  ```

- 查看用户定义的表、视图、同义词和序列

  ```sql
  SQL> select * 
    2  from user_catalog;
  
  TABLE_NAME                     TABLE_TYPE
  ------------------------------ -----------
  BONUS                          TABLE
  COUNTRIES                      TABLE
  DEPARTMENTS                    TABLE
  DEPARTMENTS_SEQ                SEQUENCE
  DEPT                           TABLE
  EMP                            TABLE
  EMPLOYEES                      TABLE
  EMPLOYEES_SEQ                  SEQUENCE
  JOBS                           TABLE
  JOB_GRADES                     TABLE
  JOB_HISTORY                    TABLE
  LOCATIONS                      TABLE
  LOCATIONS_SEQ                  SEQUENCE
  REGIONS                        TABLE
  SALGRADE                       TABLE
  ```

命名规则

	- 必须以字母开头。
	- 必须在1- 30 个字符之间。
	- 必须只能包含 A-Z,a-z，0-9, _, $, 和#
	- 必须不能够和用户自定义的其它对象重名
	- 必须不能是Oracle 保留字

### CREATE TABLE 语句

	- 必须由创建表的权限
	- 存储空间
	- 必须指定表名，列名，数据类型，尺寸

创建表

- 语法

  ``` sql
  CREATE TABLE dept	(deptno 	NUMBER(2),
  		dname 	VARCHAR2(14),
  		loc 	VARCHAR2(13));
  
  ```

- 实例

  ``` sql
  SQL> create table car
    2  (carno number(2),
    3   carname varchar2(255),
    4  caraddress varchar2(255));
  
  Table created
  ```

### 数据类型

| 数据类型             | 描述                                   |
| -------------------- | -------------------------------------- |
| **varchar2（size）** | 可变长字符数据                         |
| **char（size）**     | 定长字符数据                           |
| **number（p，s）**   | 可变长数值数据                         |
| **date**             | 日期型数据                             |
| long                 | 可变长字符数据，最大可达2G             |
| colb                 | 字符数据，最大可达4G                   |
| raw                  | 原始的二进制数据                       |
| **blob**             | 二进制数据，最大可达4G                 |
| BFILE                | 存储二进制文件的二进制数据，最大可达4G |
| rowid                | 行地址                                 |

### 使用子查询创建表

 - 使用 as subquery 选项，将创建表和插入数据结合起来

 - 指定的列和子查询的列要一一对应。

 - 通过列名和默认值来定义列。

 - 语法

   ``` sql
   CREATE TABLE table
     	  [(column, column...)]
   AS subquery;                      #subquery 子语句
   ```

- 实例

  ```
  SQL> create table dept80
    2  as
    3         select employee_id, last_name, salary*12 annsal, hire_date
    4         from employees
    5         where department_id = 80;
  
  Table created
  
  
  SQL> desc dept80
  Name        Type         Nullable Default Comments 
  ----------- ------------ -------- ------- -------- 
  EMPLOYEE_ID NUMBER(6)    Y                         
  LAST_NAME   VARCHAR2(25)                           
  ANNSAL      NUMBER       Y                         
  HIRE_DATE   DATE           
  ```

### ALTER TABLE语句

 - 追加新的列

   - ```sql
     ALTER TABLE table
     ADD		   (column datatype [DEFAULT expr]
     		   [, column datatype]...);
     ```

   - ```sql
     SQL> alter table dept80
       2  add ( department_name varchar2(255));
     
     Table altered
     
     
     SQL> desc dept80;
     Name            Type          Nullable Default Comments 
     --------------- ------------- -------- ------- -------- 
     EMPLOYEE_ID     NUMBER(6)     Y                         
     LAST_NAME       VARCHAR2(25)                            
     ANNSAL          NUMBER        Y                         
     HIRE_DATE       DATE                                    
     DEPARTMENT_NAME VARCHAR2(255) Y      							#新追加的列
     ```

 - 修改现有的列

    - ```sql
      ALTER TABLE table
      MODIFY	   (column datatype [DEFAULT expr]
      		   [, column datatype]...);
      ```

    - ``` sql
      SQL> alter table dept80
        2  modify (employee_id number(20));                          #将大小改为20
      
      Table altered
      
      
      SQL> desc dept80;
      Name            Type          Nullable Default Comments 
      --------------- ------------- -------- ------- -------- 
      EMPLOYEE_ID     NUMBER(20)    Y                                #修改成功
      LAST_NAME       VARCHAR2(25)                            
      ANNSAL          NUMBER        Y                         
      HIRE_DATE       DATE                                    
      DEPARTMENT_NAME VARCHAR2(255) Y  
      ```

 - 为新追加的列定义默认值

 - 删除一个列

    - ```sql
      ALTER TABLE table
      DROP COLUMN  column_name;
      ```

    - ```sql
      SQL> alter table dept80
        2  drop column department_name;
      
      Table altered
      
      
      SQL> desc dept80
      Name        Type         Nullable Default Comments 
      ----------- ------------ -------- ------- -------- 
      EMPLOYEE_ID NUMBER(20)   Y                         
      LAST_NAME   VARCHAR2(25)                           
      ANNSAL      NUMBER       Y                         
      HIRE_DATE   DATE       
      ```

 - 重命名表的一个列

   - ``` sql
     ALTER TABLE table_name RENAME COLUMM old_column_name 
     TO new_column_name
     ```

   - ``` sql
     SQL> alter table dept80                                    #操作alter语句的时候都需要确定操作那个表格，
       2  rename column employee_id to id;
     
     Table altered
     
     
     SQL> desc table dept80;
     Object table dept80 does not exist.
     
     SQL> desc dept80
     Name      Type         Nullable Default Comments 
     --------- ------------ -------- ------- -------- 
     ID        NUMBER(20)   Y                         
     LAST_NAME VARCHAR2(25)                           
     ANNSAL    NUMBER       Y                         
     HIRE_DATE DATE  
     ```

### 删除表

 - 数据和数据结构都被删除

 - 所有正在运行的相关事务都会被提交

 - 所有相关索引都会被删除

 - DROP TABLE 语句不能够被回滚

 - ```sql
   SQL> drop table dept80;
   
   Table dropped
   ```

### 清空表

 - 删除表中所有数据

 - 释放表的存储空间

 - truncate 语句不能够回滚

 - 可以使用delete 语句删除数据，可以回滚

 - ```sql
   SQL> select * from dept80;
   
   EMPLOYEE_ID LAST_NAME                 JOB_ID
   ----------- ------------------------- ----------
           149 Zlotkey                   SA_MAN
           174 Abel                      SA_REP
           176 Taylor                    SA_REP
   
   SQL> truncate table dept80;
   
   Table truncated
   
   
   SQL> select * from dept80;
   
   EMPLOYEE_ID LAST_NAME                 JOB_ID
   ----------- ------------------------- ---------
   ```

---

## 第九章	数据处理

数据操纵语言

DML（Data Manipulation Language）可以实现向表中插入数据，修改数据，删除数据，完成事务（由若干项工作的DML语句组成的）

### 插入数据

 - insert 语法

 - ``` sql
   INSERT INTO	table [(column [, column...])]      #使用该语法一次只能向表中插入一条数据。
   VALUES		(value [, value...]);
   ```

   - 为每一列添加一个新值
   - 按列的默认顺序列出各个列的值
   - 在INSERT 字句中随意列出列名和他们的值
   - 字符和日期数据应该包含在单引号中。

- 向表中插入空值

  - 隐式方式

     ``` sql
      SQL> insert into departments (department_id, department_name)
        2  values (30, 'Purchasing');
      
      1 row inserted
     ```

  - 显示方式

    ``` sql
    SQL> insert into departments
      2  values (100, 'zhangsan', NULL, null);
    
    1 row inserted
    ```


### 更新数据

update 语法

``` sql
UPDATE		table
SET		column = value [, column = value, ...]
[WHERE 		condition];
```

- 使用where 字句指定要更新的数据
- 如果省略where 字句，则表示所有数据都将被更新

### 删除数据

delete 语法

``` sql
delete from table
[where condition];
```

### 数据库事务管理

commit

rollback

## 第十章	约束 CONSTRAINT

约束时表级的强制规定,如果不指定约束名，Oracle server 自动安装sys_cn的格式指定约束名。

约束在创建和创建之后都可以设置，有列级和表级两种。可以通过数据字典视图来查看约束。

有以下五种约束：

1. NOT NULL 只可以作用在列上
2. UNIQUE
3. PRIMARY KEY
4. FOREIGN KEY
5. CHECK

语法

 1. 创建表是定义

    ``` sql
    CREATE TABLE [schema.]table
    	    (column datatype [DEFAULT expr]
    		[column_constraint],
    		...
    		[table_constraint][,...]);
    		
    CREATE TABLE employees(
      	     employee_id  NUMBER(6),
        	     first_name   VARCHAR2(20),
      	     ...
      	     job_id       VARCHAR2(10) NOT NULL,
    	     CONSTRAINT emp_emp_id_pk 
    		           	PRIMARY KEY (EMPLOYEE_ID));
    
    ```

	2. 列级

    ```sql
    column [CONSTRAINT constraint_name] constraint_type,
    ```

	3. 表级

    ```sql
    column,...
      [CONSTRAINT constraint_name] constraint_type
      (column, ...),
    ```

### 非空约束

用not null约束的字段不能为null值，**必须给定具体的数据**

``` sql
SQL> create table emp3(
  2   id number(3) not null,                      #系统命名
  3   last_name varchar2(255),                       
  4   hire_date date
  5             constraint emp3_constrain_nn      #用户命名
  6             not null);

Table created


SQL> desc emp3
Name      Type          Nullable Default Comments 
--------- ------------- -------- ------- -------- 
ID        NUMBER(3)                               
LAST_NAME VARCHAR2(255) Y                         
HIRE_DATE DATE   
```

### 唯一约束

unique约束的字段，具有唯一性，不可重复，但可以为null

``` sql
alter table emp3 
add ( email varchar2(255) unique,
      address varchar2(255)
      constraint emp3_constraint_un
      unique)

Name      Type          Nullable Default Comments 
--------- ------------- -------- ------- -------- 
ID        NUMBER(3)                               
LAST_NAME VARCHAR2(255) Y                         
HIRE_DATE DATE                                    
EMAIL     VARCHAR2(255) Y                         
ADDRESS   VARCHAR2(255) Y  
```

### 主键约束

表在设计的时候一定要有主键

 1. 主键涉及术语

    - 主键约束
    - 主键字段
    - 主键值

 2. 以上三种术语关系

    表中的某个字段添加主键约束之后，该字段为主键字段，主键字段中出现的每一个值称为主键值。

	3. 主键值和“not null unique” 的区别

    给某个字段添加了主键约束之后，该字段不能重复也不能够为空，效果和“NOT NULL UNIQUE”相同，但是其本质是不同的。

    主键约束除了可以做到“not null unique”之外呢，还会默认添加到索引（index）中。

	4. 一张表应该有主键字段，如果没有该表无效

    - 主键值：是当前行数据的唯一标识
    - 即使表中两条相同的数据，但是因为主键值的不同，也被视为两条数据。

	5. 按照主键约束字段的多少可以分类

    无论是单一主键还是复合主键，一张表的主键约束只能够有一个，但是可以有好几个约束字段

    - 单一主键：一个字段添加主键约束
    - 复合主键：多个字段添加主键约束

	6. 实例

    ``` sql
    SQL> create table emp3 （          #列级定义
      2  id number(5) primary key,
      3  last_name varchar(255));
    
    Table created
    ```

    ```sql
    create table emp4 (				  #表集定义，复合主键
    id number(7),
    name varchar(255),
    email varchar2(255),
    constraint emp4_constraint_pk primary key(id, name))
    ```

    

### 外键约束

只能是表级定义。什么是外键？若有两个表A、B，id是A的主键，而B中也有id字段，则id就是表B的外键，外键约束主要用来维护两个表之间数据的一致性。

A为基本表，B为信息表

1. 外键设计术语
   - 外键约束
   - 外键字段
   - 外键值
2. 某个字段添加外键约束之后，该字段被成为外键字段，其字段内的值被称为外键值。
3. 分类可以分为单一外键，和复合外键。
4. 一张表中可以有多个外键，这个和主键是不同的。

检查约束

定义每一行必须满足的条件。之在oracle中存在。

### 添加约束

```sql
SQL> alter table emp4
  2  add (address varchar2(255) not null);
```

### 删除约束

``` sql
SQL> alter table emp4
  2  drop constraint SYS_C0011148;       #系统默认名字
```

### 无效化约束

``` sql
alter table emp4
disable constraint emp4_constraint_nn
```

### 有效话约束

``` sql
alter table emp4
enable constraint emp4_constraint_nn

```

### 查询约束

``` sql
SQL> select constraint_name, constraint_type, search_condition
  2  from user_constraints
  3  where table_name = 'emp4';

CONSTRAINT_NAME                CONSTRAINT_TYPE SEARCH_CONDITION
------------------------------ --------------- --------------------
```

---



## 第十一章	视图

什么是视图

- 视图是一种虚表
- 视图建立在已有表的基础上，视图赖以建立的这些表称为基表。
- 向视图提供数据内容的语句称为select 语句，可以将视图视为存储起来的select 语句。
- 视图向用户提供基表数据的另外一种表现方式。

为什么使用视图

- 控制数据访问
- 简化查询
- 避免重复访问相同的数据

简单视图和复杂视图的对比

| 特性     | 简单视图 | 复杂视图   |
| -------- | -------- | ---------- |
| 表的数量 | 一个     | 一个或多个 |
| 函数     | 没有     | 有         |
| 分组     | 没有     | 有         |
| DML      | 可以     | 有时可以   |

如何创建视图

- 在 CREATE VIEW 语句中嵌入子查询

  ``` sql
  CREATE [OR REPLACE] [FORCE|NOFORCE] VIEW view
    [(alias[, alias]...)]
   AS subquery
  [WITH CHECK OPTION [CONSTRAINT constraint]]
  [WITH READ ONLY [CONSTRAINT constraint]];
  ```

- 举例

  ``` sql
  SQL> create view empvu80
    2  as select employee_id, last_name, salary
    3     from employees
    4     where department_id = 80;
  create view empvu80
  as select employee_id, last_name, salary
     from employees
     where department_id = 80
  
  ORA-01031: 権限が不足しています。                    #使用sqlplus登录system用户，使用命令grant create view to scott;
  
  SQL> ed
  SQL> /
  
  View created
  
  
  SQL> desc empvu80
  Name        Type         Nullable Default Comments 
  ----------- ------------ -------- ------- -------- 
  EMPLOYEE_ID NUMBER(6)                              
  LAST_NAME   VARCHAR2(25)                           
  SALARY      NUMBER(8,2)  Y    
  ```

  ``` sql
  SQL> desc empvu80
  Name        Type         Nullable Default Comments 
  ----------- ------------ -------- ------- -------- 
  EMPLOYEE_ID NUMBER(6)                              
  LAST_NAME   VARCHAR2(25)                           
  SALARY      NUMBER(8,2)  Y                         
  
  SQL> create view salvu50
    2  as select employee_id ID_NUMBER, last_name NAME,
    3            salary * 12 ANN_SALARY
    4     from employees
    5     where department_id = 50;
  
  View created
  
  
  SQL> select * from salvu50;
  
  ID_NUMBER NAME                      ANN_SALARY
  --------- ------------------------- ----------
        124 Mourgos                        69600
        141 Rajs                           42000
        142 Davies                         37200
        143 Matos                          31200
        144 Vargas                         30000
  ```

修改视图

使用create or replace view 字句修改视图，create view 字句中的各列的别名应和子查询中各列相对应。

``` sql
CREATE OR REPLACE VIEW empvu80
  (id_number, name, sal, department_id)
AS SELECT  employee_id, first_name || ' ' || last_name, 
           salary, department_id
   FROM    employees
   WHERE   department_id = 80;
```



## 第十二章	其他数据对象



