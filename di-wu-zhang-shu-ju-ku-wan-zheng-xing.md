# 第五章 数据库完整性

数据库的完整性——数据的正确性和相容性

#### 数据的完整性和安全性是两个不同概念 

*  数据的完整性
  * 防止数据库中存在不符合语义的数据，也就是防止数据库中存在不正确的数据 
  * 防范对象：不合语义的、不正确的数据 
* 数据的安全性 
  * 保护数据库防止恶意的破坏和非法的存取 
  * 防范对象：非法用户和非法操作

为维护数据库的完整性，DBMS必须： 

1.提供定义完整性约束条件的机制 

2.提供完整性检查的方法 

3.违约处理

## 5.1  实体完整性

### 5.1.1 实体完整性定义 

关系模型的实体完整性 

> CREATE TABLE中用PRIMARY KEY定义

**单属性**构成的码有两种说明方法 

* 定义为列级约束条件 
* 定义为表级约束条件 

对**多个属性**构成的码只有一种说明方法 

* 定义为表级约束条件

#### \(1\)在列级定义主码

              CREATE TABLE Student

                \(Sno  CHAR\(9\)  PRIMARY KEY，

#### \(2\)在表级定义主码

   CREATE TABLE Student

        \(Sno  CHAR\(9\)， 

         Sname  CHAR\(20\) NOT NULL，

         Ssex  CHAR\(2\) ，

         Sage  SMALLINT，

         Sdept  CHAR\(20\)，

         **PRIMARY KEY \(Sno\)**

      \);

将SC表中的Sno，Cno属性组定义为码

      CREATE TABLE SC

           \(Sno   CHAR\(9\)  NOT NULL，

            Cno  CHAR\(4\)  NOT NULL， 

            Grade    SMALLINT，

            **PRIMARY KEY \(Sno，Cno\)     /\*只能在表级定义主码\*/**

          \);

### 5.1.2 实体完整性检查和违约处理

插入或对主码列进行更新操作时，RDBMS按照实体完整性规则自动进行检查。包括： 

1. 检查主码值是否唯一，如果不唯一则拒绝插入或修改 

2. 检查主码的各个属性是否为空，只要有一个为空就拒绝插入或修改

## 5.2  参照完整性

### 5.2.1关系模型的参照完整性定义 

* 在CREATE TABLE中用**FOREIGN KEY**短语定义哪些列为**外码** 
* 用**REFERENCES**短语指明这些**外码参照哪些表的主码**

```text
--关系SC中一个元组表示一个学生选修的某门课程的成绩，（Sno，Cno）是主码。Sno，Cno分别参照引用Student表的主码和Course表的主码 
--例5.3定义SC中的参照完整性
      CREATE TABLE SC
         (Sno    CHAR(9)  NOT NULL， 
          Cno     CHAR(4)  NOT NULL，  
          Grade    SMALLINT，
          PRIMARY KEY (Sno， Cno)，   /*在表级定义实体完整性*/
          FOREIGN KEY (Sno) REFERENCES Student(Sno)，  
            /*在表级定义参照完整性*/
          FOREIGN KEY (Cno) REFERENCES Course(Cno)    
          /*在表级定义参照完整性*/
      );
```

### 5.2.2参照完整性违约处理

#### （1） 拒绝（NO ACTION）执行l

* 不允许该操作执行。该策略一般设置为默认策略

#### （2） 级联（CASCADE）操作l

* 当删除或修改**被参照表**（Student）的一个元组造成了与**参照表**（SC）的不一致，则删除或修改参照表中的所有造成不一致的元组

#### （3）设置为空值（SET-NULL）l

* 当删除或修改**被参照表的一个元组时造成了不一致**，则将**参照表中**的所有造成不一致的元组的对应属性**设置为空值**。

> 例如，有下面2个关系
>
>     学生（学号，姓名，性别，专业号，年龄）
>
>     专业（专业号，专业名）
>
> * 假设专业表中某个元组被删除，专业号为12 
> * 按照设置为空值的策略，就要把学生表中专业号=12的所有元组的专业号设置为空值 
> * 对应语义：某个专业删除了，该专业的所有学生专业未定，等待重新分配专业 
> * 对于参照完整性，除了应该定义外码，还应定义外码列是否允许空值

```text
--显式说明参照完整性的违约处理示例
    CREATE TABLE SC
        (Sno   CHAR(9)  NOT NULL，
         Cno   CHAR(4)  NOT NULL，
         Grade  SMALLINT，
         PRIMARY KEY（Sno，Cno）， 				
         FOREIGN KEY (Sno) REFERENCES Student(Sno) 
		ON DELETE CASCADE     /*级联删除SC表中相应的元组*/
                ON UPDATE CASCADE， /*级联更新SC表中相应的元组*/
         FOREIGN KEY (Cno) REFERENCES Course(Cno) 	                    
               ON DELETE NO ACTION 	
               /*当删除course 表中的元组造成了与SC表不一致时拒绝删除*/
               ON UPDATE CASCADE   
      	/*当更新course表中的cno时，级联更新SC表中相应的元组*/
        )；
```

## 5.3  用户定义的完整性

用户定义的完整性就是针对某一具体应用的数据必须满足的语义要求 

RDBMS提供，而不必由应用程序承担

### 5.3.1 属性上的约束条件的定义 

CREATE TABLE时定义 

#### 列值非空（NOT NULL） 

```sql
--例5.5］  在定义SC表时，说明Sno、Cno、Grade属性不允许取空值。
    CREATE TABLE SC
      （Sno  CHAR(9)  NOT NULL，	
          Cno  CHAR(4)  NOT NULL，	
          Grade  SMALLINT NOT NULL，	
          PRIMARY KEY (Sno， Cno)，  
          /* 如果在表级定义实体完整性，隐含了Sno，Cno不允许取空值，
          则在列级不允许取空值的定义就不必写了 * /
        ）； 
```

#### 列值唯一（UNIQUE） 

```sql
--建立部门表DEPT，要求部门名称Dname列取值唯一，部门编号Deptno列为主码
    CREATE TABLE DEPT
        (Deptno  NUMERIC(2)，
          Dname  CHAR(9)  UNIQUE，/*要求Dname列值唯一*/
          Location  CHAR(10)，
          PRIMARY KEY (Deptno)
       )；
