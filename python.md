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
and or not #逻辑运算符
++i # 正正为正
```

##### 3.类型转换

```python
bin(555) 二
act(555) 八
hex(555) 十六

ord('a') 编码unicode
chr(65) 对应字符

list(range(5))
tuple(range(5))
dict(zip('2134','abcds'))
set('1234')

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
		1.只可访问一次 next(g) \g.__next__()
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

```python
append() #尾部加一个
insert() #指定位置加
extend() #尾部加多个
pop() #尾部删
remove() #删除一个出现
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

```python
add(2)
pop() #随机删
remove() #删指定
集合运算
```

##### 8.序列解包

列表

字典

enumerate对象

filter对象

zip对象

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



