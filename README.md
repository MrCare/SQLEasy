# SQLEasy
* This is a note of  Mr.Car to learn SQL as soon as possible  

　　做数据分析需要用到SQL这个有力工具，但是我苦于从没有接触过，而且想尽量快地上手此工具，在B站SQL金老师的视频中发现了`sql.com`这款在线练习神器，于是我就想写一份学习笔记，记录一下SQL工具的常见命令和心得。 
  
* 第一个心得，`markdwn`实现段首缩进的方法：`Shift` + `Space`切换成全角输入模式，然后输入两个空格

> I will try my best to update it !

## Part 1 : Easy Query 简单查询

* 基本语句
    - `SELECT` * 
    - `FROM` table 
    - `WHERE` condition 1
        - `AND`/`OR` condition 2
    
* 心得：
    1. select * 的意思是选择所有columns,如果要选择有限的column，需要用“，”分隔  
    2. Where conditions 可以用“AND”或“OR”组合连接

### 针对数值的操作：

| 操作 | 说明 | 举例 |
| :-: | :-: | :-: |
| =,!=,<,<=,>,>= | 数值比较 | col **!=** 5 |
| BETWEEN...AND... | 与 $ \geq x \leq$ 类似| col **BETWEEN** 5 **AND** 10|
| NOT BETWEEN...AND... | 上一句的补集 | col **NOT BETWEEN** 5 **AND** 10 |
| IN(...) | 在括号中的数 | col **IN**(5,10)|
| NOT IN(...) | 不在括号中的数 | col **NOT IN**(5,10) |

* **(语句关键词大写是良好习惯，为了与表中的数据区分，但是不大写也行)**

### 针对字符串的操作：

| 操作 | 说明 | 举例 |
| :-: | :-: | :-: |
| = | 严格相等 | col **=** "Car" |
| != or <> | 不相等 | col **!=** "Car" |
| LIKE | 与 = 作用相同 | col **LIKE** "Car" |
| NOT LIKE | 与 != 作用相同 | col **NOT LIKE** "Car"|
| % | 匹配任意字符串，只能与 LIKE 与NOT LIKE 连用 | col **LIKE** "%A%"(返回 ARE,CA,CAR,CARE)|
| _ | 匹配任意一个字符，只能与 LIKE 与 NOT LIKE 连用 | col **LIKE** "CA_"(返回 CAR 而没有 CA ) |
| IN(...) | 在括号中的文本 | col **IN**("C","A","R")|
| NOT IN(...) | 不在括号中的文本 | col **NOT IN**("C","A","R") |

## Part 2 : Filtering And Sorting Query 过滤与排序

* 基本语句
    - `SELECT` **DISTINCT** col, col... 
    - `FROM` table 
    - `WHERE` condition 1
        - `AND`/`OR` condition 2
    - `GROUP BY` 1Key, 2Key
    - `ORDER BY` 1Key, 2Key `ASC`/`DESC`
        - `LIMIT` num_limit
        - `OFFSET` num_offset
* 心得：
    1. SELECT DISTINCE col,... 查询的结果是col中不重复的items
    2. GROUP BY 与 ORDER BY 必须在 WHERE 之后
    3. GROUP BY 的作用是按照关键字分组显示结果，关键字有主次之分
    4. ORDER BY 的子命令
        * ASC 默认升序ascend
        * DESC 降序 descend
        * LIMIT 5 限制结果显示项目的条数为5
        * OFFSET 10 限制结果从第11条开始显示

## Part 3：Multi-Table Query 多表连接

* 基本语句
    - `SELECT` * 
    - `FROM` table1
        - `LEFT`/`INNER`/`RIGHT`/`FULL JOIN` table2
        - `ON` table1.id = table2.id
    - `WHERE` condition 1
        - `AND`/`OR` condition 2
    - `GROUP BY` 1Key, 2Key
    - `ORDER BY` 1Key, 2Key `ASC`/`DESC`
        - `LIMIT` num_limit
        - `OFFSET` num_offset
* 心得：
    1. FROM 的子语句 JOIN 省略前面的语句则默认是 INNER 
    2. LEFT 会包含 table1 中的所有内容和 table2 中 table2.id 中与 table1.id 重合的部分
    3. INNER 会包含 table1 与 table2 中 id 重合的部分
    4. RIGHT 会包含 table2 中的所有内容和 table1 中 table1.id 中与 table2.id 重合的部分
    5. FULL 会保留 table1 与 table2 的所有信息
    6. LEFT， RIGHT， Full 的结果会含有 NULL值，而 INNER 的结果不会含 NULL 值

### 对 NULL 的处理
在`WHERE`的 condition 中队 NULL 进行处理

| 操作 | 说明 | 举例 |
| :-: | :-: | :-: |
| IS NULL | 返回 col 为 NULL 的结果 | **WHERE** col **IS NULL** |
| IS NOT NULL | 返回 col 不为 NULL 的结果| **WHERE** col **IS NOT NULL**|

## Part 4：Query with Expressions 表达式查询

* 基本语句
    - `SELECT` **expressions1** `AS` NewCol
    - `FROM` table1
        - `LEFT`/`INNER`/`RIGHT`/`FULL JOIN` table2
        - `ON` table1.id = table2.id
    - `WHERE` **expressions2**
        - `AND`/`OR` **expressions3**
    - `GROUP BY` 1Key, 2Key
    - `ORDER BY` 1Key, 2Key `ASC`/`DESC`
        - `LIMIT` num_limit
        - `OFFSET` num_offset
