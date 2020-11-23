# 第5章 Pandas数据载入与预处理

# 数据载入  

## 读/写文本分析
1. 文本文件读取  
CSV是以逗号分隔的文件格式，分隔符可以不是逗号。  

在Pandas中用read_table函数读取文本文件：  
pandas.read_table(filepath_or_buffer, sep='\t', header='infer', name=None, index_col=None, dtype=None, engine=None, nrows=None)   
在Pandas中用read_csv函数读取文本文件：  
pandas.read_csv(filepath_or_buffer, sep='\t', header='infer', name=None, index_col=None, dtype=None, engine=None, nrows=None)   

参数说明：
sep:指定分隔符。read_csv默认为“，”，read_table默认为制表符“TAB”  
header:将某行作为列名，默认自动识别  
names:表示列名  
index_col:表示索引列的位置，可多重索引  
nrows:表示接受前n行  


```python
import pandas as pd
import numpy as np
```


```python
# 例5-1 读取csv文件

# 读到DataFrame中
df1=pd.read_csv(r'D:\My_Project\DataSet\iris.csv')

# 指定分隔符
df2=pd.read_table(r'D:\My_Project\DataSet\iris.csv', sep=',')

# 文件不含表头行，自动分配默认列名
df3=pd.read_csv(r'D:\My_Project\DataSet\iris.csv')
```

2. 文本文件的存储   
通过Pandas中的to_csv函数实现以csv文件格式存储文件  
DataFrame.to_csv(path_or_buf=None, sep=',', na_rap=' ',columns=None,header=True,index=True,index_label=None,mode='w',encoding=None)  
## 读写Excel文件  
1. Excel文件读取  
Pandas中的read_excel函数可读去xls、xlsx，格式为  
pandas.read_excel(io,sheetname,header=0,index_col=None,names=None,dtype)  
参数说明：
sheetname:Excel表内数据的分表位置


```python
# 例5-2 读Excel文件

xlsx=pd.excelFile(r'D:\My_Project\DataSet\Python数据分析\tips_mod.xls')
pd.read_excel(xlsx,'Sheet1')

frame=pd.read_excel(r'D:\My_Project\DataSet\Python数据分析\tips_mod.xls','Sheet1')
```

2, Excel文件存储  
用to_excel方法  
DataFrame.to_excel(excel_writer=None,sheetname=None,na_rep=' '.header=True,index=True,index_label=None,mode='w',encoding=None)  

和to_csv的区别：  
to_excel可以指定文件路径的参数名为excel_writer，并没有sep参数，增加了sheetnames参数

# 合并数据 
## merge数据合并  
通过一个或多个键将两个DataFrame按行合并，类似于SQL的join，格式如下：  
pd.merge(left, right, how='inner', on=None, left_on=None, right_on=None,left_index=False, right_index=False, sort=True,
         suffixes=('_x', '_y'), copy=True, indicator=False,validate=None)   

参数说明：
- left: 合并的左侧DataFrame对象
- right: 合并的右侧DataFrame对象
- on: 要加入的列或索引名称。 必须在左侧和右侧DataFrame对象中找到。 如果未传递且left_index和right_index为False，则DataFrame中的列的交集将被推断为连接键。
- left_on:左侧DataFrame中用于连接键的的列或索引。 可以是列名，索引级名称，也可以是长度等于DataFrame长度的数组。
- right_on: 右侧DataFrame中用于连接键的的列或索引。可以是列名，索引级名称，也可以是长度等于DataFrame长度的数组。
- left_index: 如果为True，则使用左侧DataFrame中的索引（行标签）作为其连接键。 对于具有MultiIndex（分层）的DataFrame，级别数必须与右侧DataFrame中的连接键数相匹配。
- right_index: 与left_index功能相似。  
- sort: 按字典顺序通过连接键对结果DataFrame进行排序。 默认为True，设置为False将在很多情况下显着提高性能。
- suffixes: 用于修改重复名，即重叠列的字符串后缀元组。 默认为（‘x’，’ y’）。
- copy: 始终从传递的DataFrame对象复制数据（默认为True），即使不需要重建索引也是如此。
- indicator:将一列添加到名为_merge的输出DataFrame，其中包含有关每行源的信息。 _merge是分类类型，并且对于其合并键仅出现在“左”DataFrame中的观察值，取得值为left_only，对于其合并键仅出现在“右”DataFrame中的观察值为right_only，并且如果在两者中都找到观察点的合并键，则为left_only。


