# 第八章  Transact-SQL程序设计

### 本章学习目标： 

* 掌握系统提供的数据类型 
* 掌握变量的使用方法 
* 掌握运算符和表达式的使用 
* 掌握函数的定义和使用方法 
* 掌握流程控制语句的使用

### Transact-SQL语言包括以下四个部分： 

数据定义语言\(DDL\)：定义和管理数据库及其对象，例如：Create、Alter和Drop等语句。

 数据操作语言\(DML\)：操作数据库中各对象，例如：Insert、Update、Delete和Select语句。 

数据控制语言\(DCL\)：进行安全管理和权限管理等，例如：Grant、Revoke、Deny等语句。 

附加的语言元素：Transact-SQL语言的附加语言元素，包括变量、运算符、函数、注释和流程控制语句等。

## 8.1 系统提供的数据类型

### 整数数据类型 

1、int   
     int 数据类型存储从-231 到231-1（2 ，147 ，483，647） 之间的所有正负整数。每个INT 类型的数据按4 个字节存储。 

2、smallint  
     smallint 数据类型存储从-215 到215 -1 （ 32,767 ）之间的所有正负整数。每个smallint 类型的数据占用2 个字节的存储空间。

 3、tinyint  
     tinyint数据类型存储从0 到255 之间的所有正整数。每个tinyint类型的数据占用1 个字节的存储空间。

 4、bigint  
     bigint 数据类型存储从-263  到263-1 之间的所有正负整数。每个bigint 类型的数据占用8个字节的存储空间。 

### 浮点数据类型

    浮点数据类型用于存储十进制小数。采用上舍入（Round up 或称为只入不舍）方式进行存储。所谓上舍入是指，当（且仅当）要舍入的数是一个非零数时，对其保留数字部分的最低有效位上的数值加1 ，并进行必要的进位。如：对3.141保留2 位，结果为3.15 。

1、单精度类型（real）   
     real数据类型可精确到第7 位小数，其范围为从-3.40E -38 到3.40E +38。 每个real类型的数据占用4 个字节的存储空间。

2、双精度类型（float）  
     float数据类型可精确到第15 位小数，其范围为从-1.79E -308 到1.79E +308。 每个float 类型的数据占用8 个字节的存储空间。 float数据类型可写为float\[ n \]的形式。n 指定float 数据的精度。n 为1到15 之间的整数值。

 3、精确数值类型（decimal、numeric）          decimal数据类型可以提供小数所需要的实际存储空间，但也有一定的限制，可以用2 到17 个字节来存储从-1038 +1到1038-1 之间的数值。可将其写为decimal\( p， \[s\]\)或Numeric\(p,\[s\]\)的形式，p 和s 确定了精确的比例和数位。 例如：decimal （15， 5），表示共有15 位数，其中整数10 位，小数5。 

### 字符数据类型

 是使用最多的数据类型，使用时在其前后加上单引号’或双引号” 。 

1、 char

         char 数据类型的定义形式为char\(n\)。每个字符和符号占一个字节的存储空间，n 取值为1 到8000，若输入数据的字符数小于n，则系统自动在其后添加空格来填满设定好的空间。若输入的数据过长，将会截掉其超出部分。

 2、nchar  
     nchar数据类型的定义形式为nchar\(n\)。 它与char 相似，不同的是nchar数据类型n 的取值为1 到4000。 采用UNICODE 标准字符集， 每个字符占用两个字节的存储空间，可以将全世界的语言文字都囊括在内，在一个数据列中可以同时出现中文、英文、法文、德文等。

3、varchar  
     varchar数据类型的定义形式为varchar\(n\)。与char 类型相似，n 的取值也为1 到8000， 不同的是，varchar数据类型具有变动长度的特性，存储长度为实际数值长度，若输入数据的字符数小于n ，则系统不会在其后添加空格来填满设定好的空间。 一般情况下，由于char 数据类型长度固定，因此它比varchar 类型的处理速度快。

 4、nvarchar  
    nvarchar数据类型的定义形式为nvarchar\(n\)。 它与varchar 类型相似。不同的是，nvarchar数据类型采用UNICODE 标准字符集， n 的取值为1 到4000。

5. Unicode 

