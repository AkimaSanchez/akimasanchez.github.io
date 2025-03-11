#### 写在前面：由于每个人的系统和环境不同，本人就不在此赘述Python的开发环境，请自行配置好Python开发环境，同时Python语言真的很简单，考教资的时候才发现现在小学生已经学上了，我那时候还在学Scratch，时代变了（悲），也就是说小学生都能学会的东西大部分人都能自学，相信你自己。记住，纸上谈来终觉浅，一定要动手敲一遍代码才能更好的理解。
### 本人使用开发环境（仅供参考）：
- Anaconda 22.9.0 
- Jupyter Notebook 6.4.12
- NumPy 1.21.5
- pandas 1.4.4
### NumPy模块
&emsp;&emsp;numpy模块功能非常强大，支持大量的多维数组与矩阵计算，也针对数组运算提供大量的数学函数库，同时支持广播功能函数，线性代数运算，傅里叶变换等功能。这里仅给出常用的一些函数使用示例，同时约定俗成，将NumPy简称为np。
1. 生成NumPy数组
```python
#生成一维数组
arr0=np.array([1,3,5,7])

#生成二维数组
arr1=np.array([[1,3], [5,7]])

#生成给定范围内的数组 array([0,1,2,3,4,5,6,7,8,9])
np.arange(10)

#生成指定步长的数组 array([0,0.5,1,1.5,2,2.5,3,3.5])
np.arange(0, 4, 0.5) 

#生成2行，3列的0值数组
np.zeros((2,3))

#生成2行，3列的填充值为1的数组
np.ones((2,3))

#生成2行，3列的对角线（主对角线（左上到右下））填充值为1的矩阵
np.eye(2,3)

#生成随机数组
np.random.rand(3,3)

#生成随机整数数组
np.random.randint(1, 10, (3,3))
```
2. NumPy数据基本运算
- 对于维数相同数组
```python
arr0=np.array([1,2,3])
arr1=np.array([2,3,4])
arr0+arr1
```
>array([3,5,7])
```python
arr0*arr1
```
>array([2,6,12])
- 对于维数不同的数组（类似于矩阵相乘，前一个矩阵列数等于后一个矩阵行数）
```python
arr2=np.random.randint(1,10,(2,3))
arr3=np.random.randint(1,10,(3,2))
np.dot(arr2,arr3)
```
>array([[86,100],[140,128]])

3. Numpy数组统计方法
```python
arr4=np.random.randint(1, 10, (3, 3))
#求数组中最大值
np.max(arr4)

#求数组中最小值
np.min(arr4)

#求解数组**行**方向的最小值
np.amin(arr4,0)

#求解数组**列**方向的最小值
np.amin(arr4,1)

#求解数组的中位数
np.median(arr4)
```
### pandas模块
&emsp;&emsp;pandas是基于NumPy构建的数据分析库，但它比NumPy有更高级的数据结构和分析工具，如Series类型、DataFrame类型等。在此约定俗成，将pandas简称为pd。
1. pandas数据结构之Series
```python
pd.Series(data=[1,2,3,4])
```
>0  1
1  2
2  3
3  4
>dtype: int64
```python
pd.Series(data=[1,2,3,4], index=['a', 'b', 'c', 'd'])
```
>a  1
b  2
c  3
d  4
>dtype: int64

&emsp;&emsp;如上面的例子所示，创建Series时，语句中的data为数据源，index为索引。当index没有指定时，默认为从0开始的标签。上面输出的dtype是data的数据类型。
&emsp;&emsp;当Series数组元素为数值时，我们可以使用Series对象的describe方法对Series数组的数值进行分析，演示如下所示，具体的输出可以自己敲一遍看看效果。
```python
data=np.random.randint(1,100,10)
s=pd.Series(data=data)
s.describe()
```
&emsp;&emsp;对于一个Series数据类型的对象，可以使用append方法实现数组元素的追加，也可以理解为两个Series对象的合并。
```python
s1=pd.Series([1,2,3], index=['a', 'b', 'c'])
s2=pd.Series([4,5,6], index=['d', 'e', 'f'])
s=s1.append(s2)
s
```
>a  1
b  2
c  3
d  4
e  5
f   6
>dtype:  int64

