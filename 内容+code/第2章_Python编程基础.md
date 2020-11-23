**<center>Python编程基础</center>**



# Python语言基本语法


```python
#例2-1 Python程序实例
answer=int(input("请输入一个整数："))
if answer==2 :
    print("hello,")
    print("it's True.")
else:
    print("sorry,")
    print("it's False.")
```

    请输入一个整数：3
    sorry,
    it's False.


### 变量和赋值  
 - python中变量不需要声明数据类型，“类型”是指被赋值对象的类型。同一变量可以反复赋值且可是不同类型


```python
#例2-2 多变量赋值实例
brower, count, addsum = 'Google', 100, 123.45
print("brower, count, addsum")
```

    brower, count, addsum


- 字符串被定义为引号之间的字符集合
- 一个反斜杠表示一个转义序列的开始，即某些字符串中存在的特殊含义字符。  
|转义字符|说明|
|:--|:--:|
|\n|换行|
|\\|反斜杠|
|\"|双引号|
|\t|制表符|
- 用r+" "的方式，表示" "内部的字符串，默认不转义


```python
#例2-3 转义字符的使用
print("python\nprogram")
print(r"python\nprogram")
```

    python
    program
    python\nprogram


## 流程控制
### 条件语句


```python
#例2-4 判断一个学生的成绩是否及格：如果成绩大于或等于60分，则打印“及格”，否则输出“不及格”。
score=float(input("请输入成绩："))
if score >=60:
    print("及格。")
else:
    print("不及格。")
```

    请输入成绩：99
    及格。



```python
#例2-5 将化学分子式翻译为其所表示物质对应的英文
compound=input("请输入化学分子式：")
if compound == "H2O":
    print("water.")
elif compound == "NH3":
    print("ammonia.")
elif compound =="CH3":
    print("methane.")
else:
    print("no exist.")
```

    请输入化学分子式：N2
    no exist.


### 循环语句


```python
#例2-6 求1+2+3+4+5的值
sum=0
i=1
while i<6:
    sum=sum+i
    i=i+1
print("sum is %d."%sum)
```

    sum is 15.



```python
#例2-7 输出20以内能被3整除的数
i=0
while i<20:
    if i%3 == 0:
        print(i,end=" ")  #输出以空格结束，不换行
    i=i+1
```

    0 3 6 9 12 15 18 


```python
#例2-8 求1+2+3+4+5的值
sum=0
for i in range(1,6):  # range(start,end,step)：返回列表，从start开始，不到end，步长step缺省则为1
    sum=sum+i
print("1+2+3+4+5= %d."%sum)
```

    1+2+3+4+5= 15.



```python
#例2-9 将下面数组中的奇数变成它的平方，偶数保持不变
x=[1,2,3,4,8,7,22,33,88]
print("原数据：",x)
for i in range(len(x)):
    if(x[i]%2)!=0 :
        x[i]=x[i]*x[i]
print("处理后：",x)
```

    原数据： [1, 2, 3, 4, 8, 7, 22, 33, 88]
    处理后： [1, 2, 9, 4, 8, 49, 22, 1089, 88]


# 内置数据类型
   ## 列表


```python
#例2-10 在列表中追加元素。
list=['a','b','c','d']
list.append('Baidu')
print("更新后的列表：",list)
```

    更新后的列表： ['a', 'b', 'c', 'd', 'Baidu']



```python
#例2-11 在列表中插入元素。
list=['a','b','c','d']
list.insert(1,'Baidu')
print("列表插入元素后为：",list)
```

    列表插入元素后为： ['a', 'Baidu', 'b', 'c', 'd']



```python
#例2-12 返回第一个指定值的索引。
list=['a','b','c','d']
print("b的索引值为：",list.index('b'))
```

    b的索引值为： 1



```python
#例2-13 删除列表中第一次找到的数值
list=['a','b','c','d']
list.remove('d')
print("列表现在为：",list)
```

    列表现在为： ['a', 'b', 'c']



```python
#例2-14 删除列表中的指定元素
list=['a','b','c','d']
p=list.pop()
print("删除%r后的列表为 %r："%(p,list))
print("删除元素为：",list.pop(1))
```

    删除'd'后的列表为 ['a', 'b', 'c']：
    删除元素为： b



```python
#例2-15 列表元素倒排
list=['a','b','c','d']
list.reverse()
list
```




    ['d', 'c', 'b', 'a']




```python
#例2-16 返回列表中指定数组的出现次数
list=['a','b','c','d','a','b','c','a','c']
print("c的出现次数是：",list.count("c"))
print("list中一共有 %d 个a."%list.count("a"))
```

    c的出现次数是： 3
    list中一共有 3 个a.



