## Python

##### 1.数字类型

实数：

```python
0.4-0.1==0.3
False
abs(0.4-0.1-0.3<1e-6) #判断实数相等的办法
True
```

复数：

```python
x = 3+4j
abs(x) #求复数模
x.conjugate() #求共轭复数 x=3-4j
x.real
x.imag
```

二进制数：

二进制ob、八进制Oo、十六进制Ox

字符串：

```python
x = 'good' + 'morning' #连接字符串
x * 3 #字符串重复
‘.’.join() #连接
```

列表：

```python
list = [1,2,3]
list[1] #下标访问
```

元组：

```python
tuple = (1,2,3)
tuple[1] #下标访问
```

字典：

```python
dict={'a':97,'b':98}
dict['a'] #下标访问
```

集合：

```python
set = {1,2,3}
set[1] # ×
```

~~set[1]~~     #不支持下标访问

##### 2.运算式

```python
3/2
1.5 #数学上除法

15//4
3 #整除

789%10
9 #求余 --求最后一位数

'%c'%(65)
'A' # %字符串格式化

z1={1,2,3}
z2={3,4,5}

{}<{} #真子集
{}<={} #子集
& ={3}#交集
| ={1,2,3,4,5}#并集
^ ={1,2,4,5}#对称差集
- ={1,2} #差集

and or not #逻辑运算符具有短路或惰性求值
3 and 5
5        #把最后一个计算的表达式值返回

++i # 正正为正
```

##### 3.类型转换

```python
bin(555) 二
act(555) 八
hex(555) 十六

ord('a') 编码unicode
chr(65) 对应字符

字符串转4种类型
list(range(5))
tuple(range(5))
dict(zip('2134','abcds'))
set('1234')
字典(key)转字符串、元组、列表、集合
元组转字符串、列表、集合、不可转字典
列表转字符串、元组、集合、不可转字典
集合转字符串、元组、列表、不可转字典

type(3)
isinstance(3,int)

max([],key=len) key指定规则
	max(list,key=lambda x:x[1])

print('i=',i,sep=' ',end='\n')

import random
random.shuffle(x) 打乱
x.sort()
	sorted(x,key=lambda item:len(str(item)),reverse=True)
x.reverse()
	reversed(x) 翻转

count() 出现次数
index() 首次出现索引

对象
list(map(str,range(5))) 
	['0','1','2','3','4']  #map(1,2)多参————>函数（一个个对应映射）
reduce(lambda x,y:x+y,range(1,10))# redece(1,2)多参————>函数（一个个迭代映射）
	45
filter(func,seq)#filter(1,2)多参————>函数（一个个映射函数为true才行）

推导式
[x+y for x in [1,2,3] for y in [3,1,4] if x!=y] #列表推导式（生成 列表）
(i+2 for i range(10)) #生成器对象 
		1.只可访问一次 next(g) 或 g.__next__()
    	2.生成对象
 
```

##### 4.切片(不包含end位置)

```python
list = [3,5,7,9]
list[:3] = [1,2,3] #切片不连续，=两边必相同
	[1, 2, 3, 9]
list[3:] = [4,5,6] #从index=3开始(切片连续，=两边可不同)
	[3, 5, 7, 4, 5, 6]
list[::2] = ['a','b','c']
	['a', 5, 'b', 4, 'c', 6]
list[::2] = ['a'] # × 不连续切片
list[:3] = [] #删除元素
del list[:3]
```

##### 5.列表

常用方法

```python
append() #尾部加一个
insert(index,x) #指定位置插入
extend() #尾部加多个

pop([index]) #尾部删,index默认-1
remove() #删除第一个出现，不存在抛出异常

reverse() #对列表所有元素原地逆序
sort(key=None,reverse=True) #原地排序，key指定排序规则，reverse=True为降序

count(x) #返回指定元素出现的次数
index(x) #返回元素首次出现的位置
```

##### 6.字典

```python
1.两种括号访问
dict={'age':36,'score':[89,97]}
	dict['age'] 
	dict.get('age')
2.items()遍历
for item in dict.items():
```

##### 7.集合

常用方法

```python
add(2)
pop() #随机删
remove() #删指定
集合运算-内置函数
len() max() sorted() map() filter() enumerate()
```

##### 8.序列解包

列表、字典、enumerate对象、filter对象、zip对象

