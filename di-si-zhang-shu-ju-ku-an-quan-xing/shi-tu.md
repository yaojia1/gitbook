# 视图

### 视图的特点 

* 虚表，是从一个或几个基本表（或视图）导出的表 
* 只存放视图的定义，不存放视图对应的数据 
* 基表中的数据发生变化，从视图中查询出的数据也随之改变

### 基于视图的操作 

* 查询 
* 删除 
* 受限更新 
* 定义基于该视图的新视图

## 3.7.1  定义视图

### 建立视图

语句格式

```text
CREATE  VIEW             
   <视图名>  [(<列名>  [，<列名>]…)]       
AS  <子查询>       
[WITH  CHECK  OPTION]；
```

 组成视图的属性列名：**全部省略或全部指定**

**子查询不允许含有ORDER BY子句和DISTINCT短语**

RDBMS执行CREATE VIEW语句时只是把视图定义存入数据字典，并不执行其中的SELECT语句。 在对视图查询时，按视图的定义从基本表中将数据查出。

```text
建立信息系学生的视图。

        CREATE VIEW IS_Student
        AS 
        SELECT Sno，Sname，Sage
        FROM    Student
        WHERE  Sdept= 'IS'；
建立信息系学生的视图，并要求进行修改和插入操作时仍需保证该视图只有信息系的学生 。
        CREATE VIEW IS_Student
        AS 
        SELECT Sno，Sname，Sage
        FROM  Student
        WHERE  Sdept= 'IS'
        WITH CHECK OPTION；
```

#### check option

对IS\_Student视图的更新操作：

* 修改操作：自动加上Sdept= 'IS'的条件 
* 删除操作：自动加上Sdept= 'IS'的条件 
* 插入操作：自动检查Sdept属性值是否为'IS' 
  * 如果不是，则拒绝该插入操作 
  * 如果没有提供Sdept属性值，则自动定义Sdept为'IS'

#### 基于多个基表的视图

```text
建立信息系选修了1号课程的学生视图。
        CREATE VIEW IS_S1(Sno，Sname，Grade)
        AS 
        SELECT Student.Sno，Sname，Grade
        FROM  Student，SC
        WHERE  Sdept= 'IS' AND
                       Student.Sno=SC.Sno AND
                       SC.Cno= '1'；
```

#### 基于视图的视图

```text
建立信息系选修了1号课程且成绩在90分以上的学生的视图。
        CREATE VIEW IS_S2
        AS
        SELECT Sno，Sname，Grade
        FROM  IS_S1
        WHERE  Grade>=90；
```

#### 带表达式的视图

```text
定义一个反映学生出生年份的视图。
        CREATE  VIEW BT_S(Sno，Sname，Sbirth)
        AS 
        SELECT Sno，Sname，2000-Sage
        FROM  Student；
```

#### 分组视图

```text
将学生的学号及他的平均成绩定义为一个视图
	       假设SC表中“成绩”列Grade为数字型
            CREAT  VIEW S_G(Sno，Gavg)
             AS  
             SELECT Sno，AVG(Grade)
             FROM  SC
             GROUP BY Sno；
```

#### 不指定属性列

缺点：

     修改基表Student的结构后，Student表与F\_Student视图的映象关系被破坏，导致该视图不能正确工作。

```text
将Student表中所有女生记录定义为一个视图
      CREATE VIEW F_Student(F_Sno，name，sex，age，dept)
      AS
      SELECT *
      FROM  Student
      WHERE Ssex=‘女’；

```

### 删除视图

语句的格式：

  DROP  VIEW  &lt;视图名&gt;；

该语句从数据字典中删除指定的视图定义

**如果该视图上还导出了其他视图**，使用CASCADE级联删除语句，把该视图和由它导出的所有视图一起删除

**删除基表**时，由该基表导出的所有视图定义都必须显式地使用DROP VIEW语句删除

删除视图IS\_S1

* 拒绝执行DROP VIEW IS\_S1；
* 级联删除：DROP VIEW IS\_S1 CASCADE;  

：





                     

## 3.7.2  查询视图

用户角度：查询视图与查询基本表相同 

RDBMS实现视图查询的方法 ：视图消解法（View Resolution）

* 进行有效性检查 
* 转换成等价的对基本表的查询 （和视图定义那段拼接起来）
* 执行修正后的查询

视图消解法的局限——有些情况下，视图消解法不能生成正确查询。

## 3.7.3  更新视图

自动转换为对基本表的更新

更新视图的限制：

* **一些视图是不可更新的**，因为对这些视图的更新**不能唯一地有意义地**转换成对相应基本表的更新
* 一个不允许更新的视图上定义的视图也不允许更新

比如group by 后的值，avg函数

## 3.7.4  视图的作用

### 视图能够简化用户的操作

当视图中数据不是直接来自基本表时，定义视图能够简化用户的操作

* 基于多张表连接形成的视图 
* 基于复杂嵌套查询的视图 
* 含导出属性的视图

### 视图使用户能以多种角度看待同一数据 

视图机制能使不同用户以不同方式看待同一数据，适应数据库共享的需要

### 视图对重构数据库提供了一定程度的逻辑独立性 

使用户的外模式保持不变，用户的应用程序通过视图仍然能够查找数据

视图只能在一定程度上提供数据的逻辑独立性 

由于对视图的更新是有条件的，因此应用程序中修改数据的语句可能仍会因基本表结构的改变而改变。

### 视图能够对机密数据提供安全保护

对不同用户定义不同视图，使每个用户只能看到他有权看到的数据

### 适当的利用视图可以更清晰的表达查询