```python
# 例5-3 默认合并数据
import pandas as pd
price=pd.DataFrame({'fruit':['apple','grape','orange','orange'],'price':[8,7,9,11]})
amount=pd.DataFrame({'fruit':['apple','grape','orange'],'amount':[5,11,8]})
display(price,amount,pd.merge(price,amount))
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>fruit</th>
      <th>price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>apple</td>
      <td>8</td>
    </tr>
    <tr>
      <th>1</th>
      <td>grape</td>
      <td>7</td>
    </tr>
    <tr>
      <th>2</th>
      <td>orange</td>
      <td>9</td>
    </tr>
    <tr>
      <th>3</th>
      <td>orange</td>
      <td>11</td>
    </tr>
  </tbody>
</table>
</div>



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>fruit</th>
      <th>amount</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>apple</td>
      <td>5</td>
    </tr>
    <tr>
      <th>1</th>
      <td>grape</td>
      <td>11</td>
    </tr>
    <tr>
      <th>2</th>
      <td>orange</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>fruit</th>
      <th>price</th>
      <th>amount</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>apple</td>
      <td>8</td>
      <td>5</td>
    </tr>
    <tr>
      <th>1</th>
      <td>grape</td>
      <td>7</td>
      <td>11</td>
    </tr>
    <tr>
      <th>2</th>
      <td>orange</td>
      <td>9</td>
      <td>8</td>
    </tr>
    <tr>
      <th>3</th>
      <td>orange</td>
      <td>11</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>



```python
# 例5-4 指定合并时的列名

display(pd.merge(price,amount,left_on='fruit',right_on='fruit'))
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>fruit</th>
      <th>price</th>
      <th>amount</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>apple</td>
      <td>8</td>
      <td>5</td>
    </tr>
    <tr>
      <th>1</th>
      <td>grape</td>
      <td>7</td>
      <td>11</td>
    </tr>
    <tr>
      <th>2</th>
      <td>orange</td>
      <td>9</td>
      <td>8</td>
    </tr>
    <tr>
      <th>3</th>
      <td>orange</td>
      <td>11</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>


merge合并时默认是内连接(inner)，通过how参数选择连接方式：左连接lefft，右连接right，外连接outer


```python
# 左连接

display(pd.merge(price, amount, how='left'))
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>fruit</th>
      <th>price</th>
      <th>amount</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>apple</td>
      <td>8</td>
      <td>5</td>
    </tr>
    <tr>
      <th>1</th>
      <td>grape</td>
      <td>7</td>
      <td>11</td>
    </tr>
    <tr>
      <th>2</th>
      <td>orange</td>
      <td>9</td>
      <td>8</td>
    </tr>
    <tr>
      <th>3</th>
      <td>orange</td>
      <td>11</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>



```python
# 右连接

display(pd.merge(price,amount,how='right'))
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>fruit</th>
      <th>price</th>
      <th>amount</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>apple</td>
      <td>8</td>
      <td>5</td>
    </tr>
    <tr>
      <th>1</th>
      <td>grape</td>
      <td>7</td>
      <td>11</td>
    </tr>
    <tr>
      <th>2</th>
      <td>orange</td>
      <td>9</td>
      <td>8</td>
    </tr>
    <tr>
      <th>3</th>
      <td>orange</td>
      <td>11</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>


也可通过多个键进行合并


```python
# 通过多键合并

left=pd.DataFrame({'key1':['one','one','two'],'key2':['a','b','a'],'value1':range(3)})
right=pd.DataFrame({'key1':['one','one','two','two'],'key2':['a','a','a','b'],'value2':range(4)})
display(left,right,pd.merge(left,right,on=['key1','key2'],how='left'))
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>key1</th>
      <th>key2</th>
      <th>value1</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>one</td>
      <td>a</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>one</td>
      <td>b</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>two</td>
      <td>a</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>key1</th>
      <th>key2</th>
      <th>value2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>one</td>
      <td>a</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>one</td>
      <td>a</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>two</td>
      <td>a</td>
      <td>2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>two</td>
      <td>b</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
</div>



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>key1</th>
      <th>key2</th>
      <th>value1</th>
      <th>value2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>one</td>
      <td>a</td>
      <td>0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>one</td>
      <td>a</td>
      <td>0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>one</td>
      <td>b</td>
      <td>1</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>two</td>
      <td>a</td>
      <td>2</td>
      <td>2.0</td>
    </tr>
  </tbody>
</table>
</div>


合并时出现重复列，通过参数suffixes处理


```python
# 例5-8

print(pd.merge(left,right,on='key1'))
print(pd.merge(left,right,on='key1',suffixes=('_left','_right')))
```

      key1 key2_x  value1 key2_y  value2
    0  one      a       0      a       0
    1  one      a       0      a       1
    2  one      b       1      a       0
    3  one      b       1      a       1
    4  two      a       2      a       2
    5  two      a       2      b       3
      key1 key2_left  value1 key2_right  value2
    0  one         a       0          a       0
    1  one         a       0          a       1
    2  one         b       1          a       0
    3  one         b       1          a       1
    4  two         a       2          a       2
    5  two         a       2          b       3


## concat连接
- 参考：
https://blog.csdn.net/stevenkwong/article/details/52528616?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-2.not_use_machine_learn_pai&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-2.not_use_machine_learn_pai

合并时没有连接键则无法使用merge方法。可使用concat方法，默认按行方向堆叠数据  


```python
# 例5-9 两个Series数据连接  

