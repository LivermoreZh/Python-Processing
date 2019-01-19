# Python-Processing
pandas中的绘图函数（更加详细的绘图资料可参考pandas.pdf文档中的Visualization这一章） 
>>> import pandas as pd 
>>> import numpy as np 
>>> from pandas import Series, DataFrame 
>>> import matplotlib.pyplot as plt 
>>> import matplotlib 
>>> matplotlib.style.use('ggplot')

1， 绘图入门
在绘图之前先准备数据，数据形式必须是np.array()形式的数组数据，利用上面导入的matplotlib模块进行绘图； 
例子： 
>>> np.arange(20) 
array([ 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19]) 
>>> plt.plot(np.arange(20)) 
[<matplotlib.lines.Line2D object at 0x2ac914e15fd0>] 
>>> plt.show() 
最后生成一个由点连接的y=x的线性图 
>>> plt.plot(np.array([2.5, 4.1, 2.7, 8.8, 1.0])) #生成由5个点组成的两个点之间用线连接的折线 
如果想利用pandas绘图，可得到Series或DataFrame对象，并利用series.plot()或dataframe.plot()进行绘图； 
例子： 
>>> Series(np.array([2.5, 4.1, 2.7, 8.8, 1.0])) 
0 2.5 
1 4.1 
2 2.7 
3 8.8 
4 1.0 
dtype: float64 
>>> series=Series(np.array([2.5, 4.1, 2.7, 8.8, 1.0])) 
>>> series.plot() 
<matplotlib.axes._subplots.AxesSubplot object at 0x2ac914bab250> 
>>> plt.show() 
利用Series方法得到的折线图和plt.plot(np.arange(20))绘制的折线图图像大致相似，但图形的X轴坐标与Series的索引值相对应，X轴坐标从0到4，坐标原点为(0, 0)；

而对于DataFrame绘图，则其每个column都为一个绘图图线，会将每个column作为一个图线都绘制到一张图片当中，并用不同的线条颜色及不同的图例标签进行表示； 
例如： 
>>> dataframe=DataFrame({'A':[9.3, 4.3, 4.1, 5.0, 7.0], 'B':[2.5, 4.1, 2.7, 8.8, 1.0]}) 
>>> dataframe 
A B 
0 9.3 2.5 
1 4.3 4.1 
2 4.1 2.7 
3 5.0 8.8 
4 7.0 1.0 
>>> dataframe.plot() 
<matplotlib.axes._subplots.AxesSubplot object at 0x2ada670665d0> 
>>> plt.show()

··· ···所有绘图
会得出与上述相同的结果 
• ‘bar’ or ‘barh’ for bar plots #条状图 
• ‘hist’ for histogram #频率柱状图（计算某些值出现的频率） 
• ‘box’ for boxplot #箱线图（） 
• ‘kde’ or ‘density’ for density plots #密度图（需要scipy这个包） 
• ‘area’ for area plots #区域图（不同域的面积占比） 
• ‘scatter’ for scatter plots #散点图 >>> plt.scatter(df['part A'], df['part B']) 
• ‘hexbin’ for hexagonal bin plots # >>> plt.hexbin(df['part A'], df['part B'], df['part C']) 
• ‘pie’ for pie plots #饼图，比较适合与Series对象，看不同的占比 
上面罗列了所有可能绘制的图形 
df.plot.<TAB> #可以利用".<TAB>"的方法绘制不同的图像 
df.plot.area df.plot.barh df.plot.density df.plot.hist df.plot.line df.plot.scatter df.plot.bar df.plot.box df.plot.hexbin df.plot.kde df.plot.pie

2，线型图（Series.plot方法的参数，专用DataFrame的plot参数）
对于Series.plot方法的参数，DataFrame是可以应用的 
style参数，表示传给matplotlib的风格的字符串（如’ko–’），其中‘k’表示的是线条颜色，对于线条颜色的种类还有以下几个细分： 
Alias Colors 
b Blue 
g Green 
r Red 
c Cyan 
m Magenta 
y Yellow 
k Black 
w White 
其中‘o’表示线条中点的存在形式，o表示实心圆点，x表示x型点； 
其中‘-’表示线型，‘-’表示实线，‘–’表示虚线。 
也可将这三者表示形式分开，写法如： 
>>> series.plot(linestyle='dashed', color='k', marker='o') 
>>> series.index.name='site' 
Series对象的index.name值，在生成图表后会生成X轴的标签

