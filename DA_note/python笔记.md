# python（pandas库）

jupyter notebook ：ctrl+shift+k快捷键打开目录

代码自动补全：新版本不需要下载插件 settings--code completion--enable autocompletion

python常用的4个数据类型

1、数字Numbers：int型、float型、bool型

2、字符串String

3、列表List

4、字典Dictionary

> 字典嵌套列表：
>
> {
>     'id': [53766889040208384, 53783364377004544, 53783376819848704, 53788075865425920, 53811230195500032],
>     'progress': [2691, 9193, 23017, 31033, 68093],
>     'content': ['这熟悉的bgm让人灵魂颤抖，向敌军进攻，冲啊！', 'DNA动了', '大制作啊', '好家伙 我直接好家伙', '大制作'],
>     'ctime': [1629643402, 1629674826, 1629674850, 1629683812, 1629727976],
>     'uid': ['4658843910516903647', '-5089316151886879487', '-5089316151886879487', '-6460990597571269883', '8879111121911466217']
> }

> 列表嵌套字典：
>
> [
>     {'53766889040208384', 2691, '这熟悉的bgm让人灵魂颤抖，向敌军进攻，冲啊！', 1629643402, '4658843910516903647'},
>     {'53783364377004544', 9193, 'DNA动了', 1629674826, '-5089316151886879487'},
>     {'53783376819848704', 23017, '大制作啊', 1629674850, '-5089316151886879487'},
>     {'53788075865425920', 31033, '好家伙 我直接好家伙', 1629683812, '-6460990597571269883'}
> ]无column名和index名
>
> [
>      {'id': 984675298218860544, 'content': '异常额外的启发', '视频内时间': 757.529},
>      {'id': 55324785558898688, 'content': '14个人', '视频内时间': 807.432},
>      {'id': 1092208119322427136, 'content': '及时发现异常，找到数据之间的因果关系', '视频内时间': 622.086},
>      {'id': 1067411585695350784, 'content': 'good', '视频内时间': 1308.365},
>      {'id': 1023427862298357248, 'content': '就是需求', '视频内时间': 1295.049},
>      {'id': 1028925510874641920, 'content': '拆眼有有效期的', '视频内时间': 1738.476},
>      {'id': 1099897195806761984, 'content': '基于数据反馈不断迭代业务和策略', '视频内时间': 1274.392},
>     {'id': 1050008151560063488, 'content': '计划', '视频内时间': 1587.059}
> ]有column名

## pd.Series

它可以看作是一个一维的带标签数组。与 Python 中的列表（`list`）相比，`Series`多了一个索引（`index`）机制。

### 创建series对象

#### 1、**从列表创建**：

> import pandas as pd
> data = [1, 3, 5, 7, 9]
> series = pd.Series(data)

此时，`Series`的索引默认是从 0 开始的整数序列，与列表的索引方式类似。也可以自定义索引，例如：

> index = ['a', 'b', 'c', 'd', 'e']
> series = pd.Series(data, index = index)

#### 2、**从字典创建**：

利用字典来创建`Series`是很方便的方式。字典的键会成为`Series`的索引，字典的值会成为`Series`对应索引位置的值。例如：

> data_dict = {'apple': 0.5, 'banana': 0.75, 'cherry': 0.6}
> series = pd.Series(data_dict)

这里创建的`Series`索引是`['apple', 'banana', 'cherry']`，值分别是`0.5`、`0.75`和`0.6`

#### 3、**通过标量值创建**：

还可以创建一个所有值都相同的`Series`。例如，创建一个长度为 3，所有值都是 5 的`Series`：

> series = pd.Series(5, index = ['x', 'y', 'z'])

### **基本属性和操作**

#### 1、**索引和值的访问**：

1可以通过索引来访问Series中的值。例如，对于前面创建的series（索引为['a', 'b', 'c', 'd', 'e']），可以使用series['a']来访问索引为a的值。也可以使用整数索引，如series[0]（在索引是默认从 0 开始的整数序列时）。
2通过切片操作可以获取部分数据。例如，series[1:3]会获取索引从 1 到 2（不包括 3）的值。

#### 2、**`name`属性**：

可以为`Series`的索引和值分别设置名称。例如：

> series.name = 'Fruit Prices'
> series.index.name = 'Fruit Names'

这样在打印`Series`或者进行一些数据操作时，这些名称可以提供更好的可读性。

#### 3、**基本运算**：

Series支持各种数学运算。例如，如果有两个Series对象series1和series2，它们的索引相同，那么可以直接进行加法运算series1 + series2，pandas会自动将对应索引位置的值相加。如果索引不同，pandas会根据索引的交集来进行运算，并将缺失的部分用NaN（Not a Number）填充。

DataFrame的每一列本质上都是一个Series。例如，在一个DataFrame中，可以通过df['column_name']的方式获取到对应的Series，其中df是一个DataFrame，column_name是列名。
可以使用Series来构建DataFrame。例如，有多个Series对象，将它们组合起来就可以形成一个DataFrame。