s1 = pd.Series([0,1],index = ['a','b'])
s2 = pd.Series([2,3,4],index = ['a','d','e'])
s3 = pd.Series([5,6],index = ['f','g'])
print(pd.concat([s1,s2,s3])) 
```

    a    0
    b    1
    a    2
    d    3
    e    4
    f    5
    g    6
    dtype: int64



```python
# 例5-10 两个DataFrame数据连接

data1 = pd.DataFrame(np.arange(6).reshape(2,3),columns = list('abc'))
data2 = pd.DataFrame(np.arange(20,26).reshape(2,3),columns = list('ayz'))
data = pd.concat([data1,data2],axis = 0)
display(data1,data2,data)
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>a</th>
      <th>b</th>
      <th>c</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>3</td>
      <td>4</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
</div>



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>a</th>
      <th>y</th>
      <th>z</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>20</td>
      <td>21</td>
      <td>22</td>
    </tr>
    <tr>
      <th>1</th>
      <td>23</td>
      <td>24</td>
      <td>25</td>
    </tr>
  </tbody>
</table>
</div>



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>a</th>
      <th>b</th>
      <th>c</th>
      <th>y</th>
      <th>z</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>3</td>
      <td>4.0</td>
      <td>5.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>0</th>
      <td>20</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>21.0</td>
      <td>22.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>23</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>24.0</td>
      <td>25.0</td>
    </tr>
  </tbody>
</table>
</div>



```python
s1
```




    a    0
    b    1
    dtype: int64




```python
# 例5-11 指定索引顺序

s1 = pd.Series([0,1],index = ['a','b'])
s2 = pd.Series([2,3,4],index = ['a','d','e'])
s3 = pd.Series([5,6],index = ['f','g'])
s4 = pd.concat([s1*5,s3],sort=False)
s5 = pd.concat([s1,s4],axis = 1,sort=False)
s6 = pd.concat([s1,s4],axis = 1,join = 'inner',sort=False)
#s7 = pd.concat([s1,s4],axis = 1,join = 'inner',join_axes= [s1.index],sort=False)
#display(s4,s5,s6,s7)
display(s4,s5,s6)
```


    a    0
    b    5
    f    5
    g    6
    dtype: int64



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>0</th>
      <th>1</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>a</th>
      <td>0.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>b</th>
      <td>1.0</td>
      <td>5</td>
    </tr>
    <tr>
      <th>f</th>
      <td>NaN</td>
      <td>5</td>
    </tr>
    <tr>
      <th>g</th>
      <td>NaN</td>
      <td>6</td>
    </tr>
  </tbody>
</table>
</div>



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>0</th>
      <th>1</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>a</th>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>b</th>
      <td>1</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
</div>


## combine_first 合并数据
合并两个DataFrame存在重复索引，需要用combine_first
例如 例5-10


```python
w1=pd.DataFrame({'0':[0,1], '1':[0,5]},index = ['a','b'])
w2=pd.DataFrame({'0':[0.0,1.0,None,None],'1':[0,5,5,6]},index = ['a','b','f','g'])
print(w1)
print(w2)
w1.combine_first(w2)
```

       0  1
    a  0  0
    b  1  5
         0  1
    a  0.0  0
    b  1.0  5
    f  NaN  5
    g  NaN  6





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>0</th>
      <th>1</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>a</th>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>b</th>
      <td>1.0</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>f</th>
      <td>NaN</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>g</th>
      <td>NaN</td>
      <td>6.0</td>
    </tr>
  </tbody>
</table>
</div>



# 数据清洗
试图填充缺失的数据值、光滑噪声、识别离群点并纠正数据的不一致  
## 检测与处理缺失值  
1. 缺失值的处理  
Pandas对象的所有描述性统计默认都不包括缺失数据。用浮点值NaN表示缺失数据  
Python内置的None也会被当作NA处理
    1. 缺失值的检测与统计  
    isnull()判断列中哪些是NaN


```python
# 例5-13 isnull()检测缺失值
string_data = pd.Series(['aardvark','artichoke',np.nan,'avocado'])
print(string_data)
string_data.isnull()
```

    0     aardvark
    1    artichoke
    2          NaN
    3      avocado
    dtype: object





    0    False
    1    False
    2     True
    3    False
    dtype: bool




```python
# 例5-14 Series中的None值处理

string_data = pd.Series(['aardvark','artichoke',None,'avocado'])
string_data.isnull()
```




    0    False
    1    False
    2     True
    3    False
    dtype: bool



       B. 缺失值的统计