* 心得：
    1. **expressions** 可以在 SELECT 与 WHERE 中出现
    2. SELECT 后接能够直接计算的表达式，如 col * 5
    3. SELECT **expressions** AS NewCol 可以把 **expressions** 生成 NewCol
    4. WHERE 接条件表达式，如 col % 2 = 0 (col 是偶数)

## Part 5：Query with Aggregates 统计查询

* 基本语句
    - `SELECT` **AGG_FUNC(col_or_expression)** `AS` NewCol
    - `FROM` table1
        - `LEFT`/`INNER`/`RIGHT`/`FULL JOIN` table2
        - `ON` table1.id = table2.id
    - `WHERE` condition1
        - `AND`/`OR` condition2
    - `GROUP BY` 1Key, 2Key
        - `HAVING` **group-condition**
    - `ORDER BY` 1Key, 2Key `ASC`/`DESC`
        - `LIMIT` num_limit
        - `OFFSET` num_offset

* 心得：
    1. 若没有 GROUP BY 语句时，每一个 AGG_FUNC() 会在整个表的集合里运行
    2. 当 GROUP BY 之后，AGG_FUNC() 会在 group 运行
    3. GROUP BY 的子语句 HAVING 与 WHERE 用法相同，只不过是用于限制 group 的条件
    4. 如果不用 GROUP BY 语句，简单的 WHERE 就可以满足需求了

### 常见统计函数

| 函数 | 说明 |
| :-: | :-: |
| **COUNT( * )**,<br>**COUNT(col)** | 返回组内 col 非 NULL 行的个数，如果没有col则返回组内 item 的个数 |
| **MIN(col)** | 返回最小值 |
| **MAX(col)** | 返回最大值 |
| **AVG(col)** | 返回平均值 |
| **SUM(col)** | 返回所有数值的加和 |

## Part 6：Order of Execution 执行顺序

* 完整语句
    - `SELECT` `DISTINCET` col, AGG_FUNC(col_or_expression), ...
    - `FROM` table1
        - `LEFT`/`INNER`/`RIGHT`/`FULL JOIN` table2
        - `ON` table1.id = table2.id
    - `WHERE` condition1
        - `AND`/`OR` condition2
    - `GROUP BY` 1Key, 2Key
        - `HAVING` group-condition
    - `ORDER BY` 1Key, 2Key `ASC`/`DESC`
        - `LIMIT` num_limit
        - `OFFSET` num_offset

### 执行顺序

| 语句 | 次序 |
| :-: | :-: |
| **FROM** and **JOIN** | 0 |
| **WHERE** | 1 |
| **GROUP BY** | 2 |
| **HAVING** | 3 |
| **SELECT** | 4 |
| **DISTINCE** | 5 |
| **ORDER BY** | 6 |
| **LIMIT** or **OFFSET** |7|

## Part 7：Insert Rows 向表中插入新  Item

* 完整语句
    - `INSERT INTO` table (col1, col2, ...) **VALUES** (value1, value2, ...)

* 心得：
    1. INSERT INTO 是一条语句，后面的以空格分隔
    2. 要注意 VALUES 的格式，数值 or 字符串

## Part 8：Update Rows 更新 Item

* 完整语句
    - `UPDATE` table
    - `SET` col1 = value1, col2 = value2, ...
    - `WHERE` condition

* 心得：
    1. 使用时需要先用 SELECT 检验一下 WHERE 条件是否正确

## Part 9：Delete Rows 删除 Item

* 完整语句
    - `DELETE FROM` table
    - `WHERE` condition

* 心得：
    1. 使用时需要先用 SELECT 检验一下 WHERE 条件是否正确
    
## Part 10：Creatng Tables 创建新表

* 完整语句
    - `CREATE TABLE` `IF NOT EXISTS` table (
        - col **DataType TableConstraint** **DEFAULT** default_value, 
        - another_col **DataType** TableConstraint **DEFAULT** default_value, ...)

* 心得：
    1. IF NOT EXISTS 语句的作用是将表名重复的 ERROR 改成 WARNINGS ,但是如果重名还是不能创建表格
    2. 将 SELECT 的结果创建成新表
        * `SELECT INTO` NewTable
        * `FROM` OldTable
        
### DataType

| DataType | 解释 |
| :-: | :-: |
| **INTEGER, BOOLEAN** | 整型，布尔型（以0，1表示） |
| **FLOAT, DOUBLE, REAL** | 浮点型，双精度， |
| **CHARACTER(num_chars), VARCHAR(num_chars), TEXT** | 字符型 |
| **DATE, DATETIME** | 日期型 |
| **BLOB** | 二进制 |

### Table Constraints

| Constraint | 解释 |
| :-: | :-: |
| **PRIMARY KEY** | col 的值不可重复不可为 NULL |
| **AUTOINCREMENT** | 对于整数型的数值自动升序排列 |
| **UNIQUE** | col 的值不可重复 |
| **NOT NULL** | col 的值不可为 NULL |
| **CHEAK(expression)** | 通过条件检查 col |
| **FOREIGN KEY** | 与其他表中的 col 关联 |

### eg：
```SQL
CREATE TABLE movies (
    id INTEGER PRIMARY KEY,
    title TEXT,
    director TEXT,
    year INTEGER, 
    length_minutes INTEGER
);
```
