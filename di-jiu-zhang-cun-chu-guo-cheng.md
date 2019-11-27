# 第九章  存储过程

## 9.1 游标的使用

使用游标（CURSOR）在需要一行一行处理时，游标十分有用。游标可以打开一个结果集合（按照指定的标准选择的行），并提供在结果集中一行一行处理的功能。基于游标的类型，可以对其进行回滚或者前进。

### 游标的声明 

用DECLARE语句对游标进行声明，有两种方法可以指定一个游标。

SQL-92 语法

DECLARE cursor\_name \[ INSENSITIVE \] \[ SCROLL \] CURSOR

FOR select\_statement

\[ FOR { READ ONLY \| UPDATE \[ OF column\_name \[ ,...n \] \] } \]

Transact-SQL 扩展语法

DECLARE cursor\_name CURSOR

\[ LOCAL \| GLOBAL \]

\[ FORWARD\_ONLY \| SCROLL \]

\[ STATIC \| KEYSET \| DYNAMIC \| FAST\_FORWARD \]

\[ READ\_ONLY \| SCROLL\_LOCKS \| OPTIMISTIC \]

\[ TYPE\_WARNING \]

FOR select\_statement \[ FOR UPDATE \[ OF column\_name \[ ,...n \] \] \]

```sql
--例程9.1：定义一个游标，用于生成学生信息表中的所有记录。
DECLARE student_cursor CURSOR
    FOR SELECT * FROM student
```

### 打开游标 

  打开游标就是创建结果集，执行游标定义时指定的 Transact-SQL 语句填充游标。语法如下：

OPEN { { \[ GLOBAL \] cursor\_name } \| cursor\_variable\_name }

### 使用游标读取数据 

     在从游标中读取数据的过程中，可以在结果集中的每一行上来回移动和处理。如果游标定义成了可滚动的（在声明时使用SCROLL关键字），则任何时候都可取出结果集中的任意行。对于非滚动的游标，只能对当前行的下一行实施取操作。结果集可以取到局部变量中。Fetch命令的语法如下：

FETCH \[NEXT \| PRIOR\| FIRST \| LAST \| ABSOLUTE {n \| @nvar} \| RELATIVE {n \| @nvar}\]

FROM \[GLOBAL\] cursor\_name} \| cursor\_variable\_name}

\[INTO @variable\_name \]\[,……n\]\]

```sql
--例程9.3：定义一个游标，返回学生信息表中所有的数据，打开游标，然后遍历学生信息表，
--直到找到学生名称为“张三丰”的记录为止，并且打印学生学号和学生名称。
/*定义游标*/
DECLARE student_cursor CURSOR
FOR SELECT sno,sname FROM student

		DECLARE @ID char(10), @Name char(30)
		OPEN student_cursor
		FETCH NEXT FROM student_cursor INTO @ID, @Name
/*while循环*/
WHILE @@fetch_status = 0
BEGIN
IF @Name = '张三丰'
      BEGIN
			PRINT '找到张三丰'
			PRINT @ID+@Name
			BREAK
      END
FETCH NEXT FROM student_cursor INTO @ID, @Name
END 
/*关闭释放游标*/
CLOSE student_cursor
DEALLOCATE student_cursor

```

### 关闭和释放游标

     游标使用之后，要及时对它进行关闭和释放操作。CLOSE语句用来关闭游标并释放结果集。游标关闭之后，不能再执行FETCH操作。如果还需要使用FETCH语句，则要重新打开游标。语法如下：

CLOSE \[GLOBAL\] cursor\_name \| cursor\_variable\_name

游标确实不再需要之后，要释放游标。DEALLOCATE语句释放数据结构和游标所加的锁。语法如下：

DEALLOCATE \[GLOBAL\] cursor\_name \| cursor\_variable\_name

## 9.2 存储过程的创建与管理

存储过程是一段在服务器上执行的程序，它在服务器端对数据库记录进行处理，再把结果返回到客户端。

包括系统存储过程和用户存储过程 

系统存储过程中又分为一般系统存储过程和扩展存储过程。

### 9.2.2 使用系统存储过程管理SQL SERVER

存储过程分为两类，**系统存储过程和用户自定义存储过程**，SQL SERVER提供了大量的系统存储过程，**用于管理SQL SERVER并显示有关数据库和用户的信息**。

    例程9.5：用系统存储过程列出当前SQL SERVER实例中的所有数据库以及数据库的大小，用sp\_databases。

  EXEC sp\_databases

  例程9.6：用系统存储过程列出当前数据中所有可以访问的表，用sp\_tables

  EXEC sp\_tables

### 9.2.3 使用扩展存储过程

扩展存储过程是系统存储过程的一种，提供从 SQL Server 到外部程序的接口，以便进行各种维护活动。扩展存储过程的前缀是xp\_

  例程9.7：用扩展存储过程显示当前目录下的全部文件，可以用xp\_cmdshell实现。    EXEC master..xp\_cmdshell ‘dir \*.\*’

### \*9.2.4 定义存储过程

#### 定义存储过程

可以用CREATE PROCEDURE语句来定义存储过程，语法如下：

CREATE PROC \[ EDURE \] procedure\_name \[ ; number \]

    \[ { @parameter data\_type }

        \[ VARYING \] \[ = default \] \[ OUTPUT \]

    \] \[ ,...n \]

\[ WITH

    { RECOMPILE \| ENCRYPTION \| RECOMPILE , ENCRYPTION } \]

