# SQL

* from--where--group by--having--order by--limit--select

## 基础语句

### 1、select from

select 字段名 from 表明

select  字段名 as 别名 from  表名 （as可以省略）

* 查询多列

select  字段名1,字段名2,字段名3 from  表名

* 查询所有列

select * from  表名

* 数据去重

select  distinct  字段名 from 表名

* select中的计算字段

select   字段名,计算字段 from  表名称 

### 2、where

select 字段名 from 表名 where 表达式

* 运算符查询语法

select 字段名 from 表名 where 字段名 运算符 值

* 模糊查询语法

select 字段名 from 表名 where 字段名 like '通配符+字符'

* 使用多条件查询

select 字段名 from 表名 where 条件代码1 and|or 条件代码2

* 运算符

=、>、<、>=、<=、<>/!=不等于

between and 

in、not in 

is null、is not null

逻辑运算符：and与、or或、not非

* 通配符

%表示任何字符出现任意次数

_表示任何字符出现一次

### 3、order by

select 字段名 from 表名 [where 表达式] [order by 字段名 asc|desc]

- order by 字段名 asc|desc  规定查询出的结果集显示的顺序
- order by核心子句是可选项，使用该子句是为了对被查询出的结果集，指定依据字段排序
- asc指定该字段升序排序，desc为降序排序，不写则默认为升序排序

### 4、limit

* 查询结果返回前n行

select 字段名 from 表名 [where 表达式] [order by 字段名 asc|desc] **[limit n]**

* 查询结果返回第x+1行开始的n行到x+n行

select 字段名 from 表名 [where 表达式] [order by 字段名 asc|desc] **[limit x,n]**

### 5、group by&聚合函数

select 字段名1,聚合函数(字段名) from 表名 [where 表达式] [group by 字段名1] [order by 字段名 asc|desc] [limit [位置偏移量,]行数]

* 聚合函数

AVG()、COUNT()、MAX()、MIN()、SUM()

### 6、having

select 字段名 from 表名 [where 表达式] [group by 字段名] [having 表达式] [order by 字段名 asc|desc] [limit [位置偏移量,]行数]

### 7、部分常见函数

* 数学函数

round(x,y)——四舍五入函数

* 字符串函数

concat(s1,s2,...)——连接字符串函数

replace(s,s1,s2)——替换函数

left(s,n)——从左截取字符串一部分的函数

right(s,n)——从右截取字符串一部分的函数

substring(s,n,len)——从指定位置截取字符串一部分的函数

* 数据类型转换函数

cast(x as type)——转换数据类型的函数

* 日期时间函数

year(date)——获取年的函数

month(date)——获取月的函数

day(date)——获取日的函数

date_add(date,interval expr type)——对指定起始时间进行加操作

> - select
> - whn 更新时间
> - ,date_add(whn,interval 2 day) 加2天
> - from covid
> - where recovered > 0

date_sub(date,interval expr type)——对指定起始时间进行减操作

datediff(date1,date2)——计算两个日期之间间隔的天数

date_format(date,format)——将日期和时间格式化

* 条件判断函数——根据满足不同条件，执行相应流程

if(expr,v1,v2)

case when

> - select
> - recovered 累计治愈人数
> - ,case when recovered = 1 then 'one' when recovered > 1 then 'more' else '0' end
> - from covid
> - where recovered > 0

## 高级语句

### 1、窗口函数

* 排序窗口函数语法

rank()over([partition by 字段名] order by 字段名 asc|desc)12345

dense_rank()over([partition by 字段名] order by 字段名 asc|desc)11345

row_number()over([partition by 字段名] order by 字段名 asc|desc)11223

> - select
> - yr
> - ,party
> - ,votes
> - ,rank()over(partition by yr order by votes desc) as posn
> - from ge
> - where constituency = 'S14000021'
> - order by party,yr

* 偏移分析函数语法

lag(字段名,偏移量[,默认值])over([partition by 字段名] order by 字段名 asc|desc)

lead(字段名,偏移量[,默认值])over([partition by 字段名] order by 字段名 asc|desc)

> - select
> - name
> - ,date_format(whn,'%Y-%m-%d') date
> - ,confirmed 当天截至时间累计确诊人数
> - ,lag(confirmed,1)over(partition by name order by whn) 昨天截至时间累计确诊人数
> - ,(confirmed - lag(confirmed,1)over(partition by name order by whn)) 每天新增确诊人数
> - from covid
> - where name in ('France','Germany') and month(whn) = 1
> - order by whn

### 2、表连接

* 内连接inner join语法（注意内连接inner可以省略，直接使用join默认为内连接）【只保留匹配上的数据行】

select 字段名 from 表名1 inner join 表名2 on 表名1.字段名 =  表名2.字段名

* 左连接left join语法【保留左表】

select 字段名 from 表名1 left join 表名2 on 表名1.字段名 =  表名2.字段名

* 右连接right join语法【保留右表】

select 字段名 from 表名1 right join 表名2 on 表名1.字段名 =  表名2.字段名

### 3、子查询

子查询本身是一个完整的查询，由括号包裹嵌套在主查询中

子查询最后返回查询出的结果给主查询

子查询可以在select，from，where，having子句（同where）中使用，但要注意不同子句能接受的子查询种类有差别

子查询可以多重嵌套（子查询可以作为主查询再嵌套子查询）

> - select name
>
> - from world
>
> - where gdp/population > (
>
>   - ​                                               select gdp/population
>
>   - ​                                               from world
>
>   - ​                                               where name='united kingdom'
>
>   - ​                                           )
>
> - and continent='europe'

> - select
>
> - name
>
> - ,population
>
> - from world
>
> - where population > (
>
>   - ​                                             select population
>
>   - ​                                             from world
>
>   - ​                                             where name= 'Canada'
>
>   - ​                                  )
>
> - and population < (
>
>   - ​                                       select population
>
>   - ​                                       from world
>
>   - ​                                       where name = 'Poland'
>
>   - ​                              )

> - select
>
> - name
>
> - ,continent
>
> - ,population
>
> - from world
>
> - where continent not in (
>
>   - ​                                           select distinct continent
>
>   - ​                                           from world
>
>   - ​                                           where population >25000000
>
>   - ​                                        )

> - select
>
> - continent
>
> - ,name
>
> - ,area
>
> - from world
>
> - where (continent,area) in  (
>
>   - ​                                                   select
>
>   - ​                                                   continent
>
>   - ​                                                  ,max(area)
>
>   - ​                                                   from world
>
>   - ​                                                   group by continent
>
>   - ​                                               )

> - select
>
> - name
>
> - ,日期
>
> - ,每天新增治愈人数
>
> - ,rank()over(partition by name order by 每天新增治愈人数 desc) 排名
>
> - from
>
> - (
>
>   - select
>
>   - name
>
>   - ,date_format(whn,'%Y年%m月%d日') 日期
>
>   - ,(recovered - lag(recovered,1)over(partition by name order by whn)) 每天新增治愈人数
>
>   - from covid
>
>   - where name in ('France','Italy')
>
> - ) re
>
> - order by 排名