```python
# 例5-15 用isnull().sum()统计缺失值

df = pd.DataFrame(np.arange(12).reshape(3,4),columns = ['A','B','C','D'])
df.iloc[2,:] = np.nan
df[3] = np.nan
print(df)
df.isnull().sum()
```

         A    B    C    D   3
    0  0.0  1.0  2.0  3.0 NaN
    1  4.0  5.0  6.0  7.0 NaN
    2  NaN  NaN  NaN  NaN NaN





    A    1
    B    1
    C    1
    D    1
    3    3
    dtype: int64




```python
# 例5-16 用info方法，查看列数据缺失情况

df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 3 entries, 0 to 2
    Data columns (total 5 columns):
     #   Column  Non-Null Count  Dtype  
    ---  ------  --------------  -----  
     0   A       2 non-null      float64
     1   B       2 non-null      float64
     2   C       2 non-null      float64
     3   D       2 non-null      float64
     4   3       0 non-null      float64
    dtypes: float64(5)
    memory usage: 248.0 bytes


2. 缺失值的处理  
    1. 删除缺失值
    dropna方法删除有缺失值的行。格式如下:
        dropna(axis=0,how='any',thresh=None,subset=None,inplace=False)


```python
# 5-17 Series的dropna用法

from numpy import nan as NA
data = pd.Series([1,NA,3.5,NA,7])
print(data)
print(data.dropna())
```

    0    1.0
    1    NaN
    2    3.5
    3    NaN
    4    7.0
    dtype: float64
    0    1.0
    2    3.5
    4    7.0
    dtype: float64



```python
# 例5-18 布尔型索引选择过滤非缺失值

not_null = data.notnull()
print(not_null)
print(data[not_null])
```

    0     True
    1    False
    2     True
    3    False
    4     True
    dtype: bool
    0    1.0
    2    3.5
    4    7.0
    dtype: float64



```python
# 例5-19 DataFrame对象的dropna默认参数使用

from numpy import nan as NA
data = pd.DataFrame([[1.,5.5,3.],[1.,NA,NA],[NA,NA,NA],\
                    [NA,5.5,3.]])
print(data)
cleaned = data.dropna()
print('删除缺失值后的:\n',cleaned)
```

         0    1    2
    0  1.0  5.5  3.0
    1  1.0  NaN  NaN
    2  NaN  NaN  NaN
    3  NaN  5.5  3.0
    删除缺失值后的:
          0    1    2
    0  1.0  5.5  3.0



```python
# 例5-20 丢弃全为NA的行

data = pd.DataFrame([[1.,5.5,3.],[1.,NA,NA],[NA,NA,NA],\
                    [NA,5.5,3.]])
print(data)
data.dropna(how='all')
```

         0    1    2
    0  1.0  5.5  3.0
    1  1.0  NaN  NaN
    2  NaN  NaN  NaN
    3  NaN  5.5  3.0





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>0</th>
      <th>1</th>
      <th>2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1.0</td>
      <td>5.5</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>NaN</td>
      <td>5.5</td>
      <td>3.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 例5-21 dropna中的axis参数应用

data = pd.DataFrame([[1.,5.5,NA],[1.,NA,NA],[NA,NA,NA],[NA,5.5,NA]])
print(data)
data.dropna(axis = 1,how = 'all')
```

         0    1   2
    0  1.0  5.5 NaN
    1  1.0  NaN NaN
    2  NaN  NaN NaN
    3  NaN  5.5 NaN





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>0</th>
      <th>1</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1.0</td>
      <td>5.5</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>NaN</td>
      <td>5.5</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 例5-22 dropna中的thresh参数应用

df = pd.DataFrame(np.random.randn(7,3))
df.iloc[:4,1] = NA # 前4行的第1列
df.iloc[:2,2] = NA # 前2行的第2列
print(df)
df.dropna(thresh = 2)
```

              0         1         2
    0 -1.027097       NaN       NaN
    1 -0.715950       NaN       NaN
    2  1.075964       NaN -0.030250
    3 -1.453970       NaN -0.681575
    4 -1.830810  2.588115 -1.638807
    5 -0.666622 -0.576411  1.535716
    6 -1.872218  0.484934  0.947951





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>0</th>
      <th>1</th>
      <th>2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2</th>
      <td>1.075964</td>
      <td>NaN</td>
      <td>-0.030250</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-1.453970</td>
      <td>NaN</td>
      <td>-0.681575</td>
    </tr>
    <tr>
      <th>4</th>
      <td>-1.830810</td>
      <td>2.588115</td>
      <td>-1.638807</td>
    </tr>
    <tr>
      <th>5</th>
      <td>-0.666622</td>
      <td>-0.576411</td>
      <td>1.535716</td>
    </tr>
    <tr>
      <th>6</th>
      <td>-1.872218</td>
      <td>0.484934</td>
      <td>0.947951</td>
    </tr>
  </tbody>
</table>
</div>



    B. 填补缺失值  
    缺失值所在特征是数值型，用均值、中位数、众数等描述其集中趋势的统计量来填充。
    缺失值所在特征是类别性，用众数填充。
    缺失值替换的方法fillna(),格式如下：
    pandas.DataFrame.fillna(value=None,method=None,axis=None,inplace=False,limit=None)


```python
# 例5-23 通过字典填充缺失值

