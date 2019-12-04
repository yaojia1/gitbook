---
description: 触发器是一种特殊的存储过程，常常用于实现强制业务规则和数据完整性。
---

# 第十章 触发器

## 10.1 触发器的创建与管理

触发器由SQL Server自动执行，不能由应用程序调用，这是它与存储过程不同的地方，便于保护数据库的完整性和完全性。

触发器在对表进行操作时（UPDATE、INSERT 或 DELETE）激活。

使用触发器有如下优点： 。

* 触发器是自动的：它们在对表的数据作了任何修改（比如手工输入或者应用程序采取的操作）之后立即被激活。 
* 触发器可以通过数据库中的相关表进行层叠更改。这比直接把代码写在前台的做法更安全合理。
*  触发器可以强制限制，这些限制比用 CHECK 约束所定义的更复杂。与 CHECK 约束不同的是，触发器可以引用其它表中的列

### 10.1.2 创建触发器

触发器用CREATE TRIGGER语句创建。语法如下：

```sql
CREATE TRIGGER trigger_name
ON { table | view }
[ WITH ENCRYPTION ]
{
    { { FOR | AFTER | INSTEAD OF } { [ INSERT ] [ , ] [ UPDATE ] }
        [ WITH APPEND ]
        [ NOT FOR REPLICATION ]
        AS
        [ { IF UPDATE ( column )
            [ { AND | OR } UPDATE ( column ) ]
                [ ...n ]
        | IF ( COLUMNS_UPDATED ( ) { bitwise_operator } updated_bitmask )
                { comparison_operator } column_bitmask [ ...n ]
        } ]
        sql_statement [ ...n ]
    }
}  
```

 Microsoft 不支持在系统表上添加用户定义触发器。

```sql
CREATE TABLE Company(
		CompanyID  varchar (10) NOT NULL ,
              CompanyName varchar (30) NOT NULL
                    )     
CREATE TABLE Contract (
		ContractID varchar (10) NOT NULL ,
		CompanyID varchar (10) NOT NULL ,
		ContractName varchar (30) NULL ,
		ContractVolume numeric(18, 2) NULL,
		SignDate Datetime NULL)

	CREATE TABLE ContractDetail (
		ContractID varchar (10) NOT NULL ,
		ContractDetailID varchar (10) NOT NULL ,
		Volume numeric(18, 2) NULL)
		
CREATE TRIGGER attetion
ON Company
FOR INSERT, UPDATE, DELETE 
AS
EXEC master..xp_sendmail 'RED', ‘公司信息表已发生变化！’

```

### 10.1.3 修改和删除触发器

```sql
--修改触发器用ALTER TRIGGER，语法如下：
ALTER TRIGGER trigger_name 
ON ( table | view ) 
[ WITH ENCRYPTION ] 
{ 
    { ( FOR | AFTER | INSTEAD OF ) { [ DELETE ] [ , ] [ INSERT ] [ , ] [ UPDATE ] } 
        [ NOT FOR REPLICATION ]
        AS
        sql_statement [ ...n ]
    } 
    | 
    { ( FOR | AFTER | INSTEAD OF ) { [ INSERT ] [ , ] [ UPDATE ] }
        [ NOT FOR REPLICATION ]
        AS
        { IF UPDATE ( column )
        [ { AND | OR } UPDATE ( column ) ]
        [ ...n ]
        | IF ( COLUMNS_UPDATED ( ) { bitwise_operator } updated_bitmask )
        { comparison_operator } column_bitmask [ ...n ]
        } 
        sql_statement [ ...n ] 
    } 
}
```



