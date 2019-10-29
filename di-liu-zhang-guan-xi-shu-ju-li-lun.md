# 第六章 关系数据理论

## 6.1 问题的提出

### 一、概念回顾

关系

关系模式

关系数据库

关系数据库的模式

### 二、关系模式的形式化定义

关系模式由五部分组成，即它是一个五元组：

                    R\(U, D, DOM, F\)

R：      关系名

U：       组成该关系的属性名集合

D：       属性组U中属性所来自的域

DOM： 属性向域的映象集合

F：       属性间数据的依赖关系集合

三、什么是数据依赖

四、关系模式的简化定义 

五、数据依赖对关系模式影响

## 6.2 规范化

#### 6.2.1  函数依赖

#### 6.2.2  码

定义6.4  设K为R&lt;U,F&gt;中的属性或属性组合。若K      U，  则K称为R的侯选码（Candidate Key）。

     若候选码多于一个，则选定其中的一个做为主码（Primary Key）。

超键码：包含键码的属性集，每个候选码都是超键码，但是某些超键码不是候选码。

主属性与非主属性  

* 包含在任何一个候选码中的属性 ，称为主属性（Prime attribute）
*  不包含在任何码中的属性称为非主属性（Nonprime attribute）或非码属性（Non-key attribute）

全码 整个属性组是码，称为全码（All-key）

#### 6.2.3  范式

v范式是符合某一种级别的关系模式的集合v

关系数据库中的关系必须满足一定的要求。满足不同程度要求的为不同范式v

范式的种类： 

* 第一范式\(1NF\)
* 第二范式\(2NF\)
* 第三范式\(3NF\)
* BC范式\(BCNF\)
* 第四范式\(4NF\)   
* 第五范式\(5NF\)

6.2.4  2NF

6.2.5  3NF

6.2.6  BCNF 6.2.7  规范化小结

## 6.3 数据依赖的公理系统

逻辑蕴含

> 定义6.11  
>
> 对于满足一组函数依赖 F 的关系模式R &lt;U，F&gt;，其任何一个关系r，若函数依赖X→Y都成立, （即r中任意两元组t，s，若t\[X］=s\[X］，则t\[Y］=s\[Y］），则称F逻辑蕴含X →Y

###   Armstrong公理系统

关系模式R 来说有以下的推理规则：

1.  A1.自反律（Reflexivity）：若Y &lt;= X &lt;= U，则X →Y为F所蕴含。 
2. A2.增广律（Augmentation）：若X→Y为F所蕴含，且Z &lt;= U，则XZ→YZ为F所蕴含。 
3. A3.传递律（Transitivity）：若X→Y及Y→Z为F所蕴含，则X→Z为F所蕴含。

### 导出规则

\(1\) 根据A1，A2，A3这三条推理规则可以得到下面三条推理规则：

        § 合并规则：由X→Y，X→Z，有X→YZ。（A2， A3）

        § 伪传递规则：由X→Y，WY→Z，有XW→Z。（A2， A3）

        § 分解规则：由X→Y及 Z&lt;=Y，有X→Z。（A1， A3）

\(2\) 根据合并规则和分解规则，可得引理6.1

   引理6.l  X→A1 A2…Ak成立的充分必要条件是X→Ai成立（i=l，2，…，k）

### 函数依赖闭包

定义6.l2    在关系模式R&lt;U，F&gt;中为F所逻辑蕴含的函数依赖的全体叫作 F的闭包，记为F+。

定义6.13   设F为属性集U上的一组函数依赖，X ÍU， XF+ ={ A\|X→A能由F 根据Armstrong公理导出}，XF+称为属性集X关于函数依赖集F 的闭包

引理6.2 

   设F为属性集U上的一组函数依赖，X，Y &lt;=U，X→Y能

   由F 根据Armstrong公理导出的充分必要条件是Y &lt;=XF+

用途

    将判定X→Y是否能由F根据Armstrong公理导出的问题，转化为求出XF+ 、判定Y是否为XF+的子集的问题

## 6.4 模式的分解

## 6.5 小结