是一种重要的交互和显示的通用字符编码标准，它覆盖了美国、欧洲、中东、非洲、印度、亚洲和太平洋的语言，以及古文和专业符号。

  Unicode 允许交换、处理和显示多语言文本以及公用的专业和数学符号。它希望能够解决多语言的计算，如不同国家的字符标准，但并不是所有的现代或古文都能够获得支持。

   Unicode 字符可以适用于所有已知的编码。

   Unicode 是继 ASCII（美国国家交互信息标准编码）字符码后的一种新字符编码，它为每一个符号定义一个数字和名称，并指定字符和它的数值（码位），以及该值的二进制位表示法，通过一个十六进制数字和前缀（U）定义一个16位的数值，如：U+0041 表示 A，其唯一的名称是 LATIN CAPITAL LETTER A。但请注意：JavaScript 1.3 之前的版本并不支持 Unicode 编码。    Unicode 与 ASCII 和 ISO 兼容  


### 日期和时间数据类型

1、 datetime  
     用于存储日期和时间的结合体。它可以存储从公元1753 年1 月1 日零时起到公元9999 年12 月31 日23 时59 分59 秒之间的所有日期和时间，其精确度可达三百分之一秒，即3.33 毫秒。所占用的存储空间为8 个字节。

2、 smalldatetime  
    smalldatetime 数据类型与datetime 数据类型相似，但其日期时间范围较小，为从1900 年1 月1 日到2079 年6 月6日，时间精度较低，只能精确到分钟，其分钟个位上为根据秒数四舍五入的值,即以30 秒为界四舍五入。使用4 个字节存储数据。

3、date      date是SQL Server 2008新引进的数据类型。它表示一个日子，不包含时间部分，可以表示的日期范围从公元元年1月1日到9999年12月31日。只需要3个字节的存储空间。

#### 日期的输入格式很多大致可分为三类： 

1、英文+数字格式 

此类格式中月份可用英文全名或缩写，以下格式均为正确的日期格式： June 21 2000、 Oct 1 1999、 January 2000 、2000 February 

2. 数字+分隔符格式 

允许把斜杠（/）、连接符（-）和小数点（.）作为用数字表示的年、月、日之间的分隔符。如： YMD：2000/6/22 2000-6-22 2000.6.22 MDY：3/5/2000 3-5-2000 3.5.2000 DMY：31/12/1999 31-12-1999 31.12.2000  


3. 纯数字格式 

纯数字格式是以连续的4 位、6位或8 位数字来表示日期。20000601---2000 年6 月1 日 991212---1999 年12 月12 日 1998---1998 年 1月1日 时间输入格式 在输入时间时必须按“小时、分钟、秒、毫秒”的顺序来输入，用冒号隔开。当使用12 小时制时用AM（am） 和PM（pm）分别指定时间是午前或午后，若不指定，系统默认为AM。如： 3:5:7.2pm---下午3 时5 分7 秒200 毫秒 10:23:5.123Am---上午10 时23 分5 秒123 毫秒 可以使用SET DATEFORMAT 命令来设定系统默认的日期-时间格式。

### 二进制数据类型

  1、binary  
     用于存储二进制数据。其定义形式为binary（ n）， n 表示数据的字节数（长度），取值为1 到8000 。binary 类型数据占用n+4 个字节的存储空间。在输入数据时必须在数据前加上字符“0X” 作为二进制标识。

2、varbinary  
     varbinary数据类型的定义形式为varbinary（n）。 它与binary 类型相似，n 的取值也为1 到8000，varbinary数据类型具有变动长度的特性，存储长度为实际数值长度+4个字节。由于binary 数据类型长度固定，因此它比varbinary 类型的处理速度快。

### 逻辑数据类型

     bit数据类型占用1 个字节的存储空间，其值为0 或1 。如果输入0 或1 以外的值，将被视为1。 bit 类型不能定义为NULL 值。

### 文本和图形数据类型

  这类数据类型用于存储大量的字符或二进制数据。

1、text  
     text数据类型用于存储大量文本数据，其容量理论最大为231-1 （2G）个字节，在实际应用时需要视硬盘的存储空间而定。