## pd.DataFrame

用于以表格形式和处理数据，类似提供电子表格或数据库表格。

表格形式：DataFrame是一个二维表格，其中包含了多行和多列的数据。每个列可以有不同的数据类型，例如整数、浮点数、字符串等。

标签：DataFrame的行和列都有标签（Label），行标签称为索引（Index），列标签通常是字段名或特征名。

数据操作：DataFrame提供了丰富的数据操作方法，包括数据筛选、切片、合并、分组、聚合、排序等。

数据查看：您可以使用.head()方法来查看DataFrame的前几行数据，以了解数据的结构和内容。

数据统计：DataFrame提供了.describe()方法，用于生成数据的统计摘要信息，包括均值、标准差、简单、顶点等。

数据过滤：你可以使用条件表达式来过滤数据，例如选择满足特定条件的行。

数据可视化：Pandas 与其他数据可视化库（如 Matplotlib 和 Seaborn）结合使用，可以轻松创建各种图表和可视化，以探索和传输数据。

数据导入和导出：DataFrame可以从各种数据源导入数据，如CSV文件、Excel表格、SQL数据库等，并且可以将数据导出为不同格式的文件。

数据恢复处理：DataFrame提供了处理数据中的恢复值的方法，如删除恢复值或恢复恢复值。

数据索引：DataFrame可以使用行索引和列标签来访问特定的数据元素。

数据转换：可以对DataFrame进行各种数据转换操作，如数据类型转换、列重命名、数据透视表等。

### 具体代码操作

#### 1、**从创建列表数据框：**

```python
import pandas as pd
data = [['Alice', 25], ['Bob', 30], ['Charlie', 35]]
df = pd.DataFrame(data, columns=['Name', 'Age'])
print(df)

'''
      Name  Age
0    Alice   25
1      Bob   30
2  Charlie   35
'''
```

#### 2、**从字典创建数据框：**

```python
import pandas as pd
data = {'Name': ['Alice', 'Bob', 'Charlie'], 'Age': [25, 30, 35]}
df = pd.DataFrame(data)
print(df)
df.to_csv('test.csv', encoding='utf-8-sig')
 
# 运行结果
'''
      Name  Age
0    Alice   25
1      Bob   30
2  Charlie   35
'''
```

#### 3、**访问数据**：

```python
import pandas as pd
data = {'Name': ['Alice', 'Bob', 'Charlie'], 'Age': [25, 30, 35]}
df = pd.DataFrame(data)
a = df['Name']  # 获取 'Name' 列的数据
b = df.loc[0]    # 获取第一行的数据
print(a)
print(b)
# 运行结果
'''
0      Alice
1        Bob
2    Charlie
Name: Name, dtype: object
Name    Alice
Age        25
Name: 0, dtype: object
'''
```

#### 4、**数据操作：**

```python
import pandas as pd
data = {'Name': ['Alice', 'Bob', 'Charlie'], 'Age': [25, 30, 35]}
df = pd.DataFrame(data)
a = df['Age'].mean()  # 计算 'Age' 列的平均值
b = df.sort_values(by='Age', ascending=False)  # 按 'Age' 列排序,ascending=True是从小到大，ascending=False是从大到小
print(a)
print(b)
# 运行结果
'''
30.0
      Name  Age
2  Charlie   35
1      Bob   30
0    Alice   25
'''
```

#### 5、**数据查看：**

```python
import pandas as pd
data = {'Name': ['Alice', 'Bob', 'Charlie'], 'Age': [25, 30, 35]}
df = pd.DataFrame(data)
a = df.head(2)     # 查看前几行数据，df.head()默认为前5行
b = df.tail(2)    # 查看后2行数据
print(a)
print(b)
# 运行结果
'''
    Name  Age
0  Alice   25
1    Bob   30
      Name  Age
1      Bob   30
2  Charlie   35
'''
```

#### 6、**数据统计：**

``` python
import pandas as pd
data = {'Name': ['Alice', 'Bob', 'Charlie'], 'Age': [25, 30, 35]}
df = pd.DataFrame(data)
c=df.describe()  # 生成数据的统计摘要信
print(c)
# 运行结果
'''
Age
count   3.0
mean   30.0
std     5.0
min    25.0
25%    27.5
50%    30.0
75%    32.5
max    35.0
'''
```

### pandas生成csv模板

```python
import pandas as pd
 
# 二维数组
datalist = [['小明',15,'二班','班长'],['小红',16,"三班",'班长'],['小刚',14,"二班",',混子']]
pd.DataFrame(datalist,columns=['姓名','年龄','班级','角色']).to_csv('data.csv',index=False,encoding='utf-8-sig',sep=",")
 
# 字典
datadic = {'姓名':['小明','小红','小刚'],'年龄':[15,16,14],'班级':['二班','三班','二班'],'角色':['班长','班长','混子']}
pd.DataFrame(datadic).to_csv('data2.csv',index=False,encoding='utf-8-sig',sep=",")
 
 
li = [
{'id': 1, 'hobby': "吃惠灵顿,羊排"},
{'id': 2, 'hobby': "吃牛排"},
{'id': 3, 'hobby': "吃猪排"},
]
datalist = [l.values() for l in li]
pd.DataFrame(datalist,columns=['id','hobby']).to_csv('data3.csv',index=False,encoding='utf-8-sig',sep=",")
 
```