对于DataFrame对象，其生成的表格的column值在进行绘图后会生成图例标签 
>>> dataframe.plot(linestyle='dashed', color='k', marker='o', xticks=[0, 1, 2, 3, 4], yticks=list(np.arange(0, 10.0, 0.5)) ,xlim=[-0.25, 4.25]) 
设定X及Y轴刻度值，以及X轴的刻度界限 
>>> dataframe.plot(title='dataframe photo') #加入图像的标题 
在绘图命令中加入subplots=True参数，则会将DataFrame当中的每一列结果绘制到一个子图片中，如果加入sharex=True参数，则各个子图片共用一个X轴标签；同理sharey=True表示共用一个Y轴； 
>>> dataframe.plot(subplots=True, sharex=True) 
array([<matplotlib.axes._subplots.AxesSubplot object at 0x06A627F0>, <matplotlib.axes._subplots.AxesSubplot object at 0x06E4E0D0>], dtype=object) 
>>> plt.show()

3，柱状图
在生成线形图的代码中加入kind=’bar’（垂直柱状图）或kind=’barh’（水平柱状图）即可生成柱状图。这时，Series和DataFrame的索引将会被用作X（bar）或Y（barh）刻度 
>>> dataframe.plot(kind='bar')········>>> dataframe.plot(kind='barh') 
生成柱状图，DataFrame对象的每一列会生成一个柱状图结果，多个列会将这个结果绘制在一个表格中，不同的列所绘制的柱状图颜色不同； 
>>> dataframe.index=['once', 'twice', 'thrice', 'forth', 'fifth'] 
>>> dataframe 
··A·B 
once·9.3·2.5 
twice·4.3·4.1 
thrice·4.1·2.7 
forth·5.0·8.8 
fifth·7.0·1.0 
绘制的柱状图的X轴或Y轴标签为DataFrame对象的index值 
plt.figure(); dataframe.plot(kind=’bar’); plt.axhline(0, color=’k’)，”.axhline”的主要作用是在纵轴x=0的位置加入一条黑色的直线，来分隔y>0的轴和y<0的轴。 
堆积柱状图：设置stacked=True即可为DataFrame生成堆积柱状图，这样每行的值就会被堆积在一起： 
>>> df=DataFrame({'part A': [2.8, 5.5, 4.5, 7.0, 1.0], 'part B': [4.2, 1.2, 4.5, 2.5, 8.0], 'part C': [3.0, 3.3, 1.0, 0.5, 1.0]}, index=['May', 'June', 'July', 'August', 'September']) 
>>> df.name='bonus' 
>>> df 
···part A·part B·part C 
May·2.8·4.2·3.0 
June·5.5·1.2·3.3 
July·4.5·4.5·1.0 
August·7.0·2.5·0.5 
September·1.0·8.0·1.0 
>>> df.plot(kind='barh', stacked=True) 
最终绘制出堆积图，同时： 
>>> df.plot.barh(stacked=True)

4，直方图密度图
直方图（histogram）是一种可以对值频率进行离散化显示的柱状图。数据点被拆分到离散的、间隔均匀的面元中，绘制的是各面元中数据点的数量。 
>>> length=DataFrame({'length': [10, 20,15,10,1,12,12,12,13,13,13,14,14,14,51,51,51,51,51,4,4,4,4]}) 
>>> length.plot.hist() 
>>> plt.show() 
最终得到的数字频率分布直方图，X轴是DataFrame当中的数值分布，Y轴是对应数值出现的次数； 
密度图，利用数值出现频率绘制的直方图进行曲线拟合，会得到密度图；绘制的图形是根据直方图得到的条状分布的顶点连接后得到的平滑曲线，X轴是DataFrame当中的数值分布，Y轴是密度（Density）。 
>>> length.plot.density(color='k') #length.plot.kde(color=’k’)得到同样的图形结果 
>>> plt.show()

