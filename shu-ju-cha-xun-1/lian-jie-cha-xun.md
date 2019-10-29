# 数据更新

## 插入

### 1. 插入元组

#### 语句格式

  INSERT

  INTO &lt;表名&gt; \[\(&lt;属性列1&gt;\[，&lt;属性列2 &gt;…\)\]

  VALUES \(&lt;常量1&gt; \[，&lt;常量2&gt;\]    …           \)

#### 功能

将新元组插入指定表中

#### INTO子句      

* 属性列的顺序可与表定义中的顺序不一致
* 没有指定属性列
* 指定部分属性列

#### VALUES子句

* 提供的值必须与INTO子句匹配
  * 值的个数 
  * 值的类型

### 2. 插入子查询结果

可以一次插入多个元组

#### 语句格式

```text
INSERT    
INTO <表名>  [(<属性列1> [，<属性列2>…  )]    
子查询;
```

#### 功能

    将子查询结果插入指定表中

#### INTO子句\(与插入元组类似\)

#### 子查询 

SELECT子句目标列必须与INTO子句匹配 

* 值的个数 
* 值的类型

```text
INSERT
INTO  Dept_age(Sdept，Avg_age)
              SELECT  Sdept，AVG(Sage)
              FROM  Student
              GROUP BY Sdept；

```

#### 完整性检测

RDBMS在执行插入语句时会检查所插元组是否破坏表上已定义的完整性规则

* 实体完整性
* 参照完整性
* 用户定义的完整性
  * NOT NULL约束
  * UNIQUE约束 
  * 值域约束

### 3. 使用 Select … Into 语句

可以用Select … Into来创建一个新表，并将结果行从查询插入新表中。**使用该语句，必须在目的数据库内具有 CREATE TABLE 权限。**通过在 WHERE 子句中包含 FALSE 条件，可以使用 SELECT...INTO 创建没有数据的相同表定义，即定义新的表结构。 

通过**创建新表**，并把**查询结果添加到新表**，可以分解对一个表的复杂查询要求，简化SQL语句，提高SQL语句的可读性。

```text
--创建一个新的表，对每一个系，求学生的平均年龄，并把结果存入数据库，
--新表名为Dept_sage，SQL语句如下：
SELECT Sdept，AVG(Sage)
 INTO Dept_sage
     from Student
     GROUP BY Sdept；

```

## 更新

### 语句格式

   UPDATE  &lt;表名&gt;

    SET  &lt;列名&gt;=&lt;表达式&gt;\[，&lt;列名&gt;=&lt;表达式&gt;\]…

    \[WHERE &lt;条件&gt;\]；

功能——修改指定表中满足WHERE子句条件的元组

### SET子句    

* 指定修改方式
* 要修改的列 
* 修改后取值

### WHERE子句

* 指定要修改的元组 
* 缺省表示要修改表中的所有元组

修改方式

1. 修改某一个元组的值

```text
--将学生200215121的年龄改为22岁
         UPDATE  Student
         SET Sage=22
         WHERE  Sno=' 200215121 '；
```

2. 修改多个元组的值

```text
--将所有学生的年龄增加1岁
         UPDATE Student
         SET Sage= Sage+1；
```

3. 带子查询的修改语句

```text
--将计算机科学系全体学生的成绩置零。        
UPDATE SC
        SET  Grade=0
        WHERE  sno in 
           (select sno from student where sdept='CS’) ；

```

### 检查完整性

RDBMS在执行修改语句时会检查修改操作

* 是否破坏表上已定义的完整性规则
* 实体完整性
* 主码不允许修改
* 用户定义的完整性
  * NOT NULL约束
  * UNIQUE约束 
  * 值域约束

## 删除

### 语句格式

       DELETE

       FROM     &lt;表名&gt;

       \[WHERE &lt;条件&gt;\]；

**功能——删除指定表中满足WHERE子句条件的元组**

### WHERE子句

* 指定要删除的元组
* 缺省表示要删除表中的全部元组，表的定义仍在字典中

### 1. 删除某一个元组的值

```text
DELETE
         FROM Student
         WHERE Sno= 200215128 '；
```

### 2. 删除多个元组的值 

```text
DELETE
        FROM SC；
```

### 3. 带子查询的删除语句

```text
DELETE 
FROM  SC
WHERE  sno in 
         (select sno
                  from student 
                       where  sdept='CS’) ；

```

