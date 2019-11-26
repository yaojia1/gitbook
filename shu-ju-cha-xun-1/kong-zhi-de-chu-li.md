# 空值的处理

空值就是“不知道”或“不存在”或“无意义”的值。 

一般有以下几种情况： 

1. 该属性应该有一个值，但目前不知道它的具体值 
2. 该属性不应该有值 
3. 由于某种原因不便于填写

## 空值的产生 

空值是一个很特殊的值，含有不确定性。对关系运算带来特殊的问题，需要做特殊的处理。

#### 插入空

```text
--向SC表中插入一个元组，学生号是”201215126”，课程号是”1”，成绩为空。
 INSERT INTO SC(Sno,Cno,Grade)
 VALUES('201215126 ','1',NULL);   /*该学生还没有考试成绩，取空值*/
--或
 INSERT INTO SC(Sno,Cno)
 VALUES(' 201215126 ','1');             /*没有赋值的属性，其值为空值*/

```

#### 设置空值

```text
将Student表中学生号为”201215200”的学生所属的系改为空值。
	UPDATE Student
	SET Sdept = NULL
	WHERE Sno='201215200';
```

## 空值的判断 

判断一个属性的值是否为空值，用IS NULL或IS NOT NULL来表示。

```text
--从Student表中找出漏填了数据的学生信息
	SELECT  *
	FROM Student
	WHERE Sname IS NULL OR Ssex IS NULL OR Sage IS NULL OR Sdept IS NULL;
```

## 空值的约束条件 

属性定义（或者域定义）中

* 有NOT NULL约束条件的不能取空值 
* 码属性不能取空值

## 空值的算术运算、比较运算和逻辑运算

1. 空值与另一个值（包括另一个空值）的算术运算的结果为空值 
2. 空值与另一个值（包括另一个空值）的比较运算的结果为UNKNOWN。 
3. 有UNKNOWN后，传统二值（TRUE，FALSE）逻辑就扩展成了三值逻辑

![](../.gitbook/assets/image%20%288%29.png)

```text
--找出选修1号课程的不及格的学生。
   SELECT Sno
   FROM SC
   WHERE Grade < 60 AND Cno='1';
--查询结果不包括缺考的学生，因为他们的Grade值为null。

--选出选修1号课程的不及格的学生以及缺考的学生
SELECT Sno
FROM SC
WHERE Grade < 60 AND Cno='1'
UNION
SELECT Sno
FROM SC
WHERE Grade IS NULL AND Cno='1'
或者
SELECT Sno
FROM SC
WHERE Cno='1' AND (Grade<60 OR Grade IS NULL);

```