df = pd.DataFrame(np.random.randn(5,3))
df.loc[:3,1] = NA
df.loc[:2,2] = NA
print(df)
df.fillna({1:0.88,2:0.66})
```

              0         1         2
    0 -0.595414       NaN       NaN
    1 -0.347033       NaN       NaN
    2 -0.437476       NaN       NaN
    3  0.675657       NaN -1.560513
    4  0.043776 -1.314931  0.553270





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>0</th>
      <th>1</th>
      <th>2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>-0.595414</td>
      <td>0.880000</td>
      <td>0.660000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-0.347033</td>
      <td>0.880000</td>
      <td>0.660000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-0.437476</td>
      <td>0.880000</td>
      <td>0.660000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.675657</td>
      <td>0.880000</td>
      <td>-1.560513</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.043776</td>
      <td>-1.314931</td>
      <td>0.553270</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 例5-24 fillna中method的应用

df = pd.DataFrame(np.random.randn(6,3))
df.iloc[2:,1] = NA
df.iloc[4:,2] = NA
print(df)
df.fillna(method = 'ffill')
```

              0         1         2
    0 -1.319586 -2.430584  0.055666
    1  1.340793  0.820610 -0.929141
    2  0.587160       NaN -0.667286
    3 -0.528885       NaN  0.152756
    4  0.030774       NaN       NaN
    5 -0.924639       NaN       NaN





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>0</th>
      <th>1</th>
      <th>2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>-1.319586</td>
      <td>-2.430584</td>
      <td>0.055666</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1.340793</td>
      <td>0.820610</td>
      <td>-0.929141</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.587160</td>
      <td>0.820610</td>
      <td>-0.667286</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-0.528885</td>
      <td>0.820610</td>
      <td>0.152756</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.030774</td>
      <td>0.820610</td>
      <td>0.152756</td>
    </tr>
    <tr>
      <th>5</th>
      <td>-0.924639</td>
      <td>0.820610</td>
      <td>0.152756</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 例5-25 用Series的均值填充

data = pd.Series([1.,NA,3.5,NA,7])
print(data)
data.fillna(data.mean())
```

    0    1.0
    1    NaN
    2    3.5
    3    NaN
    4    7.0
    dtype: float64





    0    1.000000
    1    3.833333
    2    3.500000
    3    3.833333
    4    7.000000
    dtype: float64




```python
# 例5-26 在DataFrame中用均值填充

df = pd.DataFrame(np.random.randn(4,3))
df.iloc[2:,1] = NA
df.iloc[3:,2] = NA
print(df)
df[1] = df[1].fillna(df[1].mean())
print(df)
```

              0         1         2
    0 -0.200361  0.499026  1.112101
    1  1.534996 -0.736657 -0.848329
    2 -0.986558       NaN -0.442458
    3  0.398115       NaN       NaN
              0         1         2
    0 -0.200361  0.499026  1.112101
    1  1.534996 -0.736657 -0.848329
    2 -0.986558 -0.118815 -0.442458
    3  0.398115 -0.118815       NaN


## 检测与处理重复值

duplicates方法判断各行是否有重复，返回布尔值的Series，反映每一行是否与前面的行重复


```python
# 例5-27 判断DataFrame中的重复数据

data = pd.DataFrame({'k1':['one','two']*3+['two'],'k2':[1,1,2,3,1,4,4],\
                    'k3':[1,1,5,2,1,4,4]})
print(data)
data.duplicated()
```

        k1  k2  k3
    0  one   1   1
    1  two   1   1
    2  one   2   5
    3  two   3   2
    4  one   1   1
    5  two   4   4
    6  two   4   4





    0    False
    1    False
    2    False
    3    False
    4     True
    5    False
    6     True
    dtype: bool



Pandas中的drop_duplicates课删除重复行，格式如下：
pandas.DataFrame(Series).drop_duplicates(self,subset=None,keep='first',inplace=False)


```python
# 例5-28 每行各个字段都相同时去重

data.drop_duplicates()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>k1</th>
      <th>k2</th>
      <th>k3</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>one</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>two</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>one</td>
      <td>2</td>
      <td>5</td>
    </tr>
    <tr>
      <th>3</th>
      <td>two</td>
      <td>3</td>
      <td>2</td>
    </tr>
    <tr>
      <th>5</th>
      <td>two</td>
      <td>4</td>
      <td>4</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 例5-29 指定部分列重复时去重