##### 7.选择、循环

```python
if __:
elif __:
    
for i in set:   
else:最后执行，若break结束，则不执行
```

##### 8.函数

```python
位置参数：
def demo(a,b,c)

默认值参数：
def say(times=1)  #默认值右边不能出现没有默认值的参数

关键参数：
def demo(1,b,c)
调用
demo(c=2,b=1,a=3)
可变长度参数：
def demo(*p) #元组参数
def demo(**p) #字典参数
```

##### 9.参数序列解包

```python
dict ={1:'a',2:'b'}
tumple=(1,2,3)
def demo(a,b,c):
    print(a+b+c)
demo(*tumple)d
```

##### 10.变量作用域

```python
global x 在函数内全局变量声明
```

##### 11.生成器函数（yield语句）

```python
yield:返回值，挂起后面代码，下次通过__next__()、next()继续执行
return:返回值后立刻结束函数
```

##### 12.面向对象

两种数据成员：

​	1.对象数据成员：`__init__`中定义，访问时一般以self访问

​	2.类数据成员：不在任何方法内，类共享

四种成员方法：

​	1.对象实例方法（只能对象调用）

​		公有方法：self为第一个参数

​		私有方法

​	2.类方法（类和对象都能调用）

​		静态方法 `@staticmethod`

​		类方法 `@classmethod`  `(cls)`作为第一个参数

属性权限

​	只读 `@property`

​	可读、可写 `property(get,set)`

​	可读、可写、可删 `property(get,set,del)`

```python
'''
编写一个学生和教师数据输入和输出的程序。
学生数据包括编号、姓名、班号、和成绩；
教师的数据含有编号、姓名、职称和部门。
要求设计一个person类，作为学生数据操作类student和教师数据操作类teacher的基类
'''
class person(object):
    def __init__(self,number,name):
        self.setNumber(number)
        self.setName(name)
    def setNumber(self,number):
        self.number = number
    def setName(self,name):
        self.name = name
    def getNumber(self):
        return self.number
    def getName(self):
        return self.name
    def show(self):
        print('编号为：',self.getNumber(),'姓名为：',self.getName())
class student(person):
    def __init__(self,number,name,t_class,t_score):
        person.__init__(self,number,name)
        self.setClass(t_class)
        self.setScore(t_score)
    def setClass(self,t_class):
        self.t_class = t_class
    def setScore(self,t_score):
        self.t_score = t_score
    def getClass(self):
        return self.t_class
    def getScore(self):
        return self.t_score
    def show(self):
        person.show(self)
        print('班级为：',self.getClass(),'成绩为：',self.getScore())
class teacher(person):
    def __init__(self,number,name,profession,department):
        person.__init__(self,number,name)
        self.setProfession(profession)
        self.setDepartment(department)
    def setProfession(self,profession):
        self.profession = profession
    def setDepartment(self,department):
        self.department = department
    def getProfession(self):
        return self.profession
    def getDepartment(self):
        return self.department
    def show(self):
        person.show(self)
        print('职称为：',self.getProfession(),'部门为：',self.getDepartment())

zs = student(2016006408,'郝延焘','软件1627',80)
zs.show()
fy = teacher(100001,'方昀','教授','软件学院')
fy.show()

编号为： 2016006408 姓名为： 郝延焘
班级为： 软件1627 成绩为： 80
编号为： 100001 姓名为： 方昀
职称为： 教授 部门为： 软件学院
```



##### 13.字符串

字符串方法

`find() rfind()` 找出首次、最后一次出现位置，不存在返回-1

`index() rindex()` 找出首次、最后一次出现位置，不存在抛出异常

`count() 统计出现次数`

`split() rsplit()` 切割成列表，默认空格分开，maxsplit=10

`partition() rpartition()` 分隔成元组，分为三部分

`join() split()` join参数为列表，将列表中多字符用指定连接符成字符串，split将字符串切成列表。

`lower() uppper()` 字符串转大小写

`replace(A,B)` B替换为A

`maketrans() translate()` 先生存字符映射表，再根据表中关系替换字符。

`strip() rstrip() lstrip()` 删两端、右、左空格

`center() ljust() rjust()` 排版

**bool**

`startwith() endswith()`

`isalnum() isalpha() isdigit() isspace() isupper() islower()`

`in`