直方图画法进阶 
>>> df4 = DataFrame({'a': np.random.randn(1000) + 1, 'b': np.random.randn(1000), 'c': np.random.randn(1000) - 1}, index=range(1,1001), columns=['a', 'b', 'c']) 
设定1000个随机数为column b，而随机数的数值+1和-1作为column a和column c； 
>>> plt.figure() #表示设定绘制图标对象 
>>> df4.plot.hist(stacked=True, bins=20, alpha=0.5) #bins=20表示数值分辨率，具体来说是将随机数设定一个范围，例如5.6，5.7，6.5，如果数值分辨率越低，则会将三个数分到5-7之间，如果数值分辨率越高，则会将5.6，5.7分到5-6之间，而6.5分到6-7之间；值越小表示分辨率越低，值越大表示分辨率越高；

>>> df4['a'].plot.hist(orientation='horizontal', cumulative=True) #该图是将DataFrame对象当中的a进行数值累加，并绘制横向直方图，横轴表示频率（Frequency），纵轴表示数值，cumulative=True的效果是将Frequency的数值从大到小进行排列。 
>>> df4.diff().hist(color='k', alpha=0.5, bins=50) #该效果是将DataFrame当中column分开，即将a，b和c分开绘制成三张图。df4.diff().hist()可达到这个效果，即将所有column分开。

5，箱线图
箱线图所表示的各个数值的含义：线条右下到上分别表示 
最小值、第一四分位数、中位数、第三四分位数和最大值 
第一四分位数（Q1），又称“较小四分位数”或“下四分位数”，等于该样本中所有数值由小到大排列后第25%的数字； 
第二四分位数（Q2），又称“中位数”，等于该样本中所有数值由小到大排列后第50%的数字； 
第三四分位数（Q3），又称“较大四分位数”或“上四分位数”，等于该样本中所有数值由小到大排列后第75%的数字； 
第三四分位数与第一四分位数的差距又称四分位间距（InterQuartile Range，IQR）。 
计算四分位数首先要确定Q1、Q2、Q3的位置（n表示数字的总个数）： 
Q1的位置=（n+1）/4 
Q2的位置=（n+1）/2 
Q3的位置=3（n+1）/4 
具体解释及实例可参考如下网址：http://blog.csdn.net/zhanghongju/article/details/18446131

箱线图可以用如下方式绘制 
Series.plot.box()， DataFrame.plot.box()， DataFrame.boxplot() 
例如： 
>>> df = pd.DataFrame(np.random.rand(10, 5), columns=['A', 'B', 'C', 'D', 'E']) 
>>> df.plot.box() 
绘制ABCDE这5个箱线图 
np.random.rand产生的随机数都为0-1之间的正数，而np.random.randn产生的随机数中既有正值又有负值

修改箱线图线条颜色需要有一下4个方面，即boxes（盒身），whiskers（须）， 
medians（中位数），caps（最大值，最小值），可以将颜色与上面的4个keys建立字典关系，并在绘图时引入color。 
>>> color = dict(boxes='DarkGreen', whiskers='DarkOrange', medians='DarkBlue', caps='Gray')) 
>>> df.plot.box(color=color, sym='r+') #boxplot has sym keyword to specify fliers style 
>>> df.plot.box(color=color) #绘图效果和上述一样 
盒身为深绿色，须为深黄色，中位数为深蓝色，最大最小值为灰色

>>> df.plot.box(vert=False, positions=[1, 4, 5, 6, 8]) #可绘制水平箱线图，positions表示的意思是ABCDE这5个箱线图摆放位置，A在1位置，B在4位置，AB之间间隔2，3这两个位置。

同样，也可以根据DataFrame属性当中的’by’参数进行分组画图 
You can create a stratified boxplot using the by keyword argument to create groupings 
>>> df = pd.DataFrame(np.random.rand(10,2), columns=['Col1', 'Col2'] ) 
>>> df['X'] = pd.Series(['A','A','A','A','A','B','B','B','B','B']) #在df存在Col1和Col2两列的基础上增加X这一列，然后根据index编号进行赋值；对于DataFrame对象以Series方法增加column。 
>>> plt.figure(); bp = df.boxplot(by='X') 
注意：如果用>>> plt.figure(); df.plot.box(by='X')或者>>> df.plot.box(by='X')方法绘制图片，最终不会得到分组的画图效果