data.drop_duplicates(['k2','k3'])
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>k1</th>
      <th>k2</th>
      <th>k3</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>one</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>one</td>
      <td>2</td>
      <td>5</td>
    </tr>
    <tr>
      <th>3</th>
      <td>two</td>
      <td>3</td>
      <td>2</td>
    </tr>
    <tr>
      <th>5</th>
      <td>two</td>
      <td>4</td>
      <td>4</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 例5-30 去重时保留最后出现的记录

data.drop_duplicates(['k2','k3'],keep = 'last')
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>k1</th>
      <th>k2</th>
      <th>k3</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2</th>
      <td>one</td>
      <td>2</td>
      <td>5</td>
    </tr>
    <tr>
      <th>3</th>
      <td>two</td>
      <td>3</td>
      <td>2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>one</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>6</th>
      <td>two</td>
      <td>4</td>
      <td>4</td>
    </tr>
  </tbody>
</table>
</div>



## 检测与处理异常值  
个别数值明显偏离其余数据的值，一般用散点图、箱型图、3sigema法则检测  
1. 散点图


```python
# 例5-31 散点图检测异常值

wdf = pd.DataFrame(np.arange(20),columns = ['W'])
wdf['Y'] = wdf['W']*1.5+2
wdf.iloc[3,1] = 128
wdf.iloc[18,1] = 150
print(wdf)
wdf.plot(kind = 'scatter',x = 'W',y = 'Y')
```

         W      Y
    0    0    2.0
    1    1    3.5
    2    2    5.0
    3    3  128.0
    4    4    8.0
    5    5    9.5
    6    6   11.0
    7    7   12.5
    8    8   14.0
    9    9   15.5
    10  10   17.0
    11  11   18.5
    12  12   20.0
    13  13   21.5
    14  14   23.0
    15  15   24.5
    16  16   26.0
    17  17   27.5
    18  18  150.0
    19  19   30.5





    <AxesSubplot:xlabel='W', ylabel='Y'>




​    
![png](output_48_2.png)
​    


2. 箱型图  
用5个统计量：最小值、下四分位数、中位数、上四分位数、最大值，粗略的盘数据是否对称、分布的分散程度


```python
# 例5-32 用箱型图分析异常值

import matplotlib.pyplot as plt
plt.boxplot(wdf['Y'].values,notch = True)
```




    {'whiskers': [<matplotlib.lines.Line2D at 0x1e994bf81c0>,
      <matplotlib.lines.Line2D at 0x1e994bf8520>],
     'caps': [<matplotlib.lines.Line2D at 0x1e994bf8880>,
      <matplotlib.lines.Line2D at 0x1e994bf8be0>],
     'boxes': [<matplotlib.lines.Line2D at 0x1e994bd8df0>],
     'medians': [<matplotlib.lines.Line2D at 0x1e994bf8f40>],
     'fliers': [<matplotlib.lines.Line2D at 0x1e994c042e0>],
     'means': []}




​    
![png](output_50_1.png)
​    


3. 3sigema法则  
若数据服从正态分布，则3sigema原则下，异常值被定义为测量值中 与平均值的偏差超过3倍标准差的值


```python
# 例5-33 用3sigema原则检测异常

def outRange(S):
    blidx = (S.mean()-3*S.std()>S)|(S.mean()+3*S.std()<S)#找3sigema以外的值
    print(blidx)
    print('\n')
    idx = np.arange(S.shape[0])[blidx]
    print(idx)
    outRange = S.iloc[idx]
    return outRange
outier = outRange(wdf['Y'])
outier
```

    0     False
    1     False
    2     False
    3     False
    4     False
    5     False
    6     False
    7     False
    8     False
    9     False
    10    False
    11    False
    12    False
    13    False
    14    False
    15    False
    16    False
    17    False
    18     True
    19    False
    Name: Y, dtype: bool


​    
    [18]





    18    150.0
    Name: Y, dtype: float64



## 数据转换
1. 数据值替换  
将查询到的数据替换为指定数据。用Pandas中的replace进行


```python
# 例5-34 用replace替换数据值

data = {'姓名':['李红','小明','马芳','国志'],'性别':['0','1','0','1'],\
       '籍贯':['北京','甘肃','','上海']}
df = pd.DataFrame(data)
print(df)
print('\n')
df = df.replace('','不详')
print(df)
```

       姓名 性别  籍贯
    0  李红  0  北京
    1  小明  1  甘肃
    2  马芳  0    
    3  国志  1  上海


​    
       姓名 性别  籍贯
    0  李红  0  北京
    1  小明  1  甘肃
    2  马芳  0  不详
    3  国志  1  上海


也可对不同的值进行多只替换，传入参数是列表或字典。传入的第一列是被替换的值，第二列是替换值