&emsp;&emsp;而drop方法可以对Series数据类型的序列元素进行删除。
```python
s.drop(index='a', inplace=True)
s
```
&emsp;&emsp;drop方法的inplace值默认为Flase，设置为True时本地删除。

2. pandas数据结构之DataFrame

&emsp;&emsp;我们可以将Series视作Excel表中的一列，那么DataFrame就是Excel中的一张工作表。DataFrame的创建语句如下：
```python
pd.DataFrame(data, index, columns)
```
&emsp;&emsp;其中data为数据源，index为行索引，columns为列索引。data为数组元素，可以由一系列数据构成，大多数情况下由多列数据构成，同时每一列的数据类型**必须**相同。当index和columns不指定时，从0开始。通常来说，列索引最好给定，因为这样每一列数据的属性可以由列索引描述。读者可以尝试以下代码体会一下：
```python
data=np.random.randint(1, 100, (3, 4))
df=pd.DataFrame(data, columns=['a', 'b', 'c', 'd'])
df
```
&emsp;&emsp;调用DataFrame对象中的info方法可以获得表的信息概述，包括行索引、列索引、非空数据个数和数据类型信息。
```python
#返回概述
df.info()

#返回行索引
df.index

#返回列索引
df.columns

#返回值
df.values
```
&emsp;&emsp;DataFrame中访问数据的方法
```python
#查看原始的df数据
df

#访问df的第1-2行数据，返回DataFrame数据对象
df[0:2]  

#访问df列为a的数据，返回DataFrame数据对象
df.a  

#访问df的第2行，列名为a的数据，返回数据
df.at[1, 'a']  

#访问df的第2-3行，第2-3列包围的数据，返回数据
df.iloc[1:3, 1:3]  
```
对DataFrame数据进行基本操作的方法
```python
#添加列，索引为e，将列的值设置为2
df['e']=2

#添加列，索引为3，将列的值设置为3
df.loc[3]=3

#将df索引为2的行和索引为c的列的数值修改为4
df.at[2, 'c']=4

#删除df数据中行索引为3的数据，axis为0时是行，1时为列
df.drop(3, axis=0, inplace=True)
```
DataFrame中数据分析常用的方法如下表所示：
![DataFrame中数据分析常用方法表](https://i.postimg.cc/4dssG4mD/2b742905a9fdb9c686aad76054768f0.jpg)

### Matplotlib/Seaborn模块
&emsp;&emsp;在数据分析的流程中，结果的呈现是非常重要的步骤，美观规范的图表会让客户直观快速地了解数据变化的趋势，找到有关数据变化的原因。这个绘图库后面会讲到，这里就不再赘述。

### 其他模块
1. SciPy模块
&emsp;&emsp;基于NumPy的计算模块包，该模块可以处理插值、积分、优化、图像处理、常微分方程数值解的求解、信号处理等问题，主要与NumPy矩阵共同使用，广泛应用与数据科学、人工智能、数学、机械制造和生物工程等领域。

2. Stasmodels模块
&emsp;&emsp;重要的统计建模和计量经济学工具库，该模块用于估计许多不同统计模型以及进行统计测试和统计数据探索的类和函数。常用的模型包括线性模型、广义线性模型和鲁棒线性模型、线性混合效应模型、方差分析（ANOVA）方法、时间序列过程和状态空间模型、广义的矩量法等，具体可以直接访问官网[stasmodels-link]:www.statsmodels.org。

3. Scikit-Learn模块
&emsp;&emsp;重要的机器学习库，也称为sklearn，是针对Python编程语言的**免费**机器学习库。它具有各种分类，回归和聚类算法，包括支持向量机，随机森林，梯度提升，k均值和DBSCAN（集群分析算法），旨在与Python数值科学库NumPy和SciPy联合使用。

### 结语
&emsp;&emsp;这一章我们主要稍微学习了一下Python数据分析中常见的各种库的使用方法，特别是NumPy和pandas库建议读者一定要去动手敲一下看一下效果，加深印象，同时加深对这两个库的理解，也可以去网上找一些项目去实践一下，巩固学习成果。
&emsp;&emsp;好，第一章就这样结束了，愿您学习愉快，同时有不足的地方尽情指出。