\[ FOR REPLICATION \]

AS sql\_statement \[ ...n \]

#### procedure\_name

   新存储过程的名称。过程名必须符合标识符规则，且对于数据库及其所有者必须唯一。要创建局部临时过程，可以在 procedure\_name 前面加一个编号符 \(\#procedure\_name\)，要创建全局临时过程，可以在 procedure\_name 前面加两个编号符 \(\#\#procedure\_name\)。完整的名称（包括 \# 或 \#\#）不能超过 128 个字符。指定过程所有者的名称是可选的。

```sql
--例程9.9：创建一个存储过程，该存储过程能够实现根据系部的编号查询出系部中
--男生、女生的人数，运行结果如图所示。
USE students
GO
CREATE PROCEDURE proc_countsex @xbbh varchar(4)
AS
BEGIN
SELECT sex as '性别',count(id) as '人数合计'
 FROM student
 WHERE  substring(id,5,2)=@xbbh
 GROUP BY  sex
END
EXEC proc_countsex '02'

--例程9.10 在students学生数据库中，创建一个存储过程proc_ testOutput，
--要求实现返回成绩在@p1与@p3之间的学生人数，
--其中，@p1与@p3是输入参数，用于指定分数段。
CTEATE PROCEDURE dbo.proc_testOutput   
    ( 
    @p1 int , 
    @p2 int OUTPUT, 
    @p3 int  
    )    
AS 
BEGIN
    select @p2 = count(*) from stu where score between @p1 and @p3 
    RETURN @@rowcount
    PRINT @P2
END
--这个存储过程返回两个值，一个是output型参数@p2，调用存储过程后，返回结果存放在@p2中
--另外一个是由return值@@rowcount（语句所影响的行数）
```

## 存储过程使用例题

![](.gitbook/assets/image%20%2824%29.png)

![](.gitbook/assets/image%20%2821%29.png)

![](.gitbook/assets/image%20%2815%29.png)

写一个存储过程，按产品金额从高到低的次序，找出占库存总金额70%以上的所有产品，将这些产品的产品基本信息表中的ABC\_LEVEL字段赋值为’A’。 例如：如果数据库中内容如右表，则应将产品1001、1003的ABC\_LEVEL字段赋值为’A’,因为这两个产品的金额最高，且合计（400+350）〉（1000\*70%）。

![](.gitbook/assets/image%20%2826%29.png)

```sql
create proc find_A
as declare  @summ numeric(12,2),@summary numeric(12,2),@summ_t numeric(12,2), @pno char(10)
set @summary=0
set @summ_t=0
select @summ=sum(summary) from inventory
declare find_cursor cursor for select pno,summary from inventory order by summary desc
open find_cursor
fetch next from find_cursor into @pno,@summary
while @@fetch_status=0
begin
   set @summ_t=@summ_t+@summary
      begin
        if @summ_t>@summ *0.7
          begin
            update products set abc_level='A' where pno=@pno
            break
          end
        else
         update products set abc_level='A' where pno=@pno
     end
  fetch next from find_cursor into @pno,@summary
end
   close find_cursor
   deallocate  find_cursor
```

写一个存储过程，输入参数是学生的学号，计算这个学生成绩最好的三门课的平均成绩，并输出该平均成绩。当输入的学号不存在时，提示一条自定义错误信息

```sql
create proc cal_avg @sno char(10)
as
declare @score_avg  numeric(3,0),@sid char(10)
select @sid=sno from transcripts where sno=@sno
 begin
    if @sid is null 
      begin
         print '该学号不存在！'
        goto   t_over
      end
 end
  select top 3 score into #temp1  from transcripts  where sno=@sno  order by score  desc
  select  @score_avg=avg(score) from #temp1
  print '前三科平均成绩'
  print @score_avg
  t_over:
```

### 例三

创建一个SQL Server存储过程，该存储过程可以统计某几年各个学院参加竞赛获奖的人数（起止年份作为输入参数）,并在学院信息表里的备注里给获奖人数最多的前三个学院写上“最佳竞赛组织奖获得单位”。

![](.gitbook/assets/image%20%2825%29.png)

```sql
create proc findschool  @startyear  date，@endyear  date
as declare @ schoolname  char(20) , @num numeric(1,0)
set @num=0                                      
declare findcursor cursor for 
select schname from student, school, prizeinfo 
where student.schid=school.schid 
      and student.sid= prizeinfo.sid 
      and   pyear between  @startyear  
      and  @endyear  
      group by schname  
      order by count(student.sid)  desc                                 
open findcursor 
fetch next from findcursor into @ schoolname
while @@fetch_status=0
begin
   set @num =@num +1
      begin
        if @num >=3
          begin 
             update school set remark=“最佳竞赛组织奖获得单位”  
             where schname =@ schoolname
           break
          end
       else
         update school set remark=“最佳竞赛组织奖获得单位”  
         where schname =@ schoolname
         end
  fetch next from findcursor into @ schoolname                
end
   close findcursor 
   deallocate  findcursor

--另外一种写法：
create proc findschool  @startyear  date，@endyear  date
As
update school 
set remark=“最佳竞赛组织奖获得单位”  
where schname in
(
    select  top 3 schname
    from student, school, prizeinfo 
   where student.schid=school.schid 
         and student.sid=  prizeinfo.sid 
         and   pyear between  @startyear  and  @endyear  
   group by schname  
   order by count(student.sid)  desc   
)                              

```