使用两组标签可以实现多分组绘图 
>>> df = pd.DataFrame(np.random.rand(10,3), columns=['Col1', 'Col2', 'Col3']) 
>>> df['X'] = pd.Series(['A','A','A','A','A','B','B','B','B','B']) 
>>> df['Y'] = pd.Series(['A','B','A','B','A','B','A','B','A','B']) 
>>> df 
Col1 Col2 Col3 X Y 
0 0.081142 0.059051 0.862650 A A 
1 0.142670 0.554862 0.931373 A B 
2 0.720987 0.968380 0.560167 A A 
3 0.104103 0.031059 0.832768 A B 
4 0.153403 0.050152 0.747177 A A 
5 0.146358 0.016951 0.013774 B B 
6 0.387617 0.485473 0.583479 B A 
7 0.447248 0.498259 0.424981 B B 
8 0.539854 0.386803 0.410169 B A 
9 0.129224 0.910483 0.348707 B B 
>>> plt.figure(); bp = df.boxplot(column=['Col1','Col2'], by=['X','Y']) 
最终绘制的图片样式为，出现两个分组，分组后的X轴的大标签是[X, Y]，每个分组的X轴的小标签为(A, A) (A, B) (B, A) (B ,B)，这样绘图也是根据df的X列和Y列的分组结果而得到的。

6，区域面积图
绘图方式：Series.plot.area()和DataFrame.plot.area() 
NA值的处理方法： 
When input data contains NaN, it will be automatically filled by 0. If you want to drop or fill by different values, use dataframe.dropna() or dataframe.fillna() before calling plot 
>>> df = pd.DataFrame(np.random.rand(10, 4), columns=['a', 'b', 'c', 'd']) 
>>> df.plot.area() #生成堆积图 
>>> df.plot.area(stacked=False) #非堆积效果图

7，散点图
绘图方式：DataFrame.plot.scatter() 
散点图需要设定X轴及Y轴的数值；Scatter plot requires numeric columns for x and y axis. 
>>> df = pd.DataFrame(np.random.rand(50, 4), columns=['a', 'b', 'c', 'd'])#abcd四列中，各列设定50个随机数 
>>> df.plot.scatter(x='a', y='b') #之后以a列为X轴数值，b列为Y轴数值绘制散点图 
如果想将不同的散点图信息绘制到一张图片当中，需要利用不同的颜色和标签进行区分 
To plot multiple column groups in a single axes, repeat plot method specifying target ax. It is recommended to specify color and label keywords to distinguish each groups. 
>>> ax = df.plot.scatter(x='a', y='b', color='DarkBlue', label='Group 1') #先设定第一个散点图，颜色为深蓝色标签为Group 1，以ab两列作为x及y轴的值 
>>> df.plot.scatter(x='c', y='d', color='DarkGreen', label='Group 2', ax=ax) #第二个散点图以cd两列作为x及y轴的值，颜色为深绿色标签为Group 2，ax=ax的作用是将ax这个图绘制到Group 2图片当中，形成两层图形嵌套关系

如果想得到4层图形嵌套关系需要运用如下方法： 
>>> ab=df.plot.scatter(x='a', y='b', color='DarkBlue', label='Group 1') #a列与b列的绘图关系 
>>> abcd=df.plot.scatter(x='c', y='d', color='DarkGreen', label='Group 2', ax=ab) #将ab的绘图关系嵌套到c列和d列的绘图关系中：ax=ab 
>>> abcdac=df.plot.scatter(x='a', y='c', color='DarkRed', label='Group 3', ax=abcd) #将ab的绘图关系及cd的绘图关系的汇总关系当中加入到a列和c列的绘图关系中：ax=abcd 
>>> df.plot.scatter(x='b', y='d', color='DarkGray', label='Group 4', ax=abcdac) #将上述3种关系的绘图嵌套到b列和d列的绘图关系中：ax=abcdac

>>> help(df.plot.scatter) 
scatter(self, x, y, s=None, c=None, **kwds) 
c参数表示的是点图的灰度水平，用0-1数值表示，1表示透明，0表示不透明，数值越大透明度越高，将数值写入单引号中； 
The keyword c may be given as the name of a column to provide colors for each point 
>>> df.plot.scatter(x='a', y='b', c='c', s=50) #c参数设定为df的第c列数值，表示的是绘制的点的灰度水平，同时灰度水平会出现一个灰度梯度标签表示不同的灰度级别。