分页存储数据

```python
import pandas as pd
 
# 采集数据分页的逻辑
for page in range(3):
    # 采集一页数据，假设得到的数据存储在 datalist 变量中
    datalist = [['小明', 15, '二班', '班长'], ['小红', 16, "三班", '班长'], ['小刚', 14, "二班", ',混子']]
    df = pd.DataFrame(datalist, columns=['姓名', '年龄', '班级', '角色'])
    if page == 0:
        df.to_csv('data.csv', index=False, encoding='utf-8-sig', sep=",")
    else:
        df.to_csv('data.csv', mode='a', header=False, index=False, encoding='utf-8-sig', sep=",")
```

### pandas与sql交互

```python
import sqlalchemy
import pandas as pd
import sqlite3
 
# 1.创建连接
engine = sqlalchemy.create_engine('mysql+pymysql://root:root@localhost:3306/yzznewdb')
# 2.创建表
engine.execute('CREATE TABLE IF NOT EXISTS testtext(id INT PRIMARY KEY AUTO_INCREMENT NOT NULL ,name TEXT,COMMENT TEXT );')
# 3.插入数据
data = {'name':['张三','李四','王五','赵六'],'comment':['我是张三','我是李四','我是王五','我是赵六']}
pd.DataFrame(data).to_sql('testtext',con=engine,index=False,if_exists='append')
 
```

### Pandas中map、apply、transform函数

#### 1、map函数

针对series中的每个元素

在pandas中，map()函数可以用于根据字典的映射关系转换数据。map()函数是用于Series对象的方法，他将Series中的每个元素根据字典的映射进行转换。

输出是对应的列数据（所有列中单元格即列）

```python
import pandas as pd
data = pd. DataFrame (
    {"name": ['Jack', 'Alice', 'Lily', 'Mshis', 'Gdli' , 'Agosh', 'Filu', 'Mack', 'Lucy', 'Pony' ],
    "gender": ['F', 'M', 'F', 'F', 'M', 'F', 'M', 'M', 'F', 'F'],
    "age":[25, 34, 49, 42, 28, 23, 45, 21, 34, 29]}
)
data.gender=data.gender.map({"F":'男','M':'女'})
print(data)
```

 map也可以传替函数：

```python
import pandas as pd
import numpy as np
 
data=pd.DataFrame(np.random.randint(0,100,size=(100,1)),#size参数表明几行几列故为100行1列
                    columns=["python"]                
                  )
print(data)
def change(x):
    if x<60:
        return '不及格'
    elif x<80:
        return '中等'
    elif x<90:
        return '良好'
    else:
        return '优秀'
s=data.python.map(change)
data['等级']=s
print(data)
```

#### 2、apply函数

针对axis的整行或整列数据

apply函数是用于DataFrame的方法，它可以用于数据转换和处理。其接受一个函数作为参数，并将参数应用到DataFrame中的每一行或每一列。[相当于是通过axis来指定方向即行和列，axis=0是自上而下即列，axis=1是自左向右即行]

既支持Series也支持DataFrame。

```python
import pandas as pd
import numpy as np
 
df=pd.DataFrame(np.random.randint(0,100,size=(100,3)),
                columns=['math','python','en']
                )
def cover(x):
    return x.mean().round(2),x.count(),x.median()
 
print(df.apply(cover,axis=0))
print('---------')
print(df.apply(cover,axis=1))
```

再计算每一行的和

```python
def sum(row):
    return row['math']+row['python']+row['en']
 
df['sum']=df.apply(sum,axis=1)#axis=0会报错
print(df)
```

#### 3、transfrom函数

axis轴向默认为0

针对groupby后的组进行聚合运算，随后填充到每行或列数据中

在pandas中，transform函数用来对数据进行转换（允许对每个组的值进行操作），并返回与输入相同形状的结果，使得转换后的结果与原始数据保持对应的关系。

`transform`通常与`groupby`一起使用，允许你对每一组的数据进行操作。最常见的用途是计算每个组的某种统计量，并在原始数据框中返回相同的长度。

```python
import pandas as pd  
 
# 创建示例数据  
data = {  
    '部门': ['销售', '销售', '市场', '市场', '研发', '研发'],  
    '工资': [5000, 6000, 4500, 7000, 8000, 9000]  
}  
df = pd.DataFrame(data)  
 
# 使用groupby和transform计算每个部门工资的标准化  
df['工资标准化'] = df.groupby('部门')['工资'].transform(lambda x: (x - x.mean()) / x.std())  
 
print(df)
```