```python
#例2-17 列表元素排序
list=['a','b','c','d']
list.sort(reverse=True)
list
```




    ['d', 'c', 'b', 'a']



## 列表推导式  
   根据确定的判定条件创建子序列
    语法格式为：  
    ```[ <expr1> for k in L if<expr2> ]```  
    语义：  
    ```
    list=[]  
    for k in L:  
      if<expr2>:  
         list.append(<expr1>)  
    return list;
    ```


```python
#例2-18 列表推导式实例
vec=[2,4,6,8,10]
print([3*x for x in vec])
vec=[2,4,6,8,10]
print([3*x for x in vec if x>6])
vec1=[2,4,6]
vec2=[4,3,-9]
print([x*y for x in vec1 for y in vec2 if x*y>0])
```

    [6, 12, 18, 24, 30]
    [24, 30]
    [8, 6, 16, 12, 24, 18]



```python
#例2-19 随机生成30个整数构成列表，并计算列表均值，然后使用列表中每个元素逐个减去均值所得的数值重新构建列表
import random
total=[]
for i in range(30):  # 随机生成30个1-150之间的整数
    total.append(random.randint(1,150))
print("列表：",total)
sum=0
for item in total:
    sum=sum+item  # 列表元素求和
total_m=sum//len(total)  # 计算列表均值。//表示取整。
print("新列表：",[x - total_m for x in total])
```

    列表： [18, 32, 138, 123, 6, 29, 45, 26, 117, 106, 98, 3, 134, 97, 60, 19, 78, 54, 78, 111, 70, 58, 26, 124, 77, 78, 138, 9, 16, 143]
    新列表： [-52, -38, 68, 53, -64, -41, -25, -44, 47, 36, 28, -67, 64, 27, -10, -51, 8, -16, 8, 41, 0, -12, -44, 54, 7, 8, 68, -61, -54, 73]



```python
#例2-20 计算矩阵的转置矩阵
matrix=[[1,2,3,4],[5,6,7,8],[9,10,11,12]]
print("原矩阵：",matrix)
# row[i]以行的形式在matrix中循环，i表示每一列的下标。将同一列不同行的元素加入一个列表中，完成转置
print("转置矩阵为：",[[row[i] for row in matrix] for i in range(4)])  
```

    原矩阵： [[1, 2, 3, 4], [5, 6, 7, 8], [9, 10, 11, 12]]
    转置矩阵为： [[1, 5, 9], [2, 6, 10], [3, 7, 11], [4, 8, 12]]


## 元组  
   元组是固定长度、不可变的对象序列，用圆括号。


```python
#例2-21 元组的创建。
tup=tuple('bar')  # 创建元组
print("输出元组tup：",tup)  # 输出元组
nested_tup=(4,5,6),(7,8)
print("输出元组tup：",nested_tup)  # 输出元素是元组的元组
print("元组的连接：",tup+tuple('wwy'))
a,b,c=tup  # 因为只有3个变量被赋值，所以只有元组的前3个元素被赋值出去
print(a,b,c)
print(tup.count(a))  # 统计某个数值在元组中出现的次数
```

    输出元组tup： ('b', 'a', 'r')
    输出元组tup： ((4, 5, 6), (7, 8))
    元组的连接： ('b', 'a', 'r', 'w', 'w', 'y')
    b a r
    1


## 字典  
   - 由键值对组成的非排序可变集合体。键是唯一的，且必须使用不可变对象（例如字符串）。  

    字典的常用方法及描述：  