```python
# 例5-35 用replace传入列表实现多值替换

df = df.replace(['不详','甘肃'],['兰州','云南'])
print(df)
```

       姓名 性别  籍贯
    0  李红  0  北京
    1  小明  1  云南
    2  马芳  0  兰州
    3  国志  1  上海



```python
# 例5-35 replace传入字典进行多值替换

df = df.replace({'1':'男','0':'女'})
print(df)
```

       姓名 性别  籍贯
    0  李红  女  北京
    1  小明  男  云南
    2  马芳  女  兰州
    3  国志  男  上海


2. 用函数或映射进行数据转换  
先自定义函数，再通过map方法实现


```python
# 例5-37 用map映射数据

data = {'姓名':['李红','小明','马芳','国志'],'性别':['0','1','0','1'],\
       '籍贯':['北京','兰州','兰州','上海']}
df = pd.DataFrame(data)
df['成绩'] = [58,86,91,78]
print(df)
print("\n")
def grade(x):
    if x>=90:
        return '优'
    elif 70<=x<90:
        return '良'
    elif 60<=x<70:
        return '中'
    else:
        return '差'
df['等级'] = df['成绩'].map(grade)
print(df)
```

       姓名 性别  籍贯  成绩
    0  李红  0  北京  58
    1  小明  1  兰州  86
    2  马芳  0  兰州  91
    3  国志  1  上海  78


​    
       姓名 性别  籍贯  成绩 等级
    0  李红  0  北京  58  差
    1  小明  1  兰州  86  良
    2  马芳  0  兰州  91  优
    3  国志  1  上海  78  良


# 数据标准化
## 离差标准化数据
对原始数据做一种线性 变换，映射到[0,1]


```python
def MinMaxScala(data):
    data = (data-data.min())/(data.max()-data.min())
    return data
x = np.array([[1.,-1.,2.],[2.,0.,0.],[0.,1.,-1.]])
print('原始数据为:\n',x)
x_scaled = MinMaxScala(x)
print('标准化后的矩阵为：\n',x_scaled,end = '\n')
```

    原始数据为:
     [[ 1. -1.  2.]
     [ 2.  0.  0.]
     [ 0.  1. -1.]]
    标准化后的矩阵为：
     [[0.66666667 0.         1.        ]
     [1.         0.33333333 0.33333333]
     [0.33333333 0.66666667 0.        ]]


## 标准差标准化数据  
经过本方法处理的数据均值为0，标准差为1


```python
# 例5-39 标准差标准化

def StandardScala(data):
    data = (data-data.mean())/data.std()
    return data
x = np.array([[1.,-1.,2.],[2.,0.,0.],[0.,1.,-1.]])
print('原始数据为:\n',x)
x_scaled = StandardScala(x)
print('标准化后的矩阵为：\n',x_scaled,end = '\n')
```

    原始数据为:
     [[ 1. -1.  2.]
     [ 2.  0.  0.]
     [ 0.  1. -1.]]
    标准化后的矩阵为：
     [[ 0.52128604 -1.35534369  1.4596009 ]
     [ 1.4596009  -0.41702883 -0.41702883]
     [-0.41702883  0.52128604 -1.35534369]]


# 数据转换
类别型数据变换或连续性数据的离散化  
## 类别型数据的哑变量处理  
用Pandas库中的get_dummies()


```python
# 例5-40 类别数据的哑变量处理

df = pd.DataFrame([['green','M',10.1,'class1'],\
                  ['red','L',13.5,'class2'],\
                  ['blue','XL',15.3,'class1']])
df.columns = ['color','size','prize','class label']
print(df)
pd.get_dummies(df)
```

       color size  prize class label
    0  green    M   10.1      class1
    1    red    L   13.5      class2
    2   blue   XL   15.3      class1





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>prize</th>
      <th>color_blue</th>
      <th>color_green</th>
      <th>color_red</th>
      <th>size_L</th>
      <th>size_M</th>
      <th>size_XL</th>
      <th>class label_class1</th>
      <th>class label_class2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>10.1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>13.5</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>15.3</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



## 连续性数据的离散化  
常用的离散化方法：等宽法、等频法、聚类分析法  
1. 等宽法  
用Pandas的cut函数，格式如下：
pandas.cut(x,bins,right=True,labels=None,retbins=False,precision=3)


```python
# 例5-41 cut函数应用  

np.random.seed(666)
score_list = np.random.randint(25,100,size = 10)
print('原始数据：\n',score_list)
bins = [0,59,70,80,100]
score_cut = pd.cut(score_list,bins)
print(pd.value_counts(score_cut))
#统计每个区间人数
```

    原始数据：
     [27 70 55 87 95 98 55 61 86 76]
    (80, 100]    4
    (0, 59]      3
    (59, 70]     2
    (70, 80]     1
    dtype: int64


2. 等频法  
将相同数量的记录放进每个区间  