2、 ntext  
     ntext数据类型与text类型相似,不同的是ntext 类型采用UNICODE 标准字符集\(Character Set\), 因此其理论容量为230-1\(1G\)个字节。

 3、 image  
     image数据类型用于存储大量的二进制数据Binary Data。 其理论容量为231-1\(2G\)个字节。其存储数据的模式与text 数据类型相同。通常用来存储图形等OLE \(Object Linking and Embedding，对象连接和嵌入）对象。在输入数据时同binary数据类型一样，必须在数据前加上字符“0X”作为二进制标识 。

### 货币数据类型     

货币数据类型用于存储货币值。在使用货币数据类型时，应在数据前加上货币符号，系统才能辨识其为哪国的货币，如果不加货币符号，则默认为“￥”。

1、 money

    money 数据类型的数据是一个有4 位小数的decimal 值，其取值从-2 63到263-1（+922，337，203，685，477.5807），数据精度为万分之一货币单位。money 数据类型使用8个字节存储。

2、 smallmoney     

smallmoney数据类型类似于money 类型，但其存储的货币值范围比money数据类型小,其取值从-214,748.3648到+214,748.3647,存储空间为4 个字节。  


## 8.2 常量、变量及运算符

* 常量，也称为文字值或标量值，是表示一个特定数据值的符号，在程序运行过程中其值保持不变。 
* 常量的格式取决于它所表示的值的数据类型 
* 变量是可以对其赋值并参与运算的一个实体，其值在运行过程中可以发生改变。变量可以分为全局变量和局部变量两类。 
* 表达式是指将常量、变量、函数等，用运算符按一定的规则连接起来的有意义的式子。

### 8.2.1 常量

1.字符串常量

字符串常量括在单引号内并包含字母、数字字符（a-z、A-Z 和 0-9）以及特殊字符，如!、@ 和\#。

2．数值常量

数值常量以没有用引号括起来的数字字符串来表示，包括Integer常量、Decimal常量、Float和Real常量等，其中Integer常量没有小数点，例如100，54等；Decimal常量包含小数点，例如123.45，5.6等；Float 和 Real 常量使用科学记数法来表示，例如123E2，0.3E-3等。

如果要表示一个数是正数还是负数，可以对数值常量应用 + 或 -运算符。

3．日期时间常量

    日期时间常量使用特定格式的字符日期时间值来表示，并被单引号括起来。

4. 二进制常量

     二进制常量用不加引号的数字0和1表示，如果使用大于1的数字表示，则转换为1。

5. 十六进制常量

    十六进制常量用加前缀0x的十六进制形式表示，例如0x24EB5、0xCF21A等。

6.空值

    空值是表示值未知，不同于空白或零值，用Null来表示。比较两个空值或将空值与任何其他值相比均返回未知，这是因为每个空值均为未知。在往表中添加记录时，如果不对某一列赋值则系统自动让该列取空值，或者也可以在Insert语句或Update语句中显式地对某列赋空值。

### 8.2.2 变量

变量是可以对其赋值并参与运算的一个实体，其值在运行过程中可以发生改变。 

变量可以分为全局变量和局部变量两类 

全局变量由系统定义并维护，局部变量由用户定义并赋值。局部变量的用法非常广泛，除了可以参加运算构成表达式之外，还可以在程序中保存中间结果、控制循环执行次数、保存存储过程的输出结果和函数的返回值等

#### 1.全局变量

全局变量由系统定义，通常用来跟踪服务器范围和特定会话期间的信息，不能被用户显式地定义和赋值，但是我们可以通过访问全局变量来了解系统目前的一些状态信息 ,全局变量以两个@符号开头 

![SQL Server&#x4E2D;&#x8F83;&#x5E38;&#x7528;&#x7684;&#x5168;&#x5C40;&#x53D8;&#x91CF;](.gitbook/assets/image%20%2831%29.png)

2.声明局部变量

* 局部变量一般出现在批处理、存储过程和触发器中，必须在使用前用Declare 语句声明：
* 指定局部变量名称。名称的第一个字符必须是@。
* 指定变量的数据类型，可以是系统提供的数据类型或用户自定义数据类型。对于字符型变量，还可以指定长度；数值型变量，指定精度和小数位数。赋初值Null
* Declare语句的语法如下：

Declare @local\_variable \[AS\] data\_type \| \[ = value \]\[,…\]

#### 使用Set语句或SELECT语句 给变量赋值

格式如下：

    SET @local\_variable = expression

    SELECT @local\_variable = expression\[,…n\]

    其中，**@local\_variable是变量的名称**，expression是任何有效的SQL Server表达式，可以是常量、变量、函数和表达式，还可以是子查询。

    SELECT与SET不同的是：**SELECT可以一次为多个变量赋值。**

### 8.2.3 运算符

Microsoft SQL Server 2008提供了7中类型的运算符，分别是算术运算符、赋值运算符、位运算符、比较运算符、逻辑运算符、字符串运算符和一元运算符。

### 8.2.4 运算符的优先级

![](.gitbook/assets/image%20%2816%29.png)

## 8.3 常用函数

函数是能够完成特定功能并返回处理结果的一组Transact-SQL语句，处理结果称为“返回值”，处理过程称为“函数体”。函数可以用来构造表达式，可以出现在Select语句的选择列表中，也可以出现在Where子句的条件中。SQL Server提供了许多系统内置函数，同时也允许用户根据需要自己定义函数。 

SQL Serve提供的常用的内置函数主要有以下几类：数学函数、字符串函数、日期函数、转换函数、聚合函数等

### 常用的数学函数有： 

Abs \( numeric\_expression \)：返回指定数值表达式的绝对值

Round \( numeric\_expression , length \[ ,function \] \)：返回一个舍入到指定的长度或精度的数值 

Floor \( numeric\_expression \)：返回小于或等于指定数值表达式的最大整数 

Ceiling \( numeric\_expression \)：返回大于或等于指定数值表达式的最小整数 

Power \( float\_expression , y \)：返回指定表达式的指定幂的值 

Sqrt \( float\_expression \)：返回指定表达式的平方根 

Square \( float\_expression \)：返回指定表达式的平方 

Exp \( float\_expression \)：返回指定的表达式的指数值 

Log \( float\_expression \)：返回指定表达式的自然对数 

Log10 \( float\_expression \)：返回指定表达式的以10为底的对数 

Sin \( float\_expression \)：返回指定角度（以弧度为单位）的三角正弦值 

Cos \( float\_expression \)：返回指定角度（以弧度为单位）的三角余弦值

### 常用的字符串函数有： 

Ascii \( character\_expression \)：返回字符表达式中最左侧的字符的 ASCII 代码值 

Char \( integer\_expression \)：将 int型的 ASCII 代码转换为字符 

Str \( float\_expression \[ , length \[ , decimal \] \] \)：返回由数字数据转换来的字符数据 

Len \( string\_expression \)：返回指定字符串表达式的字符数，其中不包含尾随空格 

Substring \( value\_expression ,start\_expression , length\_expression \)：返回字符表达式的从start\_expression位置开始的长度为length\_expression的子串 

Left \( character\_expression , integer\_expression \)：返回字符串中从左边开始指定个数的字符 

Right \( character\_expression , integer\_expression \)：返回字符串中从右边开始指定个数的字符 

Ltrim\( character\_expression \)：返回删除了前导空格之后的字符表达式 

Rtrim \( character\_expression \)：截断所有尾随空格后返回一个字符串

### 日期时间函数：

Getdate \( \)：返回系统当前的日期和时间 

Year \( date \)：返回表示指定参数 date 的“年”部分的整数 

Month \( date \)：返回表示指定参数 date 的“月”部分的整数 

Day \( date \)：返回表示指定参数date 的“日”部分的整数 

Datename \( datepart , date \)：返回表示指定参数date的指定 datepart参数 的字符串 

Datepart \( datepart , date \)：返回表示指定参数 date 的指定 datepart参数 的整数 

Datediff \( datepart , startdate , enddate \)：根据指定datepart参数返回两个指定日期之间的差值 

Dateadd \(datepart , number , date \)：根据datepart参数将一个时间间隔与指定 date 参数相加，返回一个新的 datetime 值

### Convert函数

Convert函数可以将一种数据类型的表达式强制转换为另一种数据类型的表达式。

Convert函数的语法格式为：vConvert \( data\_type \[ \( length \) \] , expression \[ , style \] \)

参数说明：vexpression ：任何有效的表达式。vdata\_type ：目标数据类型。vlength ：指定目标数据类型长度的可选整数。vstyle ：用于日期时间型数据类型和字符数据类型的转换。

## 8.4 用户自定义函数

通过用户自定义函数可以接受参数，执行复杂的操作并将操作的结果以值的形式返回。根据函数返回值的类型，可以把SQL Server用户自定义函数分为 **标量值函数（数值函数）**和 **表值函数（内联表值函数和多语句表值函数）**。数值函数返回结果为单个数据值；表值函数返回结果集（table数据类型）。

### 8.4.1 使用Create Function创建

#### 1.创建标量值函数 

标量值函数的函数体由一条或多条Transact-SQL语句组成，这些语句以Begin开始，以End结束。创建标量值函数的语法为：

```text
Create  Function  function_name 
([ { @parameter_name [ As ] parameter_data_type  [ = default ] [ Readonly ] } 
[ ,...n ]
]
)
Returns return_data_type
 [ With Encryption ]
[ As ]
Begin 
function_body 
Return scalar_expression
End

```

* owner\_name

  拥有该用户定义函数的用户ID的名称。

* unction\_name用户定义函数的名称，符合命名规则，在库中唯一
* @parameter\_name

  用户定义函数的参数，1到多个

* scalar\_parameter\_data\_type

    参数的数据类型

* scalar\_return\_data\_type

    函数返回值 

* function\_body

    指定一系列T-SQL语句定义函数的值

#### 2. 创建内联表值函数

创建内联表值函数的语法如下：

```text
Create  Function  function_name 
( [ { @parameter_name [ As ] parameter_data_type  [ = default ] [ Readonly ] } 
   [ ,...n ]
]
)
Returns Table
[ With Encryption ]
[ As ]
Return （select_statement）

-例
CREATE FUNCTION ProductStore (@productid varchar(30))
RETURNS TABLE
AS
RETURN (SELECT productid,amount,volume
      	FROM Storage
      	WHERE Productid=@productid)
```

说明： 

内联表值函数没有函数体 

Return Table子句指明该用户自定义函数的返回值是一个表。 

Return子句中的Select语句决定了返回表中的数据。

#### 3. 多语句表值函数

与内联表值函数不同的是，多语句表值函数在返回语句之前还有其他的Transact-SQL语句，具体的语法如下：

```text
Create  Function  function_name 
( [ { @parameter_name [ As ] parameter_data_type  [ = default ] [ Readonly ] } 
   [ ,...n ]
]
)
Returns @return_variable Table <table_type_definition>
[ With Encryption ]
[ As ]
Begin 
function_body 
Return
End
-例
CREATE FUNCTION get_Company (@InCompanyID nchar(5))
RETURNS @returnFind TABLE (CompanyID nchar(5) primary key,
   		CompanyName nvarchar(50) NOT NULL,
   		GenCompanyID nchar(5),
   		Remark nvarchar(30))
AS
BEGIN
   	DECLARE @RowsAdded int
      	DECLARE @reports TABLE (CompanyID nchar(5) primary key, 
      	CompanyName nvarchar(50) NOT NULL,
      	GenCompanyID nchar(5),
      	Remark nvarchar(30),
      	processed tinyint default 0)
   	INSERT @reports
   		   SELECT CompanyID, CompanyName, GenCompanyID, Remark, 0
   		   FROM Company 
   		   WHERE CompanyID = @InCompanyID 
SET @RowsAdded = @@rowcount
   		WHILE @RowsAdded > 0
   		BEGIN
      	       UPDATE @reports SET processed = 1 WHERE processed = 0
      	      INSERT @reports
      	        SELECT c.CompanyID, c.CompanyName, c.GenCompanyID, c.Remark, 0
      	         FROM Company c, @reports r
                         WHERE c.GenCompanyID=r.CompanyID 
                             and c.GenCompanyID <> c.CompanyID and r.processed = 1
      	    SET @RowsAdded = @@rowcount
      	    UPDATE @reports SET processed = 2	WHERE processed = 1
   		END
    		INSERT @returnFind
   		  SELECT CompanyID, CompanyName, GenCompanyID, Remark 
   		  FROM @reports
   		RETURN
END
-调用该函数方法如下：
	SELECT *  FROM get_Company ('00011')

```

### 8.4.2 使用“SQL Server Management Studio”创建

具体的步骤为：

\(1\)展开选定的数据库下的“可编程性”节点。

\(2展开“函数”节点，可以看到“标量值函数”和“表值函数”节点。

\(3\)在相应的节点上单击鼠标右键，在弹出的快捷菜单中选择“新建…函数”。

\(4\)在新建的查询窗口中可以看到关于自定义函数的语句模板，在其中添上相应的内容，单击工具栏上的“执行”按钮即可。

### 8.4.3 修改自定义函数

跟创建自定义函数一样，函数的修改既可以通过Alter Function语句进行，也可以通过对象资源管理器进行。

### 8.4.4. 删除自定义函数

1.利用对象资源管理器删除函数

在“SQL Server Management Studio”中，找到需要删除的函数节点，在其上单击鼠标右键，在弹出的快捷菜单中选择“删除”选项，弹出确认删除窗口，选择“确定”即可删除。

2.使用Drop Function语句删除函数

使用Drop Function语句可以从当前数据库中删除一个或多个用户自定义函数，语法形式如下：

Drop Function function\_name \[,…n\]

## 8.5 批处理和流程控制语句

### 8.5.1 批处理和注释

1.批处理

在程序中，我们可以使用GO语句将多条SQL语句进行分隔，两个GO之间的SQL语句可作为一个批处理。在一个批处理中可以包含一条或多条T-SQL语句，成为一个语句组。批处理中的语句是同时从应用程序发送到 SQL Server服务器执行，加快了执行速度。在书写批处理语句时，需要使用GO语句作为批处理命令的结束标志。

2. 注释

     注释是对程序的说明解释。一般使用注释对程序进行说明，例如变量的含义、程序的功能描述、基本思想等，增加程序的可读性。 SQL Server 2008提供了两类注释符。

--：单行注释，注释语句写在注释符--的后面，以最近的回车符作为注释的结束。 /\*…\*/：多行注释，“/\*”在开头，“\*/”在结尾。

### 8.5.2 流程控制语句

#### 1. Begin…End语句

Begin…End语句用于将多条Transact-SQL语句组成一个语句块，作为一个整体来执行。Begin…End语句的语法格式为：

Begin

  sql\_statement \| statement\_block

End

说明：

Begin…End语句块中至少要包含一条SQL语句。

Begin…End语句块常用在If条件语句和While循环语句中

Begin…End语句允许嵌套。

#### 2.IF…ELSE条件语句

v语法格式为：

IF Boolean\_expression

{sql\_statement\|statement\_block}

ELSE { sql\_statement\|statement\_block}

#### 3. Case分支语句

\(1\)简单Case表达式

简单Case表达式将一个测试表达式与一组简单表达式进行比较，如果**某个简单表达式与测试表达式的值相等，则返回相应结果表达式的值**。简单Case表达式的语法格式如下：

Case input\_expression

When when\_expression Then result\_expression

\[ ...n \]

\[ Else else\_result\_expression \]

End

例：

CASE sex WHEN 1 THEN '男' WHEN 2 THEN '女' ELSE '未知' END

\(2\)搜索Case表达式

搜索Case表达式中，各个When子句后都是布尔表达式。搜索Case表达式的语法格式如下：

Case

When Boolean\_expression Then result\_expression

\[ ...n \]

\[ Else else\_result\_expression \]

End

例：CASE WHEN sex=1 THEN '男' WHEN sex=2 THEN '女' ELSE '其他' END

#### 4. While循环语句

While语句用来实现重复执行语句或语句块，当指定的条件为真时，重复执行循环语句。具体的语法格式如下：

While Boolean\_expression

{sql\_statement \| statement\_block}

\[ Break\]

{sql\_statement \| statement\_block}

\[ Continue\] {sql\_statement \| statement\_block}

#### 5. Goto语句

   Goto语句用于实现程序跳转，作用是跳过Goto语句后面的SQL语句，并从标号所定义的位置处继续执行。

语法格式如下：

   GOTO  label

说明：

   label为标号，标号的定义部分由标识符和冒号“：”组成，而GOTO后面的标号只写标示符。

#### 6. Return语句

无条件终止批处理、存储过程或查询的执行，Return之后的语句是不执行的。具体的语法格式为：

Return  \[ integer\_expression \]

其中，integer\_expression表示返回的整数值。返回值是可以省略的，这时系统将根据程序的执行情况返回一个整数，其中0表示执行成功，非0值则表示失败。

#### 8.屏幕输出语句Print

    在程序运行过程中经常需要向屏幕输出一些中间结果或最后的结果值，Print语句用于实现这一功能，向屏幕输出信息，其语法格式为：

    Print  msg\_str \| @local\_variable \| string\_exprv

参数说明如下。

* msg\_str：要输出的字符串，用单引号括起来。
* @local\_variable：任何有效的字符数据类型的局部变量。
* string\_expr：输出的字符串的表达式。可由字符串连接运算符连接的常量、函数和变量等组成。