|方法|描述|
|:--|:--:|
|dic.get(key,default=None|返回指定键的值，若值不在字典中则返回default|
|dic.items()|以列表返回可遍历的（键，值）元组数组|
|dic.keys()|以列表返回一个字典中的所有的键|
|dic.values()|以列表返回字典中的所有值|


```python
#例2-22 字典应用示例
scientists={'Newton':1642,'Darwin':1809,'Turing':1912}
print('keys：',scientists.keys())  # 返回字典中的所有键
print('values：',scientists.values())  # 返回字典中的所有值
print('items：',scientists.items())  # 返回字典中的所有键值对，形式（键，值）
print('get：',scientists.get('Curie',1867))  # get方法
temp={'Curie':1867,'Hopper':1906,'Franklin':1920}
scientists.update(temp)  # 用字典temp更新字典scientists
print('after update：',scientists)
scientists.clear()  # 清空字典
print("after clear：",scientists)
```

    keys： dict_keys(['Newton', 'Darwin', 'Turing'])
    values： dict_values([1642, 1809, 1912])
    items： dict_items([('Newton', 1642), ('Darwin', 1809), ('Turing', 1912)])
    get： 1867
    after update： {'Newton': 1642, 'Darwin': 1809, 'Turing': 1912, 'Curie': 1867, 'Hopper': 1906, 'Franklin': 1920}
    after clear： {}


## 集合
- 由唯一元素组成的非排序集合体，即元素没有特定顺序且不重复。用set()创建空集合。  
    集合的常用方法及其描述：  
|方法|描述|
|:--|:--:|
|issubset()|判断调用函数的集合的所有元素是否都包含在指定集合(方法的参数)中|
|union()|返回两个集合的并集，即包含了所有集合的元素，重复的元素只会出现一次。|
|difference()|返回的集合元素包含在第一个集合(调用函数的集合)中，但不包含在第二个集合(方法的参数)中|


```python
#例2-23 集合用法示例
set1=set([0,1,2,3,4])
set2=set([1,3,5,7,9])
print(set1.issubset(set2))  # 判断set1中的所有元素是否都在set2中
print(set1.union(set2))  #返回两个集合的并集，重复的元素只出现一次
print(set2.difference(set1))  # 返回的集合元素包含在set1中，但不包含在set2中
print(set2.issubset(set1))
```

    False
    {0, 1, 2, 3, 4, 5, 7, 9}
    {9, 5, 7}
    False


# 函数  
函数是对程序逻辑进行过程化和结构化的一种方法，增强代码的重用性和可读性。  
函数的定义的格式：  
    ```def function_name(arguments):
        function_block```


```python
#例2-24
def week():
#提供正确的正常上班时间
    while True:
        try:
            x=int(input("请输入本周常规工作实践（小时）："))
            if x<0 :
                print("一周常规工作实践不能小于0小时！")
            elif x>40 :
                print("一周常规工作时间不能超过40小时！")
            else:
                break
        except ValueError:
            print("输入错误！请输入整数！")
    return x

def extrawork():
#提供正确的加班时间
    while True:
        try:
            x=int(input("请输入本周加班时间（小时）："))
            if x<0 :
                print("一周的加班时间不能小于0小时！")
            else:
                break
        except ValueError:
            print("输入错误！请输入整数！")
    return x

def salary():
#提供正确的时薪
    while True:
        try:
            x=float(input("请输入您的时薪："))
            if x<0 :
                print("您的时薪不能小于0！")
            else:
                break
        except ValueError:
            print("输入错误！请输入整数！")
    return x

if __name__=="__main__":
    print("-"*30)
    week=week()
    print("您的常规上班时间的：%d小时"%week)
    extrawork=extrawork()
    print("您的加班时间是：%d小时"%extrawork)
    salary=salary()
    print("您的时薪是：%.2f元/小时"%salary)
    print("-"*30)
    print("您的周工资是：%.2f元"%((week*salary)+(extrawork*1.5*salary)))
```

    ------------------------------
    请输入本周常规工作实践（小时）：4
    您的常规上班时间的：4小时
    请输入本周加班时间（小时）：5
    您的加班时间是：5小时
    请输入您的时薪：10
    您的时薪是：10.00元/小时
    ------------------------------
    您的周工资是：115.00元


## lambda函数
   lambda只是一个表达式，用来创建匿名函数。lambda表达式中只能封装有限的逻辑，有自己的命名空间，不能访问有参数列表之外或全局命名空间中的参数。


```python
#例2-25 编写函数实现计算多项式1+2x+y^2+zy的值，可以简单地定义一个lambda函数来完成这个功能。
polynominal=lambda x,y,z:1+2*x+y**2+z*y
polynominal(1,2,3)
```




    13



# 文件操作
## 文件处理过程
- 打开文件：open()函数
- 读取、写入文件：read()、readline()、readlines()、write()等。
- 对读取道德数据进行处理。
- 关闭文件：close()。  

**读完文件之后，一定要关闭，否则文件一直被python占用。**可使用`with open() as f`语句，操作后自动关闭文件。

## 数据的读取方法
|方法|描述|
|:--|:--:|
|read([size])|读取文件所以内容，返回字符串类型，size表示读取的数量，以byte为单位，可省略|
|readline([size])|读取文件一行的内容，以字符串的形式返回，size表示读取一行的一部分|
|readlines([size])|读取所有的行到列表里面[line1,line2,...,linen]（文件每一行是list的一个 成员），size表示读取内容的总长|

readlines()读取后得到的是每行数据组成的列表，即一行数据存储为一个字符串，换行符也未去掉。


```python
#例2-26 读取txt文件
file=open("D://My_Project//PythonProject//2020-2021第一学期_数据可视化_杨波//data//泰戈尔的诗.txt",mode='r')
content=file.read()
print(content)
file.close()
```

    #有这样一位诗人。他的诗，让世界上无数人，走出消极，甚至治愈了一个民族的痛。
    #他就是，诗人之王泰戈尔。
    #下面是泰戈尔的一些经典语录
    1. 生如夏花之绚烂，死如秋叶之静美。
    2. 眼睛为她下着雨，心却为她打着伞，这就是爱情。
    -
    3. 世界以痛吻我，要我报之以歌。
    ###
    5. 当你为错过太阳而哭泣的时候，你也要再错过群星了。
    6. 我们把世界看错，反说它欺骗了我们。
    -
    7. 不要着急，最好的总会在最不经意的时候出现。



```python
#例2-27 读取txt文件时指定读取数量
f=open("D://My_Project//PythonProject//2020-2021第一学期_数据可视化_杨波//data//泰戈尔的诗.txt",mode='r')
content=f.read(10)  # 读取10byte
print(content)
f.close()
print(type(content))
```

    #有这样一位诗人。他
    <class 'str'>



```python
#例2-28 按行读取txt文件
f=open("D://My_Project//PythonProject//2020-2021第一学期_数据可视化_杨波//data//泰戈尔的诗.txt",mode='r')
content=f.readlines()
print(content)
f.close()
```

    ['#有这样一位诗人。他的诗，让世界上无数人，走出消极，甚至治愈了一个民族的痛。\n', '#他就是，诗人之王泰戈尔。\n', '#下面是泰戈尔的一些经典语录\n', '1. 生如夏花之绚烂，死如秋叶之静美。\n', '2. 眼睛为她下着雨，心却为她打着伞，这就是爱情。\n', '-\n', '3. 世界以痛吻我，要我报之以歌。\n', '###\n', '5. 当你为错过太阳而哭泣的时候，你也要再错过群星了。\n', '6. 我们把世界看错，反说它欺骗了我们。\n', '-\n', '7. 不要着急，最好的总会在最不经意的时候出现。']


## 读取CSV文件
CSV文件也称为字符分割值文件。`import csv`之后即可读取CSV文件。  
其特点：
- 纯文本，使用某个字符集。
- 以行为单位读取数据，每行一条记录。
- 每条记录被分隔符分割成字段。
- 每条记录都要同样的字段序列。


```python
#例2-29 读取CSV文件
import csv
with open("D://My_Project//PythonProject//2020-2021第一学期_数据可视化_杨波//data//student.csv",'r') as f:
    reader=csv.reader(f)
    rows=[row for row in reader]
for item in rows:
    print(item)
```

    ['No.', 'Name', 'Age', 'Score']
    ['1', 'mayi', '18', '99']
    ['2', 'jack', '21', '89']
    ['3', 'tom', '25', '95']
    ['4', 'rain', '19', '80']


## 文件写入与关闭
### 文件写入
需要将open()函数中文件打开的参数设置为mode=w。 

|方法|描述|
|:--|:--:|
|write()|逐次写入|
|writelines()|将列表中的所有数据一次性写入文件|

若有换行需要，需在每条数据后增加换行符，同时使用`字符串.join()`将变量数据联合成一个字符串，同时增加间隔符"\t"。  
还可调用writerow函数将列表中的每个元素逐行写入文件。


```python
#2-30 文件写入
import csv
content=[['0','hanmeimei','23','81'],['1','mayi','18','99'],['2','jack','21','89']]
f=open("test.csv","w",encoding='gbk',newline='')  # 如果不加newline=''，会出现空行
content_out=csv.writer(f)
for con in content:
    content_out.writerow(con)
#f.flush()
f.close()  # 若不用close()和flush()的话，读取的数据还在缓存区，没有写入文件。
```


```python
#例2-31 跳过文件中的注释内容和缺失值
def skip_header(f):
    line=f.readline()
    while line.startswith('#'):
        line=f.readline()
    return line
def process_file(f):
    #调用函数使其跳过文件头部
    line=skip_header(f).strip()
    print(line)  # 打印第一行有用数据
    for line in f:
        if line.startswith("-") or line.startswith("#"):  # 处理缺失值
            line=f.readline()
        line=line.strip()
        print(line)
if __name__=="__main__":
    input_file=open("D://My_Project//PythonProject//2020-2021第一学期_数据可视化_杨波//data//泰戈尔的诗.txt",'r')
    process_file(input_file)
    input_file.close()
```

    1. 生如夏花之绚烂，死如秋叶之静美。
    2. 眼睛为她下着雨，心却为她打着伞，这就是爱情。
    3. 世界以痛吻我，要我报之以歌。
    5. 当你为错过太阳而哭泣的时候，你也要再错过群星了。
    6. 我们把世界看错，反说它欺骗了我们。
    7. 不要着急，最好的总会在最不经意的时候出现。