```

#### 检查列值是否满足一个布尔表达式（CHECK）

```sql
--Student表的Ssex只允许取“男”或“女”。
    CREATE TABLE Student
        (Sno  CHAR(9) PRIMARY KEY，
          Sname CHAR(8) NOT NULL，                     
          Ssex  CHAR(2)  CHECK (Ssex IN (‘男’，‘女’) ) ，                
              /*性别属性Ssex只允许取'男'或'女' */
          Sage  SMALLINT，
          Sdept  CHAR(20)
        );
--SC表的Grade的值应该在0和100之间。
   CREATE TABLE  SC
         (  Sno     CHAR(9) ,
            Cno    CHAR(4),
	Grade   SMALLINT CHECK (Grade>=0 AND Grade <=100)，
	/*Grade取值范围是0到100*/
            PRIMARY KEY (Sno,Cno),
            FOREIGN KEY (Sno) REFERENCES Student(Sno),
            FOREIGN KEY (Cno) REFERENCES Course(Cno)
           );

```

### 5.3.2 元组上的约束条件的定义

* 在CREATE TABLE时可以用CHECK短语定义元组上的约束条件，即元组级的限制 
* 同属性值限制相比，元组级的限制可以设置不同属性之间的取值的相互约束条件

```sql
当学生的性别是男时，其名字不能以Ms.打头。
    CREATE TABLE Student
         (Sno    CHAR(9)， 
          Sname  CHAR(8) NOT NULL，
          Ssex    CHAR(2)，
          Sage   SMALLINT，
          Sdept  CHAR(20)，
          PRIMARY KEY (Sno)，
          CHECK (Ssex='女' OR Sname NOT LIKE 'Ms.%')
          /*定义了元组中Sname和 Ssex两个属性值之间的约束条件*/
        )；
--性别是女性的元组都能通过该项检查，因为Ssex=‘女’成立；
--当性别是男性时，要通过检查则名字一定不能以Ms.打头
```

## 5.4  完整性约束命名字句

### CONSTRAINT 约束

CONSTRAINT &lt;完整性约束条件名&gt;

［PRIMARY KEY短语 

  \|FOREIGN KEY短语

   \|CHECK短语］

```sql
/*建立学生登记表Student，要求学号在90000~99999之间，
姓名不能取空值，年龄小于30，性别只能是“男”或“女”。*/
CREATE TABLE Student
      (Sno  NUMERIC(6)
        CONSTRAINT C1 CHECK (Sno BETWEEN 90000 AND 99999)，
        Sname  CHAR(20)  
        CONSTRAINT C2 NOT NULL，
        Sage  NUMERIC(3)
        CONSTRAINT C3 CHECK (Sage < 30)，
        Ssex  CHAR(2)
        CONSTRAINT C4 CHECK (Ssex IN ( '男'，'女'))，
        CONSTRAINT StudentKey PRIMARY KEY(Sno)
      )；
/*在Student表上建立了5个约束条件，
包括主码约束（命名为StudentKey）以及C1、C2、C3、C4四个列级约束。*/
--建立教师表TEACHER，要求每个教师的应发工资不低于3000元。
--应发工资是工资列Sal与扣除项Deduct之和。
      CREATE TABLE TEACHER
              (   Eno    NUMERIC(4)  PRIMARY KEY    /*在列级定义主码*/
                  Ename  CHAR(10),
                  Job     CHAR(8),
 		      Sal     NUMERIC(7,2),
 		      Deduct  NUMERIC(7,2),
 		      Deptno  NUMERIC(2),
  		      CONSTRAINT TEACHERFKey 
                   FOREIGN KEY (Deptno) REFERENCES DEPT(Deptno),
  		      CONSTRAINT C1 CHECK (Sal + Deduct >= 3000) 
                );

```

### 修改表中的完整性限制 

使用ALTER TABLE语句修改表中的完整性限制

可以先删除原来的约束条件，再增加新的约束条件

```sql
--去掉例5.10 Student表中对性别的限制。
        ALTER TABLE Student 
        DROP CONSTRAINT C4;
--修改表Student中的约束条件，要求学号改为在900000~999999之间，年龄由小于30改为小于40
      ALTER TABLE Student
        DROP CONSTRAINT C1;
        ALTER TABLE Student
        ADD CONSTRAINT C1 CHECK (Sno BETWEEN 900000 AND 999999)，
        ALTER TABLE Student
        DROP CONSTRAINT C3;
        ALTER TABLE Student
        ADD CONSTRAINT C3 CHECK (Sage < 40)；
```

## 5.5  小结

数据库的完整性是为了保证数据库中存储的数据是正确的

RDBMS完整性实现的机制

* §完整性约束定义机制
* §完整性检查机制
* §违背完整性约束条件时RDBMS应采取的动作