s参数表示的是点的大小，用pd.Series表示 
You can pass other keywords supported by matplotlib scatter. Below example shows a bubble chart using a dataframe column values as bubble size 
>>> df.plot.scatter(x='a', y='b', s=df['c']*200) #用df的c列数值的200倍表示点的大小

8，饼图
绘图方式：DataFrame.plot.pie()和Series.plot.pie() 
饼图展现的是百分比关系，所以数值当中如果出现NA值在绘图时默认为0，负值会报错 
If your data includes any NaN, they will be automatically filled with 0. A ValueError will be raised if there are any negative values in your data. 
>>> series = pd.Series(3 * np.random.rand(4), index=['a', 'b', 'c', 'd'], name='series') #首先创建一个Series对象 
>>> series 
a 2.311426 
b 1.595443 
c 2.268239 
d 2.227010 
Name: series, dtype: float64 
>>> series.plot.pie(figsize=(6, 6)) #绘制饼图，图片规格为figsize=(6, 6) 
对于Series对象，生成4个随机数，在生成饼图之后看各个index的占比 
而对于DataFrame对象，每一个column都可独立绘制一张饼图，但需要利用subplots=True参数将，每个饼图绘制到同一张图中。 
>>> df = pd.DataFrame(3 * np.random.rand(4, 2), index=['a', 'b', 'c', 'd'], columns=['x', 'y']) 
>>> df 
···x y 
a 2.116810 0.643499 
b 0.678533 0.164728 
c 2.142849 2.590517 
d 2.077125 2.926192 
>>> df.plot.pie(subplots=True, figsize=(8, 4)) 
最终绘制两张饼图，分别是column x和y及各自的abcd占比 
You can use the labels and colors keywords to specify the labels and colors of each wedge 
>>> series = pd.Series(3 * np.random.rand(4), index=['a', 'b', 'c', 'd'], name='series') 
>>> series 
a 2.548463 
b 1.716728 
c 2.009944 
d 0.735294 
Name: series, dtype: float64 
>>> series.plot.pie(labels=['AA', 'BB', 'CC', 'DD'], colors=['r', 'g', 'b', 'c'], autopct='%.2f', fontsize=20, figsize=(6, 6)) 
绘制的饼图，label分别是AA对应a，BB对应b，CC对应c，DD对应d，四种颜色rgbc分别对应，autopct显示各个index所占百分比，并保留两位小数，字体大小fontsize，图片大小figsize。 
If you pass values whose sum total is less than 1.0, matplotlib draws a semicircle 
如果所有数字总和加起来小于1.0，则会画一个半圆。 
>>> series = pd.Series([0.1] * 4, index=['a', 'b', 'c', 'd'], name='series2') 
>>> series 
a 0.1 
b 0.1 
c 0.1 
d 0.1 
Name: series2, dtype: float64 
>>> series.plot.pie(figsize=(6, 6)) 
各个index数值都为0.1，所以只能绘制半圆

9，各种绘图方式对于缺失值的处理
Missing values are dropped, left out, or filled depending on the plot type 
| Plot Type | NaN Handling | 
| Line | Leave gaps at NaNs | 
| Line (stacked) | Fill 0’s | 
| Bar | Fill 0’s | 
| Scatter | Drop NaNs | 
| Histogram | Drop NaNs (column-wise) | 
| Box | Drop NaNs (column-wise) | 
| Area | Fill 0’s | 
| KDE | Drop NaNs (column-wise) | 
| Hexbin | Drop NaNs | 
| Pie | Fill 0’s | 
If any of these defaults are not what you want, or if you want to be explicit about how missing values are handled, consider using fillna() or dropna() before plotting. 
散点图，柱状图，箱线图，密度图，都是将NA值去除 
堆积图，条状图，区域图，饼图都是将NA值填充为0
--------------------- 
作者：genome_denovo 
来源：CSDN 
原文：https://blog.csdn.net/genome_denovo/article/details/78322628 
版权声明：本文为博主原创文章，转载请附上博文链接！