```python
# 例5-42 等频法

def SameRateCut(data,k):
    k = 2
    w = data.quantile(np.arange(0,1+1.0/k,1.0/k)) # np.quantile
    print(w)
    data = pd.cut(data,w)
    return data
result = SameRateCut(pd.Series(score_list),3)
result.value_counts()
```

    0.0    27.0
    0.5    73.0
    1.0    98.0
    dtype: float64





    (73.0, 98.0]    5
    (27.0, 73.0]    4
    dtype: int64



3. 聚类分析法  
    - 将连续数据用聚类算法（如K-Means)进行聚类
    - 处理聚类得到的簇，为合并到一个族的连续型数据做同一标记

# 本章小结 
数据预处理：用Pandas进行数据导入、合并、清洗、标准化、转换

## 实训
对小费数据集进行预处理


```python
# 1.导入模块

import pandas as pd
import numpy as np
```


```python
#2.获得数据

fdata=pd.read_excel(r'D:\My_Project\DataSet\Python数据分析\tips_mod.xls',index_col=0) #选择不读取第一列信息
fdata.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>消费总额</th>
      <th>小费</th>
      <th>性别</th>
      <th>是否抽烟</th>
      <th>星期</th>
      <th>聚餐时间段</th>
      <th>人数</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>16.99</td>
      <td>1.01</td>
      <td>Female</td>
      <td>No</td>
      <td>Sun</td>
      <td>Dinner</td>
      <td>2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>10.34</td>
      <td>1.66</td>
      <td>Male</td>
      <td>No</td>
      <td>Sun</td>
      <td>Dinner</td>
      <td>3</td>
    </tr>
    <tr>
      <th>2</th>
      <td>21.01</td>
      <td>3.50</td>
      <td>Male</td>
      <td>No</td>
      <td>Sun</td>
      <td>Dinner</td>
      <td>3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>23.68</td>
      <td>3.31</td>
      <td>Male</td>
      <td>No</td>
      <td>Sun</td>
      <td>Dinner</td>
      <td>2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>24.59</td>
      <td>3.61</td>
      <td>Female</td>
      <td>No</td>
      <td>Sun</td>
      <td>Dinner</td>
      <td>4</td>
    </tr>
  </tbody>
</table>
</div>




```python
#3.分析数据 
##1.查看数据的描述信息

print(fdata.shape)
fdata.describe()
```

    (244, 7)





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>消费总额</th>
      <th>小费</th>
      <th>人数</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>244.000000</td>
      <td>244.000000</td>
      <td>244.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>19.785943</td>
      <td>2.998279</td>
      <td>2.569672</td>
    </tr>
    <tr>
      <th>std</th>
      <td>8.902412</td>
      <td>1.383638</td>
      <td>0.951100</td>
    </tr>
    <tr>
      <th>min</th>
      <td>3.070000</td>
      <td>1.000000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>13.347500</td>
      <td>2.000000</td>
      <td>2.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>17.795000</td>
      <td>2.900000</td>
      <td>2.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>24.127500</td>
      <td>3.562500</td>
      <td>3.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>50.810000</td>
      <td>10.000000</td>
      <td>6.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
##2.显示用餐时间time的不重复值

fdata['聚餐时间段'].unique()
```




    array(['Dinner', 'Lunch'], dtype=object)




```python
##3.修改拼写错误的字段值

fdata.loc[fdata['聚餐时间段']=='Diner','聚餐时间段'] = 'Dinner'
fdata.loc[fdata['聚餐时间段']=='Dier','聚餐时间段'] = 'Dinner'
fdata['聚餐时间段'].unique()
```




    array(['Dinner', 'Lunch'], dtype=object)




```python
##4.检测数据的缺失值

fdata.isnull().sum()
```




    消费总额     0
    小费       0
    性别       0
    是否抽烟     0
    星期       0
    聚餐时间段    0
    人数       0
    dtype: int64




```python
##5.删除一行内有两个缺失值的数据

fdata.dropna(thresh = 6,inplace = True)
fdata.isnull().sum()
```




    消费总额     0
    小费       0
    性别       0
    是否抽烟     0
    星期       0
    聚餐时间段    0
    人数       0
    dtype: int64




```python
##6.删除sex或time为空的行

fdata.dropna(subset = ['性别','聚餐时间段'],inplace = True)
fdata.isnull().sum()
```




    消费总额     0
    小费       0
    性别       0
    是否抽烟     0
    星期       0
    聚餐时间段    0
    人数       0
    dtype: int64




```python
##7.对剩余有空缺的数据用平均值替换

fdata.fillna(fdata.mean(),inplace = True)
fdata.isnull().sum()
```




    消费总额     0
    小费       0
    性别       0
    是否抽烟     0
    星期       0
    聚餐时间段    0
    人数       0
    dtype: int64




```python

```
