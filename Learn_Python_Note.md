##Python笔记（一）

@(Python)

- **1.元组与列表的区别及元组的一些小细节**
```python
type((1)) 	 	 #这里显示为int类型，而不是tuple类型，这里把里面的“（）”即括号当作是数学运算中的括号
type(('hello'))  #这里显示为str类型，而不是tuple类型
type(["hello",1,3,False]) #显示为list类型
type((False,"hello",1,4)) #显示为tuple类型
#注意：方括号为list类型，而圆括号为tuple类型
type((1,))       #表示只有一个元素的元组
type(("hello",)) #表示只有一个元素的元组
type(())		 #这里显示为tuple类型
type([1])		 #这里显示为list类型
# str,list,tuple都为序列，其许多相应操作都类似
```

- **2. 集合**
```python
type({1,5,3,7})
#"{}"表示集合，集合的几个特性
# （1）无序（因此不是序列，不可以通过下标标识来取某个元素，也不可以切片）
# （2）不重复

#空集合
set()
type(set()) #这里显示为set类型

#集合的几种特殊操作：
>>> {1,23,4} - {1}  #两个集合的差集
{4, 23}
>>> {123,5,6,7} & {34,6,8,5} #两个集合的交集
{5, 6}
>>> {7,5,3,6} | {5,1,8,2} #两个集合的并集
{1, 2, 3, 5, 6, 7, 8}
```
- **3.  字典**
```python
>>> {'lbj': '詹姆斯', 'kg': '加内特', 'kb': '科比'}
{'lbj': '詹姆斯', 'kg': '加内特', 'kb': '科比'}

#字典的典型操作：通过key值取value值操作
>>> {'23': '詹姆斯', 5: '加内特', 'kb': '科比',23:'乔丹'}['23']
'詹姆斯'
>>> {'23': '詹姆斯', 5: '加内特', 'kb': '科比',23:'乔丹'}[23]
'乔丹'
#注意：（1）key值不能重复；
#	  （2）23是整数值，‘23’是字符串值，因此这两者当作key时是两个不同的key值；
#	  （3）当字典中有两个相同的key值，通过该key值取出来的是后面对应这个value值
```

- **4. 不要使用关键字和保留字作变量名**
```python
>>> type=1
>>> print(type)  #正确
>>> type(1)      #报错
#这样会报以下错误：
Traceback (most recent call last):
  File "<pyshell#15>", line 1, in <module>
    type(1)
TypeError: 'int' object is not callable
#错误即为：不可以把int对象当作函数调用，你已经把type定义为int类型的变量了，所以当你要使用type函数时就会报错。这里的type(1)就相当于1(1),这显然不是一个正确的函数调用。
```

- **5. 值类型和引用类型**

**注意:** int 、str 、tuple(元组)为值类型（*不可改变*），list、set、dict为引用类型（*可改变*）。

```python
>>> a=1
>>> b=a
>>> a=3
>>> print(a,b)
3 1			#a(int类型)为值类型（不可改变）， b不随a变化而变化，值仍为1。

>>> a=[1,2,3]
>>> b=a
>>> a[2]='hello'
>>> print(a,b)
[1, 2, 'hello'] [1, 2, 'hello']
# a（list类型）为引用类型(可改变)，b的值随a的改变而改变。
```
注：
```python
>>> a='hello'
>>> a=a+'python'
>>> print(a)
hellopython
>>> b='hello'
>>> id(b)
87241952	
>>> b=b+'python'
>>> id(b)
82488640

#比较两次id(b)的变化可以知道，执行完b=b+'python'后，b是一个新的字符串了（相当于形成的新字符串存储在新的存储位置，而不是在原字符串的存储位置上进行修改，原字符串并未改变），不是原来的字符串了，因此值类型不可改变。
```
以下代码也进一步说明了值类型不可以改变：
```python

>>> 'python'[0]
'p'
>>> 'python'[0]='a'
#报如下错误：
Traceback (most recent call last):
  File "<pyshell#9>", line 1, in <module>
    'python'[0]='a'
TypeError: 'str' object does not support item assignment
```
- **6.  列表的可变与元组的不可变**
```python
#列表的可变
>>> a=[1,2,3]
>>> id(a)
95224576
>>> hex(id(a))
'0x5ad0300'
>>> a[0]='hello'
>>> id(a)
95224576	#修改列表中某个元素的值后，该列表的id依然未变，说明列表的可变性。
>>> print(a)
['hello', 2, 3]

#元组的不可变
>>> a=(1,2,3)
>>> a[1]='hello'
#当对元组中的元素进行修改时，会报以下错误（说明元组的不可变性）：
Traceback (most recent call last):
  File "<pyshell#7>", line 1, in <module>
    a[1]='hello'
TypeError: 'tuple' object does not support item assignment
```

列表可以利用append函数添加元素
```python
>>> b=[1,2,3]
>>> b.append(4)
>>> print(b)
[1, 2, 3, 4]
```
而元组不可以利用append函数添加元素，因为元组不可变
```python
>>> c=(1,2,3)
>>> c.append(4)

#报错
Traceback (most recent call last):
  File "<pyshell#12>", line 1, in <module>
    c.append(4)
AttributeError: 'tuple' object has no attribute 'append'
```
访问元组中的列表的元素（类似于多维数组）
```python
>>> a=(1,2,3,[7,8,['hello',9,4]])
>>> a[3][2][0]
'hello'		#以类似于三维数组的方式访问到‘hello’

>>> a[3][2][0]='python'  #把‘hello’改成python
>>> print(a)
(1, 2, 3, [7, 8, ['python', 9, 4]])
#这里把‘hello’改成了‘python’,
#注意：这里之所以可以改变是因为这里改变的是列表，而不是元组。
```
- **7. 逻辑运算符**

（1）对于int、float类型，0被认为False,非0被认为True；
（2）对于字符串（str）类型，空字符串被认为False,非空字符被认为是True；
（3）对于列表，空列表被认为是False，非空列表被认为是True；（元组、集合、字典则与此类似）

```python
>>> 'a' and 'b'
'b'		#相当于True
>>> ' ' and 'b'  
'b'
#注意：' '里面是空格，空格也是字符串，所以' '是非空字符串，相当于True，当第一个元素为True时，则整个与运算的结果由第二个元素决定，因此返回第二个元素。
>>> '' and 'b'
''     	#相当于False
#当第一个元素为False时，则整个与运算的结果必定为False，因此返回第一个元素。
>>> 'a' or 'b'
'a'
>>> '' or  'b'
'b'
#当第一个元素为True时，则整个或运算的结果必然为True，因此返回第一个元素；
#当第一个元素为False时，则整个或运算的结果由第二个元素决定，因此返回第二个元素。
```
- **8. 成员运算符**

*注意：字典中使用成员运算符，是针对 key而言，而不是value*

```python
>>> 'lbj' in {'lbj':23}
True
>>> 23 in {'lbj':23}
False
```
- **9. 身份运算符**

注意：关系运算符比较的是*值的关系*,而身份运算符比较的是身份的关系（即存储地址是否相同）。

```python
>>> a=1
>>> b=1.0
>>> a==b
True	#值相等
>>> a is b
False	#身份相同（即存储地址相同）
#通过id函数可知1和1.0两者存储的位置不同（即身份不同）
>>> id(a)
1723716656
>>> id(b)
87465824
```
关系运算符和身份运算符在集合中的应用
```python
>>> a={1,2,3}
>>> b={2,1,3}
>>> a==b 
True	#集合是无序的，只要元素相同，则两个集合的值是相等的。
>>> a is b
False	#两者的身份不同（即存储的地址不同）
```
关系运算符和身份运算符在元组中的应用
```python
>>> c=(1,2,3)
>>> d=(3,2,1)
>>> c==d
False	#注意：因为元组属于序列，是有序的，顺序不同则两者的值是不同的。
>>> c is d
False	#两者的身份不同（即存储的地址不同）

#以下证明了元素属于序列
>>> (1,2,3)[1]
2
```
- **10. 判断变量的值、身份与类型**

```python

>>> type('hello')==int
False
>>> type('hello')==str
True
```
注：虽可以通过type函数加关系运算符来判断变量的类型，但是不推荐使用这种方法，因为type不可以判断变量的子类型。

推荐使用*isinstance*函数来判断变量的类型：
```python
>>> isinstance('hello',str)
True	#‘hello’是str类型
>>> isinstance('hello',(int,str,tuple))
True	#‘hello’是int,str,tuple三种类型中的一种。
>>> isinstance('hello',(int,list,tuple))
False	#‘hello’是int,list,tuple三种类型中的一种。
>>> 
```
- **11. 位运算符**

无论数字是否是二进制（如八进制，十进制等），都会被转化为二进制进行运算。
‘&’（按位与）
‘|’（按位或）
‘^’（按位异或）
'~'（按位取反）
'<<'（左移动）
'>>（右移动）
- **12. 表达式**
表达式（Expression）是运算符（operater）和操作数（operand）所构成的序列。
```python
>>> a=1
>>> b=2
>>> c=3
>>> a+b*c
7
>>> a or b and c
1	#结果为1的原因：and的优先级高于or，所以先执行b and c得到结果为3，再执行a or 3得到结果为1。相当于 a or (b and c)

# not and or的优先级的顺序是not>and>or.
```
运算符优先级（注：同一行的优先级没有进行编排）
![Alt text](./2015514101804370.jpg)

```python
>>> a=1
>>> b=2
>>> c=3
>>> not a or b+2 ==c
False
>>> (not a) or ((b+2)==c)
False

#两者是等价的，注意：关系运算符的优先级大于逻辑运算符的，所以'=='的优先级大于not。
```
- **13. break 与continue的区别**

 break:
```python
 A = [1,2,3]
 for x in A:
     if x == 2:
         break     
         #只输出1，后面的2，3以及else中的end字符全都不输出。
     print(x)
 else:
     print("end")
 #结果为：1
```


continue:
```python
A = [1,2,3]
for x in A:
    if x == 2:
        continue    
        #跳过2，不输出，其他的（如1，3，end字符）都输出。
    print(x)
else:
    print("end")

#结果为：1
#	    3
#	   end
```
- **14. 在python中实现类似于其他语言的for(int i=0;i<10;i++)的效果**
```python
#python中的for循环主要用来对序列或集合、字典进行遍历或循环
for target_list in expression_list:
    pass
else:
    pass
```
for与range
（1）顺序：
```python
#类似于其他语言的for(int i=0;i<10;i++)

for x in range(0, 10, 2):
    print(x, end=' | ')
#注意：这里不会取到10,‘2’表示间隔量
```
输出结果为：0  |  2  |  4  |  6  |  8  | 
（2）逆序：
```python
for y in range(10, 0, -2):
    print(y, end=' | ')
#注意：这里不会取到0,‘2’表示间隔量
```
输出结果为：10  |  8  |  6  |  4  |  2  | 
- **16. 两种方法实现按某种要求对序列进行访问**

下面两者实现效果相同，
(1)法一：使用for循环方式
```python
A = [1, 2, 3, 4, 5, 6]
for i in range(0, len(A), 2):
    print(A[i])
```
(2)法二：使用切片方式（python的特色）
```python
A = [1, 2, 3, 4, 5, 6]
B = A[0:len(A):2]   #注意这里的‘2’表示步长（即间隔大小）
print(B)
```
- **17. 导包问题**

t文件夹下的c1文件：
```python
#利用__all__=['','']和‘*’可实现选择性导入变量
__all__ =['a','b']

a = 1
b = 2
c = 4
```
与t文件夹在同级目录下的文件：
```python
import t.c1 as m
print(m.a)

#引入变量
from t.c1 import a
print(a)
 
#引入模块
from t import c1
print(c1.a)

from t.c1 import *
print(a)
print(b)
print(c) #会报错，因为__all__=['a','b']并未包括c.

```
- **18. 包的循环引入问题（比较隐蔽）**

p1.py文件的代码:
```python
#p1文件引入p2文件
from p2 import b
a = 1
print(b)
```
p2.py文件的代码：
```python
#p2文件又引入p1文件
from p1 import a
b = 2
print(a)
```
（1）p1文件引入了p2文件，而p2文件则又引入了p1文件，这就导致了循环引入现象，这是一个很隐蔽的错误。
（2）当然也可以是多个文件的循环引入，这发现起来更加困难。所以引入包时要尽量避免循环引入包。

- **19. 入口文件和普通模块**

t文件夹下的c4.py代码如下
```python
'''
This is a t.c4 doc.

'''

print('name:  '+__name__)
print('package:   '+__package__)
print('doc:   '+__doc__)
print('file:   '+__file__)
#print('import:   '+__import__)
```
seven文件夹下的c9.py文件代码如下：（注：c9.py和t文件夹位于seven中的同一级目录）
```python
'''
This is a seven.c9 doc
'''
import t.c4

print('-----------------c9.py is below---------------')
print('name:  '+__name__)
print('package:   '+(__package__  or '当前文件不存在任何包'))
print('doc:   '+(__doc__  or '当前文件不存在文档注释' ))
print('file:   '+__file__)
```
__注意入口地址的输入方式不同所引发的变化：__
图一：
![Alt text](./TIM截图20180109160709.png)
图二：
![Alt text](./TIM图片20180109162322.png)

注：c4文件是普通模块，c9是入口文件，入口文件和普通模块在内置变量方面是存在一定的差异的。

注二：
__(1)利用‘python -m’把可执行文件当作一个模块来运行（把入口文件当作普通模块来执行，不过应注意该指令执行的路径是基于该普通模块的包的上一级目录）。__
__(2)如果作为一个普通模块必须有一个包的。__

![Alt text](./TIM图片20180109170437.png)
'python -m'指令后面接的是一个命名空间加一个模块名称，而不是文件路径。


- **20. 判断这是入口文件还是普通文件**

```python
#判断这是入口文件还是普通文件
if __name__ == '__main__':
    print('This is a entrance document!')
else:
    print('This is a module!')

```
- **21. 相对导入和绝对导入（难点）**

项目文件结构
![Alt text](./TIM图片20180109194836.png)

__（1）__利用相对导入方式导入上一层目录结构下的m1.py模块：
package2\m1.py文件代码如下:
```python
'''
 This is m1.py doc
'''
m_1 = 1
print('m1’s package:  '+__package__)
```
package2\pacakge4\m2.py文件代码如下:
```python
'''
 This is m2.py doc
'''
from ..m1 import m_1 #引入上一层目录下的m1.py(利用相对导入的方式)
m_2 = 2
print('m2’s package: '+__package__)
```
LearnPython\main.py代码如下：
```python
import package2.package4.m2
print('main’s package: '+str(__package__))
```
输出结果如下：
![Alt text](./TIM图片20180109201813.png)

__注：__

（1）"__from__ xxx __import__ xxx"这样的方式才可以使用相对导入，"__import__ xxx"不可以使用相对导入的方式。

（2）绝对导入必须从顶级包开始，如这里的import     package2.package4.m2就是采用的绝对导入，package2为顶级包。

__(2)__ LearnPython\main.py代码如下:
```python
from .package2.package4.m2 import m_2
print('main’s package: '+str(__package__))
```
结果会报如下错误：
![Alt text](./TIM截图20180109200401.png)

注意：

（1）一个模块的顶级包与main.py(入口文件)所在的层级有关。

（2）main.py是不属于如何包的，这里的LearnPython不是顶级包。

（3）package2\package4\m2.py的顶级包是package2而不是LearnPython，LearnPython并不是顶级包。

__（3）__LearnPython\main.py代码同（2）所示:

但是如果利用 'python -m'指令把main.py不当作入口文件执行，而当作普通模块来执行，则不会报错。
结果执行如下所示：

![Alt text](./TIM截图20180109203058.png)

__(4)__ package2\package4\m3.py代码如下：
```python
from ...m5 import m_5 #引入上上级目录(即到了LearnPython目录)下的m5.py模块
m_3 = 3
```
LearnPython\m5.py代码如下：（注意：m5.py和main.py是在同一级目录下）
```python
m_5 = 5
print('m5’s package:  '+__package__)
```
LearnPython\main.py代码如下:
```python
from package2.package4.m3 import m_3
print('main’s package: '+str(__package__))
```
__运行时会报如下错误：*超越顶级包错误*__
![Alt text](./TIM截图20180109204804.png)
__*注意：*相对导入一定不可超越顶级包。__（这里的LearnPython并不是顶级包，package1，package2，package3才是顶级包。）

- **22. 函数中的关键字参数、默认参数和可变参数**

__（1）关键字参数：__
```python
'''
关键参数
默认参数
'''
def damages(skill1, skill2):
    damages1 = skill1*3
    damages2 = skill2*10
    return damages1, damages2
skill1_damages,skill2_damages = damages(skill2 = 2, skill1 = 10)   
 #这里括号里面的就是两个典型的关键字参数，
 #使用关键字参数的一个突出特点就是不用按形参顺序引入实参。
print(skill1_damages, skill2_damages)

```
__(2)默认参数：__
```python
def information(name, gender='男', age='25', school='八一小学'):
    print('姓名： ' + name)
    print('性别： ' + gender)
    print('年龄： ' + str(age))
    print('学校： ' + school)
print('------------------------------------')
information('张三', '男', 16, '光明小学')
print('------------------------------------')
information('李四')
print('------------------------------------')
information('赵六', '女', 16)
print('------------------------------------')
information('王二', age=18, school='红旗小学')   #修改王二的年龄和学校
```
__注意：__

（1）不能出现有默认参数和没有默认参数的混在一起，没有默认参数的形参必须放在前面：
         如def information(name, gender='男', age='25', school='八一小学'，teacher)
         
（2）实参书写时要注意按顺序进行一一对应；
       如只要修改‘王二’的年龄，print('王二',18)这样书写是错误的；
      应这样书写（使用关键字参数,这样就不用按顺序引入实参了）：print('王二',age='20')

__(3) 可变参数（*param）__
```python
#可变参数
def demo(*param):
    print(param)
    print(type(param))

demo(1,3,4,6,8)
```
运行结果：（可知可变参数是元组类型的。）
![Alt text](./TIM截图20180109235755.png)

```python
def demo(*param):
    print(param)
    print(type(param))
    
demo((8,9,7,10))

demo((1,3,4,6,8),'hello')

a = (9,4,5,6)
demo(a)

b = (8,3,6,8) 
demo(*b)	#实参里面加‘*’相当于序列解包。要传递元组，调用时需加上‘*’
```
运行结果：
![Alt text](./TIM截图20180110000949.png)

__注意必须参数、关键字参数、默认参数和可变参数混合的几种情况：__
```python
#必须参数必须放在可变参数之前
#param为必须参数，param2为默认参数，param为可变参数
def demo(param1,param2 = '4',*param): #可变参数放在默认参数之后
     print(param1, param2, param)

demo(1,2,3,4)
```
运行结果：（一 一 对应，多余的全部当作元组对应于可变参数）
![Alt text](./TIM截图20180110002425.png)

```python
def demo(param1,*param,param2 = '4'): #可变参数放在默认参数之前
     print(param1, param2, param)

demo(1,2,3,"hello")
```
运行结果:
![Alt text](./TIM截图20180110003452.png)

要实现使‘hello’对应于默认参数可以利用关键字参数实现：
```python
def demo(param1,*param,param2 = '4'):
     print(param1, param, param2)

demo(1,2,3,param2 ="hello") #利用关键字参数方法（param2 = 'hello'）把默认参数的实参值指定为‘hello’
```
运行结果：
![Alt text](./TIM截图20180110003939.png)

- **23. 关键字可变参数**

```python
格式：
def demo(**param):
    pass
    
(1)这样在调用时可以传递多个关键字参数，此时python会将其转化为字典类型dict。

(2)若调用时想传递字典类型而不转化为多维数组，调用时需加上**。

a={'bj':'32c','sh':'31c'}
demo(**a)

(3)遍历字典类型数据方法：(注：param为关键字可变参数即**param)

```python
for key,value in param.items():
    print(key, ':', value)
```
例如：
```python
def city_weather( **param1):
    print(param1)
    print(type(param1))
    for key,value in param1.items():
        #print(key+' : '+value) 这种输出方法也可以,输出效果与下面相同
        print(key, ':', value)
        
city_weather(bj='10c', sh='20c', nc='18c')

a = {'nb':'15c', 'hz':'16c', 'ja':'19c'}
city_weather(**a)
```
运行结果：
![Alt text](./TIM截图20180110214951.png)

- **24. 全局变量和局部变量**


```python
def demo():
    c = 20
    # a = '' 在其他语言要加上这行，才能在for循环外面引用a变量，
    # 而Python不用加这行也可以在for循环外面引用a变量，因为python没有块级作用域（for,while）的概念
    for i in range(0,6):
        a = 'abc'
        c += 1
    print(c)
    print(a)
demo()
```
注意：

（1）Python和其他语言的一个重要区别：Python没有块级作用域的概念！！！

（2）函数内部for循环外部可以引用for循环内部的变量

（3）python中可以在for循环外部访问for循环内部定义的变量，if-else和while也是如此!!因为在python里for、while和if-else不能形成作用域。所以其中的变量视作与函数内变量同一级别。代码块无法形成一个作用域

- **一个问题：为何全局变量不能在函数内部的for循环内部使用**
```python
c = 50
def demo():
    #c = 20
    for i in range(0,6):
        c += 1
    print(c)
demo()
```
c变量不是全局变量吗？为什么还会报“局部变量引用前应该先赋值”的错误？
- **25. global关键字的使用（将局部变量转变为全局变量）**

```python
def demo():
    global c
    c = 2
demo()   #注意不要忘了调用函数！！！
print(c) #c变成全局变量之后，函数外部也可以引用该变量。
#结果将打印c的值
```
注：使用import 导入该模块后，其他模块也可以使用该模块的全局变量。

- **26. 类和对象（重点和难点）、面向对象思想**
```python
'''
（1）有意义的面向对象的代码
（2）类 = 面向对象
（3）类、 对象
（4）实例化（类和对象就是通过实例化关联起来的；）
（5）类基本的作用是封装代码
（6）在类的内部不能执行类中的方法，类只负责定义，类的方法必须在类的外面调用执行
（7）不推荐在同一模块下面即定义类，又调用执行类中的方法，类的定义和调用类中的方法最好放在不同的模块中。
（8）类经过实例化后得到一个具体的对象（类就像一个打印机，通过一个打印机可以产生各种各样的对象）
（9）可以把类中的数据成员（变量）当作类的特征、把类中的函数当作类的行为，在类中，行为与特征是两大要点。
（10）类的定义：类似现实世界或思维世界中的实体在计算机中的反映，它将数据以及这些数据上的操作封装在一起。
'''

class Student():
    name = 'lee'
    age  = 18
    def stu_information(self):  #类中的函数定义一定要加‘self’关键字
        print('姓名： ' + self.name)  #在引用类中的变量时也需要在变量之前加上‘self’关键字(如self.name)
        print('年龄： ' + str(self.age))


#不推荐在同一模块下面即定义类，又调用执行类中的方法，类的定义和调用类中的方法最好放在不同的模块中。

# student = Student() #类的实例化
# student.stu_information() #调用类中的方法
```

- **27. 构造函数**

（1）构造函数只能返回None类型，不能定义返回其他类型。

（2）构造函数通常用于初始化对象的属性。
```python
class Student():
    name = 'lee'
    age  = 18
    def __init__(self,name,age):
        #构造函数
        #构造函数初始化对象的属性
        self.name = name  #等号左边为定义的实例变量，等号右边表示要传的参数。
        self.age = age

student = Student('kg',15)     #在类的实例化的时候，就调用了一次构造函数。注意：实例化时一定要记得加上与构造函数的形参相对应的实参。
a = student.__init__('kb',36)      #构造函数也可以显式调用，如student.__init__('kb',36)
print(a)
print(type(a))
print(student.name)
print(student.age)
```
运行结果：
![Alt text](./TIM截图20180111182105.png)

构造函数初始化对象的属性的方式：
```python
def __init__(self,name,age):
        self.name = name 
        self.age = age
```

- **28. 类变量和实例变量的区别（重点与难点）**

（1）类变量是与类相关的变量，不受对象的影响；

（2）实例变量是与对象相关的变量，受对象的影响；

```python
class Student():
    #类变量
    name = 'lee'
    age  = 18
    #类变量：与类相关的变量，实例变量：与对象相关的变量
    def __init__(self,name,age):
        #实例变量
        #这里的self后面的name是实例变量，通过"self.实例变量名"的方式定义实例变量
        self.name = name  #等号左边为定义的实例变量，等号右边表示要传的参数。
        self.age = age

student1 = Student('王二小',16)
student2 = Student('刘胡兰',20)
print(student1.name)
print(student2.name)    #实例变量与对象相关，不同对象的实例变量的值不同
print(Student.name)     #类变量只与类有关，与对象无关
```
运行结果：
![Alt text](./TIM截图20180111185959.png)

- **29. 类与对象的变量查找顺序**

（1）错误的实例变量定义方式导致的后果：
```python
class Student():
    #类变量
    name = 'lee'
    age  = 18
    #类变量：与类相关的变量，实例变量：与对象相关的变量
    def __init__(self,name,age):
       #正确的实例变量的定义方法      
       # self.name = name  
       # self.age = age
       
       #这里使用错误的实例变量定义方法，这样就会导致对象不存在变量（即实例变量），所以显示出来的对象的属性字典（变量字典）为空。
        name = name
        age = age

#__dict__ : 类的属性（包含一个字典，由类的数据属性组成）

student1 = Student('王二小',16)
print(student1.__dict__)  #对象的属性字典
print(Student.__dict__)   #类的属性字典
```
运行结果：
![Alt text](./TIM截图20180111194359.png)
（2）正确的实例变量定义方法的运行结果为：
![Alt text](./TIM截图20180111193112.png)
（3）
```python
class Student():
    #类变量
    name = 'lee'
    age  = 18
    #类变量：与类相关的变量，实例变量：与对象相关的变量
    def __init__(self,name,age):
    #这里使用错误的实例变量定义方法，这样就会导致对象的实例变量表为空，所以当在该表中寻找某个实例变量时，是找不到的。
        name = name
        age = age

student1 = Student('王二小',16)
print(student1.name)  #对象的实例变量
print(Student.name)   #类变量
```
运行结果：
![Alt text](./TIM截图20180111195127.png)
__注意：__
__python寻找变量的机制：__
当python尝试去寻找一个实例变量时，首先在对象的实例变量表中寻找，如果没有找到则继续在类变量表中寻找，如果有父类，则继续父类的类变量表中寻找，而不是直接返回None。

- **30. self 与实例方法**

（1）python中类的实例方法的参数列表中需要有self，但是在调用时不需要传入。
在实例方法中也可以将self定义成别的名字（如this），但python建议为self。

（2）self  就是当前调用的某一个方法的对象
```python
#下面两种都可以称为实例方法
 def __init__(self,name,age):
   
        self.name = name 
        self.age = age

    def stu_information(self):  
        print('姓名： ' + self.name) 
        print('年龄： ' + str(self.age))
```

- **31. 在实例方法中访问类变量的方法以及在类外部访问实例变量的方法**

__(1)__ 在实例方法中访问实例变量:  self.变量名
__(2)__ 在实例方法中访问类变量:   
①类名.变量名  ②self.__class__.变量名
__(3)__ 在类外部访问类变量:    
①类名.变量名      ②实例.变量名
```python
class Student():
    #类变量
    sum1 = 0
    name = 'lee'
    age  = 18
    def __init__(self,name,age):
       
        self.name = name  #等号左边为定义的实例变量，等号右边表示要传的参数。
        self.age = age
        
        print(self.name) #打印的是实参变量的值
        print(name)      #打印的是传递的形参值
        print(Student.name) #打印的是类变量的值
        print('在实例方法中访问类变量的方法一：', Student.sum1) #打印的是类变量sum1的值
        print('在实例方法中访问类变量的方法二：',  self.__class__.sum1) #打印的是类变量sum1的值


student = Student('叮当',12)
print('在类的外部访问类变量的方法：', Student.sum1)   #在类的外部访问类变量
```
- **32. 类方法**

__(1)__ 类变量的使用的一个小例子：
每次把一个类实例化成对象，类变量sum1就加1
```python
class Student():
    #类变量
    sum1 = 0
    name = 'lee'
    age  = 18
    def __init__(self,name,age):
       
        self.name = name 
        self.age = age
        self.__class__.sum1 += 1
        print('当前学生人数为：',self.__class__.sum1)

student1 = Student('叮',11)
student2 = Student('当',12)
student3 = Student('叮当',13)

```
__(2)类方法定义和调用__

```python
'''
类方法
'''

class Student():
    #类变量
    sum1 = 0
    name = 'lee'
    age  = 18
    #实例方法
    def __init__(self,name,age):
       
        self.name = name 
        self.age = age
        
    #类方法
    @classmethod
    def plus_sum(cls):
        cls.sum1 += 1
        print(cls.sum1)

student1 = Student('叮',11)
#调用类方法：类名.类方法
Student.plus_sum()          #这里虽然可以使用对象调用类方法如"student1.plus_sum()"，
                            #但是不建议这样做，这样容易导致逻辑混乱。
student2 = Student('当',12)
Student.plus_sum()
student3 = Student('叮当',13)
Student.plus_sum()
```
注：
（1）这里的cls表示class类，也可以使用别的名字表示，但是建议使用cls这个名字作为参数。
（2）此外，类方法中操作类变量，直接使用‘cls.类变量’即可。而且python里对象也可以调用类方法。

- **33. 静态方法**

```python
'''
静态方法
'''
class Student():
    #类变量
    sum1 = 0
    name = 'lee'
    age  = 18
    def __init__(self,name,age):
       
        self.name = name 
        self.age = age
       
    #类方法
    @classmethod
    def plus_sum(cls):
        cls.sum1 += 1
        print(cls.sum1)
      # print(self.name)    #会报错：self没有定义。所以类方法中不能访问实例变量
        print(name)         #去掉self也会报错：name没有定义。且去掉name就不是访问实例变量了。
    
    #静态方法
    @staticmethod
    def add(x, y):
        print(Student.sum1)     #静态方法可以访问类变量，不过只能使用“类名.类变量”的方式
      # print(self.name)        #会报错：self没有定义。所以静态方法中不能访问实例变量
        print(name)             #去掉self也会报错：name没有定义。且去掉name就不是访问实例变量了。
        print('This is a static method!')   

student1 = Student('叮',11)
#调用类方法：类名.类方法
Student.plus_sum()       

#调用静态方法（类和对象都可以调用静态方法）
student1.add(1, 3) #通过对象调用静态方法
Student.add(2, 4)  #通过类调用静态方法
```
__注：__

（1）静态方法无需传入类似self，cls之类的参数，而且它可以被对象和类调用。静态方法可以访问类变量。

（2）静态方法和类方法都不可以访问实例变量。

（3）尽量少用静态方法，因为它和类与对象的关系很弱，和普通函数区别不大。当某个方法比较纯粹，与类和对象的关系不是很强，可以考虑静态方法。

- **34. 成员的可见性：公开与私有**

（1）改变类下面的变量的值最好用方法，不要直接对变量进行更改。
```python
class Student():
    #类变量
    sum1 = 0
    name = 'lee'
    age  = 18
    def __init__(self,name,age):  
        self.name = name 
        self.age = age
        self.score = 0
        
    def  marking(self, score):
         if score < 0 :
             score = 0
             return '不能打负分！'
         self.score = score
         print(self.name + '同学的分数为：'+ str(self.score))
      
student1 = Student('叮',11)
#student1.score = -1    #修改类下面的变量不应该直接对变量进行更改。
result = student1.marking(66)  #而应该通过方法进行更改。
print(result)
```
（2）为了防止直接对类下面的变量进行更改，可以通过成员的可见性进行控制，将成员设置成私有即可。

```python
 #私有方法
    def __func(self,param):
        self.__param = 0 #私有变量：变量名之前加双下划线
```
__注：__

(1) python中定义__私有变量或私有方法__只需在名称前加__双下划线__即可，__但__若在__名称结尾__也加了双下划线则不是私有的。

(2) 应尽量避免定义开头和结尾都有双下划线的变量或函数名称。

__(3)__ __实例私有变量__
```python
'''
成员可见性:公开和私有
'''
class Student():
    #类变量
    sum1 = 0
    name = 'lee'
    age  = 18
    def __init__(self,name,age):
       
        self.name = name 
        self.age = age
        self.__score = 0    #类中的score实例变量已被定义为私有实例变量
    def  marking(self, score):
         if score < 0 :
             score = 0
             return '不能打负分！'
         self.__score = score
         print(self.name + '同学的分数为：'+ str(self.__score))

student1 = Student('叮',11)

result = student1.marking(66)  
print(result)

student1.__score = -1    #这里的__score便不是上面的私有类中的实例变量__score,这是两回事。这是一个新的实例变量。
print(student1.__score)
print(student1.__dict__)

student2 = Student('王二小',14)

print(student2.__score)  #会报错，因为Student的__score是私有实例变量，在类的外部是访问不到的。
```
运行结果如下：
![Alt text](./TIM截图20180112222846.png)
将上面的
```python
print(student2.__score)
```
改为
```python
print(student2._Student__score)  #注意：Student前面是单下划线
```
是可以访问到私有实例变量的。

- **35. 继承（重点和难点）**

父类文件
```python
'''
继承
这是父类文件
'''
class Human():
    sum1 = 0
    def __init__(self, name, age):
        self.name = name
        self.age = age
    def do_work(self):
        print('This is a parent method !')
```
子类文件：
```python
'''
继承
这是子类文件
'''
from n10 import Human

class Student(Human):
     
     def __init__(self, school, name, age): #子类的构造函数要引入父类的构造函数中的参数
         self.school = school
         #Human.__init__(self, name, age)  #注意：使用类调用父类的构造函数需要引入self参数。因为这就是一个普通的方法调用，没有人帮忙把self参数传入进去，所以需要把所有的参数传入进去。
          super(Student,self).__init__(name,age) #推荐使用这种方法调用父类中的构造函数，这种方法也可以用于调用父类中的实例方法。
     def do_work(self):
         super(Student, self).do_work()   #调用父类中的实例方法
         print('Studying!!! ')
student1 = Student('八一小学','王二小',18) #而这里之所以不用传入self参数，是因为在实例化的时候Python自动把self参数传入了。
print(student1.sum1)    #通过对象访问类变量（继承自父类）
print(Student.sum1)     #通过类访问类变量
print(student1.school)  #通过对象访问实例变量(子类特有的)
print(student1.name)    #通过对象访问实例变量（继承自父类）
print(student1.age)
student1.do_work() 
#而这里之所以不用传入self参数，是因为这里是通过student1对象来调用方法的，self在这里就是指代student1对象，调用会自动把student1这个对象传入进去
```
__调用父类中的构造函数(两种)：__
方法一：
```python
Human.__init__(self, name, age) 
```
方法二：
```python
super(Student,self).__init__(name,age) #推荐使用这种方法调用父类中的构造函数，这种方法也可以用于调用父类中的实例方法。
```
__调用父类中的实例方法:__
```python
super(Student, self).do_work()   #调用父类中的实例方法
```

__一个重要的对比及区别：__
(1) 使用类调用父类的构造函数需要引入self参数。因为这就是一个普通的方法调用，没有人帮忙把self参数传入进去，所以需要把所有的参数传入进去。
```python
 Human.__init__(self, name, age) 
```
(2)这里之所以不用传入self参数，是因为在实例化的时候Python自动把self参数传入了。
```python
student1 = Student('八一小学','王二小',18) 
```
(3)这里之所以不用传入self参数，是因为这里是通过student1对象来调用方法的，self在这里就是指代student1对象，调用会自动把student1这个对象传入进去
```python
student1.do_work() 
```
- **36. 正则表达式**
 
 正则表达式:是一个特殊的字符序列,用于检测一个与我们设定的字符序列是否相匹配.
如：
快速检索文本,实现一些文本替换.
检测一串数字是否是电话号码.
检测一个字符串是否符合 email 的标准
把一个文本里指定的单词替换成另外一个单词.
__(1)小试牛刀：__
```python
'''
正则表达式
'''
import re
a = 'c++|java|c|C#|javascript|python'

#（1）使用内置方法判断是否a变量中是否存在某个字符串
r = a.index('python') > -1   #大于-1说明存在
print(r)
#(2)用in关键字也可以判断a变量是否存在某个字符串
r1 = 'python' in a
print(r1)
#(3)通过正则表达式判断a中是否存在某个字符串
result = re.findall('python',a)   #返回为list
print(result)
if len(result) > 0:
    print('a中存在python这个字符串！')
else:
    print('a中不存在python这个字符串！')
```
__(2)正则表达式之字符集：__
```python
'''
正则表达式之字符集
'''
import re

s = 'abcd,haha,hell,appl,afcd,efdg,accd'

r1 = re.findall('a[bf]cd', s)  #查找第二个字符是b或f字母的字符串，且第一个字符为a,第三个为c,第四个为d
print(r1)
r2 = re.findall('a[b-f]cd', s) #查找第二个字符是b到f字母中的一个字母的字符串，且第一个字符为a,第三个为c,第四个为d
print(r2)
r3 = re.findall('a[^bf]cd', s) #查找第二个字符既不是b也不是f的字符串，且第一个字符为a,第三个为c,第四个为d
print(r3)

```
__（3）正则表达式之概括字符集__

常见的概括字符集：

__\d: __0-9的数字，等价于[0-9]

__\D:__ 非数字，等价于[^0-9]

__\w：__匹配包括__下划线__的任何单词字符。类似但不等价于“[A-Za-z0-9_]”，这里的"单词"字符使用Unicode字符集。

__\W：__匹配任何非单词字符。等价于“[^A-Za-z0-9_]”。

__\s：__匹配任何不可见字符，包括空格、制表符、换页符等等。等价于[ \f\n\r\t\v]

__\S：__匹配任何可见字符。等价于[^ \f\n\r\t\v]

__(4) 数量词__
(1)数量词可以控制正则表达式选取的字符的长度，但是感觉按照数量词从大到小进行匹配，遇到不符合表达式的字符就停止，到新的字符重新开始匹配。
因此，案例中每个单词后边因为有了空格、数字才使得{3,6}数量词成立，如果将这些非单词字符去掉，那么输出的就都是按照最大的数量词匹配出来的字符。

(2)而且数量词并不只是在匹配单词的时候可以用，数字也可以。

(3)python在进行正则表达式匹配时倾向于__贪婪__。在匹配字符串时若在结尾加上?则可进行__非贪婪__匹配。
```python
import  re
a = 'Java45python html&php'
#找出a中的字符个数在3到6个的单词
r1 = re.findall('[A-Za-z]{3,6}', a)  #贪婪：尽可能地按最大数量词匹配单词。（如这里的python是从起始字符p开始后面一直到第6个都是字符，所以都匹配选取出来。）
print(r1)
r2 = re.findall('[A-Za-z]{3,6}?', a) #非贪婪（后面加问号）：尽可能地按最小数量词匹配单词(如这里匹配到3个字符就结束了匹配)
print(r2)
```
运行结果：
![Alt text](./TIM截图20180113215857.png)
__（5）匹配0次、1次或无限次__

‘/*’  ：  0次或无限多次

‘+’ ： 1次或无限多次

‘?’ ： 0次或1次

```python
import  re
a = 'pytho0python1pythonn2'
#
r1 = re.findall('python*', a)  # *号表示*号前面的n字母出现0次或无限多次
print(r1)
r2 = re.findall('python+', a)  # +号表示+号前面的n字母可以出现1次或无限多次
print(r2)
r3 = re.findall('python?', a)  # ?号表示?号前面的n字母可以出现0次或1次
print(r3)    #这里输出的第三个元素是'python'

r4 = re.findall('python{1,2}', a)  #注意与?号的区别
print(r4)    #注意这里选取匹配时采用的是贪婪模式，所以输出的第二个元素是'pythonn'(多一个n)
```
运行结果：
![Alt text](./TIM截图20180113222757.png)
__（6） 边界匹配符__

^ : 起始边界符

$ : 结束边界符

```python
'''
正则表达式之边界匹配符
'''
import re
qq = '10005000'

r1 = re.findall('[0-9]{4,8}', qq) #这种方式排除位数小于4位的QQ号，但是不可以排除位数大于8位的QQ号，当大于8位时，会截取前8位
print(r1)

r2 = re.findall('^\d{4, 8}$', qq)     #筛选位数为4到8位的QQ号
print(r2)

#效果与上面相同
# r2 = re.findall('^[0-9]{4, 8}$', qq) #筛选位数为4到8位的QQ号
# print(r2)
```
__(7) 组__
__注：__
( )可以在正则表达式中使用__组__的概念,
( )里的元素是__且__关系，[ ]里的元素是__或__关系。
```python
import re

a =  'pythonpythonuythonpythonpython'

r1 = re.findall('(python){3}', a)  #表示Python以组的形式匹配 连续出现3次才算匹配成功
#返回的值（即r1的值）为['python'],若不存在连续3个python，则返回[]即空列表
print(r1)
```
__(8) 匹配模式__

```python
'''
正则表达式之匹配模式
re.I 表示匹配时忽略大小写
re.S 表示匹配时匹配所有字符包括换行符（\n）

.    表示匹配除换行符（\n）以外的所有字符
re.S 可以影响'.'的行为，平时'.'表示匹配除换行符之外的所有字符，
加入re.S匹配模式之后，'.'就可以匹配所有的字符（包括换行符）
'''
import re

a = 'javaC#\nPythonC++'

#r1 = re.findall('c#.{1}',a , re.I)        #忽略大小写匹配c#然后匹配任意一个字符（不包括'\n'即换行符）
r1 = re.findall('c#.{1}', a, re.I | re.S) #忽略大小写匹配c#然后匹配任意一个字符（包括'\n'即换行符）
print(r1)
```
运行结果：
![Alt text](./TIM截图20180114005723.png)
__（9）re.sub正则替换__

（1）re.sub('正则表达式','即将覆盖原有字符串的新字符串',"老字符串",该字符被替换的次数,正则表达式的附加条件)
（2）替换的次数（count）为0表示为无限替换，如果为1则表示第一次匹配成功后替换，之后不再匹配替换。
（3）如果定义一个函数后  替换位置可以换成函数
def convert(value):
    pass
r = re.sub("C#",convert,lanuage,0)
```python
import re

a = 'pythonjavaC#htmlC#javascriptC##c++'

re.sub('C#', 'Go', a, 3)      #只有把该函数返回值赋值给新的字符串才能使输出的结果发生改变，因为字符串是不可改变的。
print(a)

r1 = re.sub('C#', '__Go__', a, 1)    # count=0(默认值),也依然会把字符中的三个C#全部替换，设置成1则只替换第一个。（count=0表示无限次替换）
print(r1)                        

r2 = a.replace('C#','__Ruby__')   #如果只是简单进行一些字符的替换可以使用“字符串.replace()"的内置函数完成
print(r2)

def convert(value):
    print(value)      #注意观察value的值
    matched = value.group() #group()是获取当前匹配的内容
    return '!!_' + matched +'_!!'

r3 = re.sub('C#', convert, a, 3)
print(r3)
```
运行结果：
![Alt text](./TIM截图20180114153554.png)

__re.sub( )__中为何要引入函数参数，其一个原因就是引入函数可以写一些具体的逻辑，这是相比只能引入一些变量参数而言的一个极大的优势。
如下所示：
```python
import re

a = 'AB389408HE67632'

def convert(value):
    matched = value.group()
    if int(matched) >= 6:
        return '9'
    else:
        return '0'

r1 = re.sub('\d',convert ,a)  #将字符串中是数字的且大于6的全部转换为9，小于6的全部转换为0
print(r1)
```
__（10）match 和 search函数__

__match函数：__ 从字符串开始位置（即首字符）开始匹配，匹配成功则返回一个对象，否则返回None。

__search函数：__ 搜索整个字符串，找到第一个匹配的结果，则返回一个对象；如果没有一个符合条件的结果，则返回None。
__注：__

（1）可以使用group函数从对象取出符合的结果的内容。

（2）这两个函数一旦匹配到结果就立即返回，而停止匹配（这是与findall函数的最大区别）。

（3）findall函数返回结果为list类型，    match与search返回类型为对象（如果需要获取对象截取内容 需要用group( )方法）

```python
'''
正则表达式之match和search函数
'''
import re

s = 'WA6fj4a52f34hfu'

r1 = re.match('\d', s) # match函数是从第一个字符开始匹配，如果不符合条件就返回None，如果符合就返回包含该字符的对象。（如这里因为第一个字符为W，不是数字，所以返回None）
print(r1)

r2 = re.search('\d', s) # search函数是匹配选取出第一个包含符合条件的值的对象。（如这里为第一个符合的值为6）
print(r2)
print(r2.group())     #用group获取匹配的值的内容

r3 = re.findall('\d', s)   # findall函数是匹配所有符合条件的值，而search 和 match函数只匹配选取出第一个符合条件的值。
print(r3)
```
运行结果：
![Alt text](./TIM截图20180114161935.png)

__（11）group分组__

```python
'''
正则表达式之group分组
'''
import re 

s = 'life is short,i use python,i love python'

r1 = re.search('life(.*)python(.*)python',s)   #（.*）表示一个分组，括号表示一个分组。
print(r1.group(0))  # group(0)是一种特殊情况，返回的是正则表达式的完整匹配结果
print(r1.group(1))  # 如果要返回完整匹配结果中的第1个分组应使用group(1),表示匹配中间的第一个分组
print(r1.group(2))  # group(2)表示完整匹配结果中间的第2个分组
print(r1.group(0, 1, 2))  #可以使用这一行代码，返回上面三行代码的返回结果，返回的形式是以元组形式
print(r1.groups())  #用groups()返回完整匹配结果中间的所有分组，以字符串元组的形式返回
```
运行结果：
![Alt text](./TIM截图20180114170336.png)
注：
' .* '表示匹配所有字符（除换行符以外）无限次，常用于模糊查询（匹配）。

- **37. JSON**

__（1）__JSON全称为JavaScript Object Notation即JavaScript对象标记。
__（2）__JSON 是一种轻量级的数据交换格式 。
__（3）__符合JSON格式的字符串叫作 JSON字符串。
__（4）__JSON 的优势：
							1. 易于阅读
							2. 易于解析
							3. 网络传输效率高
					    	4. __适合跨语言交换数据__
- **38. 反序列化**

反序列化：将某种格式的（如JSON格式）数据类型转化为特定语言（如Python）的数据类型的过程
```python
import json
json_str = '{"name": "王二小", "age": 15, "girl": false}'  #注意：JSON数据格式中字符串用“双引号”括起来，JSON数据格式是与语言无关的，这点很容易造成错误。
json_array = '[{"name": "王二小", "age": 15, "girl": false}, {"name": "刘胡兰", "age": 18, "girl": true}]'
r = json.loads(json_str)  #使用json.loads()函数把json数据格式转化为Python相应的数据格式，
#（1）JSON中的{key:value,...}格式（即JSON的一个数据对象）对应于Python中的字典数据类型（dict）
#（2）JSON中的array（数组）如（[{"name":"王小二",...},{"name":"刘胡兰", ...}]）对应于Python中的列表类型（list）
print(type(r))
print(r)
print(r['name'])    #通过字典的访问数据项的形式就可以访问某个数据项的值
print(r['age'])    

r1 = json.loads(json_array)
print(type(r1))
print(r1)
```
运行结果：
![Alt text](./TIM截图20180114183848.png)

__JSON数据类型与Python数据类型的对应关系__

|  JSON  | Python |
| :-----:|:------:|
|object|dict|
|array|list|
|string|str|
|number|int|
|number|float|
|true|True|
|false|False|
|null|None|

- **39. 序列化**

__序列化：__特定语言下（如Python）的数据类型转化为某种数据格式的数据类型（如JSON）的过程。
```python
'''
序列化：特定语言下（如Python）的数据类型转化为某种数据格式的数据类型（如JSON）的过程
json.loads() 用于反序列化
json.dumps() 用于序列化
'''
import json
#python下的列表数据类型
student = [
            {'name':'熊大', 'age':16, 'male':True},
            {'name':'熊二', 'age':13, 'male':True}
          ]
 
r = json.dumps(student)   #序列化：这里Python下的列表数据类型（list）转化为JSON字符串,在JSON数据格式里就是array类型
print(r)
print(type(r))  # 转化之后的结果放在Python就是str类型
```
运行结果：
![Alt text](./TIM截图20180114192222.png)

- **40. 序列化和反序列化的区别**

(1) __反序列化：__将某种格式的（如JSON格式）数据类型转化为特定语言（如Python）的数据类型的过程
(2)__序列化：__特定语言下（如Python）的数据类型转化为某种数据格式的数据类型（如JSON）的过程
(3) __json.loads()__ 用于反序列化
(4) __json.dumps()__ 用于序列化

- **41. JSON理解的一些常见误区**

__（1）__ 应该跳出特定语言的范畴来理解JSON. (JSON并不是专属于特定语言里的，如JavaScript)

__（2）__ JSON和JavaScript一样，都是W3C标准script（ECMA Script）的实现方式之一。
由于JS是目前主流的Web前端语言，而JSON被大量用于JavaScript的交互层, 因此常被误解为附属于JavaScript。

__（3）JSON对象__：JSON本身没有对象的概念，通常这个问题是关联到javascript里时带出来的。

__（4）__ JSON有自己的数据类型（虽然和Javascript的数据类型有些类似），JSON更多的是作为不同语言传输数据的中间类型（即 JSON是一种__中间数据类型__）。

__（5） JSON字符串__：符合JSON数据格式的字符串。

__（6）REST服务__的标志性特点就是轻量，正好与JSON的特点相匹配，因此在REST服务中应该多采用JSON而不是XML

- **42. 枚举类型**

__(1)__ python中所有枚举类型都是enum模块下Enum类的子类。

__(2)__ 枚举中的标识最好全部使用大写。

```python
from enum import Enum   #从内置的enum模块下导入Enum类
class VIP(Enum):
    YELLOW = 1
    GREEN = 2
    BLCAK = 3
    RED = 4

print(VIP.YELLOW)   #输出的结果为VIP.YELLOW
```
__注：__

枚举的意义重在标签而不在于数值，使用print(VIP.YELLOW)打印结果不是1而是VIP.YELLOW，这也符合枚举的意义。

__(3)__ 枚举的__优点__：
1. 定义枚举类后不能从外部更改标签属性。(枚举类型中的值不能更改。)
```python
#这样并不能更改枚举类型变量的值，会报错。
VIP.YELLOW = 6
```
2. 可以防止相同标签的定义。
```python
#相同标签是不可以重复定义的
YELLOW = 1
YELLOW = 2
```
```python
#在枚举中YELLOW和GREEN的是可以相同的
#枚举类中不同枚举类型的值可以相同，但此时这两个枚举类型中的第二个名称是第一个的别名，建议使用第一个的名称+_ALIAS作为名称。
YELLOW = 1
GREEN = 1   #GREEN就相当于YELLOW的别名，并不是一个独立的枚举类型
```
__(4)__用字典类型和类表示枚举值的__缺点__：
1. 可变
```python
#改变类变量的值（假设name为Student类的类变量）
Student.name = 'wanger' #是可修改的（即可变）
```
2. 没有防止相同值的功能。如：
```python
{'YELLOW':1, 'GREEN':1} #字典中值相同是可以的
```
__（5）枚举类型，枚举名字，枚举值__
```python
'''
枚举类型，枚举的名字，枚举值
'''
from enum import Enum

class VIP(Enum):
    YELLOW = 1
    GREEN = 2
    BLACK = 3
    RED = 4

print(VIP.GREEN) #枚举类型,返回VIP.GREEN
print(type(VIP.GREEN)) #返回结果为<enum 'VIP'>

print(VIP.GREEN.name) #枚举名字（是str类型）,返回GREEN
print(type(VIP.GREEN.name)) #返回结果为<class 'str'>

print(VIP.GREEN.value) #枚举值
print(VIP['GREEN']) #表示通过枚举名字获取枚举类型，返回VIP.GREEN

#利用for循环遍历枚举类中的枚举类型
for v in VIP:
    print(v)

```
__（6） 枚举类型比较__

__(1).__ 枚举类型之间可以进行等值比较(==)，但直接和数值比较会返回False，如：
    VIP.GREEN == 2 返回False
__(2).__ 枚举类型之间不支持大小比较操作符(>、<)的.
__(3).__枚举类型可以进行身份比较(is)，如：
VIP.GREEN is VIP.GREEN 返回 True
__(4).__ 不同枚举类中的枚举类型进行比较都会返回False。
```python
from enum import Enum

class VIP(Enum):
    YELLOW = 1
    GREEN = 2
    BLACK = 3
    RED = 4
class VIP1(Enum):
    YELLOW = 1
    GREEN = 2
    BLACK =4
    RED = 3

r1 = VIP.GREEN == VIP.GREEN  #返回True，枚举类型之间可以进行等值比较
print(r1)

r2 = VIP.GREEN == VIP.YELLOW #返回False
print(r2)

r2_1 = VIP.GREEN == VIP1.GREEN #返回False，不同枚举类中的枚举类型进行比较时会返回False，即使两者的值相等
print(r2_1)

r3 = VIP.GREEN == 2    #返回False，因为枚举类型不可以直接与数字进行等值比较   
print(r3)

# r4 = VIP.GREEN > VIP.YELLOW #会报错，因为枚举类型不可以进行大小比较
# print(r4)

r5 = VIP.GREEN is VIP.GREEN #返回True，枚举类型可以进行身份比较（is）
print(r5)
```

__(7) 枚举的一些常见误区__
```python
from enum import Enum

class VIP(Enum):
    YELLOW = 1
    YELLOW_ALIAS = 1  #这是别名（值相同）
    BLACK = 3
    RED = 4

#利用for循环只能遍历枚举类中的枚举类型（不包括别名）
for v in VIP:
    print(v)

#别名也会被遍历，打印出枚举名字
for v in VIP.__members__:
    print(v) 

#以元组的形式显示
for v in VIP.__members__.items():
    print(v)
```
__注：__
枚举类中不同枚举类型的值可以相同，但此时这两个枚举类型中的第二个名称是第一个的别名，建议使用第一个的名称+_ALIAS作为名称。

__（8）由枚举类型的值获取枚举类型（枚举转换）__
```python
#通过数值获取枚举类型   枚举类名(枚举类型的值)
a = 1
print(VIP(a))
```
__（9）枚举的小结__

（1）python中枚举类是通过__单例__实现的，__无法实例化__。
（2）使用enum模块中的__unique装饰器__可以避免一个枚举类中出现__值相同__的枚举类型
（3）enum模块中的__IntEnum类__被继承后，定义的枚举类型的值只能为int类型
```python
from enum import IntEnum,unique
@unique
class VIP(IntEnum):
    YELLOW = 1
    GREEN = 1      #会报错：加了“@unique”的装饰器，枚举类型不能出相同的值。
    BLACK = 'str'  #会报错：继承了IntEnum类，枚举类型的值只能为int类型的
    RED = 4
```
- **43. 函数**

__(1)__ python中一切皆对象，函数也是对象。
__(2)__ python中的函数不仅可以赋值给变量，还可以作为另外一个函数的参数传递，也可以作为另外一个函数的返回结果。

- **44. 闭包（难点！！！！！）**

```python
1.python在函数内部还可以定义函数，但该函数作用域只在外部函数内部有效，除非作为外部函数的返回值被返回，在外部函数的外部用一个变量接收后就可以调用它了。
2.python中闭包的定义：由函数及其在定义时外部的环境变量（不能是全局变量）构成的整体。
闭包 = 函数 + 环境变量（函数定义时候）
3.f.__closure__ 返回环境变量，此时的环境变量为一个对象
  f.__closure__[0].cell_contents 返回环境变量的值
4.f()指调用函数
```
```python
def curve_pre():
    a = 25 #如果a = 25(闭包中的环境变量)放在curve函数中就不是闭包了，必须放在curve之外和curve_pre之内
    def curve(x):
        return a * x * x
    return curve
a = 10    #如果前面的a = 25被注释，则a取值为这里的10
#闭包中的环境变量a（即a=25）被调用，全局变量a（即a=10）不会被调用，没有才被调用.
f = curve_pre()
print(f(2))

print(f.__closure__) #返回环境变量，是一个对象
print(f.__closure__[0].cell_contents) #返回环境变量的值
```
运行结果：
![Alt text](./TIM截图20180115182205.png)


__（1）__闭包的意义在于保存了一个环境。尤其是函数中调用函数时，如果没有闭包，很容易被外部变量所影响。

__（2）__闭包必须满足2个条件，函数嵌套函数，并且内部函数需要引用外部函数的变量（环境变量）。

未构成闭包情况：
```python
def f1():
    a = 1
    def f2():
        a = 2  # 此时a被Python认为是一个局部变量，与外面的a是没有关系的，并没有去引用外面的环境变量，所以此时闭包是不存在的，且这与闭包的意义在于保存了一个环境。尤其是函数中调用函数时，如果没有闭包，很容易被外部变量所影响，而且这与f2()函数有没有返回结果没有关系.(此时a已经被重新赋值)
    return f2
f = f1()
f()
print(f)
print(f.__closure__)
```
运行结果：
![Alt text](./TIM截图20180115221220.png)
构成闭包情况：
```python
def f1():
    a = 1
    def f2():
        c = 2 * a  #闭包的环境变量不能在函数里被重新赋值
    return f2
f = f1()
f()
print(f)
print(f.__closure__)
```
运行结果：
![Alt text](./TIM截图20180115221905.png)

- **45.  旅行者行走问题解决方法：非闭包方法和闭包方法**

__（1）非闭包方法__
```python
'''
旅行者行走问题
（1）用非闭包的方法
'''
x = 0
def position(move):
    global x   #将 x 声明为全局变量，这样函数内部的x就会被全部当作全局变量，而不是局部变量
    new_pos = x + move
    x =new_pos    # 如果没有"global x"声明，这里就会把x当作局部变量，从而会报局部变量未被定义的错误。
    return x
print(position(3))
print(position(4))
print(position(6))
```
运行结果：
![Alt text](./TIM截图20180115230056.png)

__注：__
即便是此变量被赋值为全局变量，但如果在函数内部出现了变量赋值，即使出现在函数最后一条语句，这个变量也会被认为是局部变量，执行时不再往外找值，赋值前使用报未定义的错误。
解决办法之一是将变量赋值为global。
__（2）闭包方法__

闭包有保存现场的一个功能，有记忆性。
```python
'''
旅行者行走问题
（2）用闭包的方法
'''
origin = 0
def position(pos):
    def go(step):
        nonlocal pos  #定义pos为非局部变量，这里的pos是环境变量。从而形成闭包。
        new_pos = pos + step
        pos =new_pos
        return pos
    return go
f = position(origin)
print(f(3))
print(f(4))
print(f(6))
```
运行结果：
![Alt text](./TIM截图20180115231652.png)

- **46. 匿名函数**
```python
'''
匿名函数
'''
#普通函数
def add(x, y):
    return x + y
print(add(1,3))
#匿名函数(定义函数时不需要函数名)
#lambda parameter_list(变量列表): expression（只能是表达式，不能是代码块）
reslut = lambda x, y: x + y
print(reslut(1,3))
```
- **47. 三元表达式**
```python
#三元表达式
#其他语言的写法一般为：x > y ? x : y
x = 6
y = 4
result = x if x > y else y
print(result)
```
- **48. map**

（1）用法：map(func,list)  
第二个参数可以是list 或者其他集合类型
意思是 对list执行func函数，然后返回一个map对象
r = map(func,list)  #返回的是 map对象
（2）可以根据需要将map对象（如r）转换为需要的类型。
```python
list_x = [1, 2, 3, 4, 5, 6, 8]
def square(x):
    return x * x

# for x in list_x:
#     print(square(x))
result = map(square,list_x)
print(result)        #返回一个map对象
print(list(result))  #转换为list类型输出
```
__map和lambda（匿名函数）的结合使用场景__
```python
list_x = [1, 2, 3, 4, 5, 6, 8]
list_y = [1, 2, 3, 5, 6, 7, 9]

result = map(lambda x, y: x + y ,list_x, list_y)
print(result)        #返回一个map对象
print(list(result))  #转换为list类型输出
```
__注： __
map可以接受多个参数（* iterables），map后面的参数集合长度（如这里的list_x, list_y）最好一样，如果不一致，map会按照长度最小的计算。

- **49. reduce**

reduce函数用于连续计算，连续调用参数中的lambda表达式；第一个参数是函数，第二个参数是序列或者集合，第三个参数是初始值。

```python
'''
reduce :连续计算
'''
from functools import reduce

list_x = [1,2,3,4,5,6,7]
#主要用于连续计算，不只是累加。（连续调用lambda）
r = reduce(lambda x, y: x + y,list_x) #这里相当于((((((1+2)+3)+4)+5)+6)+7)的计算过程，即求列表中元素的累加和
print(r)

list_y = ['1', '2', '3', 'python', '1','2', '3']
r1 = reduce(lambda x, y: x + y, list_y, 'hello')  #这里的第三个参数是指初始值，即在进行计算之前的值是初始值'hello'
print(r1)
```
运行结果：
![Alt text](./TIM截图20180116160437.png)

- **50. filter(过滤器)**

```python
'''
filter:过滤器
filter最重要的是判断True或者False,True则返回，False则过滤掉。
'''
list_x = [1,3,0,5,0,7,0,0,6]

r1 = filter(lambda x: True if x > 0 else False, list_x)
print(r1)
print(list(r1))

#上述也可以简写为
r2 = filter(lambda x: x, list_x)
print(list(r2))
```

- **51. 装饰器（难点）**

__编程原则：对修改是封闭的，对扩展是开放的。__
__(1) 写在装饰器之前（未使用装饰器）__

```python
import time

def f1():
    print('This is a function1!')

def f2():
    print('This is a function2!')

def print_time(func):
    print(time.time())
    func()

print_time(f1)
print_time(f2)
```
__(2) 装饰器的实现__

这里与（1）类似，没有体现出装饰器的特点和优势。
```python
import time
def decorator(func):  #实现装饰器要用到嵌套函数
    def wrapper():    #通过wrapper封装实现不改变原来函数的新功能
        print(time.time()) 
        func()
    return wrapper

def f1():
    print('This is a function1!')
#这里改变了函数调用的方式
f = decorator(f1)
f()
```
__（3）装饰器的使用__
使用装饰器：在定义的函数前面加上"@decorator"
```python
import time
def decorator(func):  #实现装饰器要用到嵌套函数
    def wrapper():    #通过wrapper封装实现不改变原来函数的新功能
        print(time.time()) 
        func()
    return wrapper

#(1)使用装饰器一个最大的特点是：不用改变函数调用的方式。
#(2)通过在f1()上面添加了一个装饰器就能添加新的功能，且不用改变f1()函数
@decorator     
def f1():
    print('This is a function1!')
#使用装饰器后，就可以按照普通的函数调用方式就可以调用函数，并且也可以添加新功能
f1()
```
__(4) 装饰器之优化：使用装饰器的函数引入多个参数__

关键解决之道：在wrapper函数中引入__可变参数__
```python
import time
def decorator(func):  #实现装饰器要用到嵌套函数
    def wrapper(*args):    #使用可变参数（*args），这样使用装饰器的函数传入的参数个数可以是多个。
        print(time.time()) 
        func(*args)
    return wrapper
#使用装饰器一个最大的特点是：不用改变函数调用的方式。
#通过在f1()上面添加了一个装饰器就能添加新的功能，且不用改变f1()函数
@decorator  
def f1(param):          #一个参数
    print('This is a function1! '+ param)

@decorator
def f2(param1,param2):  #两个参数
    print('This is a function2! '+ param1)
    print('This is a function2! '+ param2)
#使用装饰器后，就可以按照普通的函数调用方式就可以调用函数，并且也可以添加新功能
f1('Test function1_param')
f2('Test function2_param1','Test function2_param2')
```
__(5) 装饰器之优化：使用装饰器的函数引入了多个参数，且引入了关键字可变参数__
```python
'''
装饰器：当使用装饰器的函数传入的参数包含关键字参数，只使用可变参数是不能引入关键字参数的，
       除了使用可变参数（*args）还要使用关键字可变参数（**kw）的。
'''
import time
#完整的装饰器实现方法
def decorator(func):
    def wrapper(*agrs, **kw):   #需要引入可变参数和关键字可变参数
        print(time.time())
        func(*agrs, **kw)
    return wrapper

@decorator
def f1(f1_param1):
    print('This is function1! ' + f1_param1)

@decorator
def f2(f2_param1, f2_param2):
    print('This is function2! ' + f2_param1)
    print('This is function2! ' + f2_param2)

@decorator
def f3(f3_param1, f3_param2, **kw):  #关键字可变参数
    print('This is function3! ' + f3_param1)
    print('This is function3! ' + f3_param2)
    print('This is function3! ' + str(kw))
    print('This is function3! ', kw)

f1('Test function1_param1')
f2('Test function2_param1', 'Test function2_param2')
f3('Test function3_param1', 'Test function3__param2',a = 1,c= 'python', b = 'hello')
```
运行结果：
![Alt text](./TIM截图20180116212037.png)

- **52. VSCode调试代码技巧**
F5    开始调试
F10  单步调试
F5    跳断点
F11  进入函数或对象的内部
- **53. 爬虫思路**

![Alt text](./5a40f2500001526e19201080.jpg)

- **53. 爬虫一些拓展 **
(1) BeautifulSoup , Scrapy
(2) 爬虫，反爬虫，反反爬虫
(3) ip被封（多次爬取同一网站）
(4) 代理IP库

- **54. Python中使用字典实现Switch... Case ...**
```python
'''
在Python中利用字典实现switch...case...
'''
day = 2

def get_sunday():
    return 'Sunday'

def get_monday():
    return 'Monday'

def get_thursday():
    return 'Thursday'
#当day为其他值的时候，返回Unknown
def get_unknown():
    return 'Unknown'

#字典中key对应的值为函数
switcher = {
    0 : get_sunday,
    1 : get_monday,
    2 : get_thursday,
}

#字典访问的方式有两种：（1）通过下标访问的方式； （2）通过get()方法访问的方式。

result = switcher.get(day,get_unknown)()  #当day的值为其他值时，调用get_unknown函数
#print(switcher[day]) 
print(result)
```
- **55. 列表推导式、集合推导式、字典推导式、元组推导式**

__（1）列表推导式__
```python
a = [1, 2, 3, 5, 6]

b=  [ i * i for i in a if i > 4]  #列表推导式
print(b)  # b是一个列表
```
__（2）集合推导式__
```python
c = {1, 2, 3, 5, 6}
d = { i * i for i in c if i > 4}  #集合推导式
print(d)  # d是一个集合
```
__（3）字典编写列表推导式、字典推导式、元组表达式__
```python
a = {
    '王二小' : 16,
    '刘胡兰' : 20,
    '嘎子'   : 13
}

b = [ key for key, value in a.items() ]  #字典编写列表推导式,不能同时输出key和value
c = { value:key for key, value in a.items()}  #字典推导式，可以更换key和value的顺序
d = ( key for key, value in a.items())        #元组推导式，返回的是一个generator对象，因为元组是不可变，所以它的行为和列表、元组等一些可变序列不一样，且不建议使用元组推导式
#d是一个generator对象
for x in d:   #遍历generator对象
    print(x)
print(b)
print(c)
print(d)
```
运行结果：
![Alt text](./TIM截图20180118172719.png)

- **56. None**

__（1）__None表示空，表示不存在，和''(空字符)、'[]'(空列表)、'False'是不一样的。
```python
a = ''
b = []
c = False

print(a == None)
print(b == None)
print(c == None)

print(a is None)
```
运行结果：
![Alt text](./TIM截图20180118174151.png)
__（2）判空__

a = None时使用' not  a ' 和‘ is None ’两者效果相同。
```python
def func():
    return None

a = func()

if not a:
    print('S')
else:
    print('F')

if a is None:
    print('S')
else:
    print('F')
```
运行结果：
![Alt text](./TIM截图20180118190333.png)

a = [ ]时两者效果不同，所以判空时不要将这两种方式混为一谈。
```python    
a = []
if not a:
    print('S')
else:
    print('F')

if a is None:
    print('S')
else:
    print('F')
```
运行结果：
![Alt text](./TIM截图20180118190706.png)
__（3）__
判空推荐使用下面两种方式
```python
if a:
```
```python
if not a:
```
这样无论是
```python
    a = None
    a = ''
    a =[]
    a = False
```
这四种形式，以上两种判空操作都有效。所以不建议使用‘if a is None’判空。

- **57. 对象存在也不一定是True**

__（1）__对象存在，且不存在“__ __len____ ( )”内置方法和“__ __bool____ ( )”内置方法，则对象的bool值为True。
```python
class Test():
    pass
    
test = Test()

if test:
    print('S')
else:
    print('F')
```
运行结果：输出 ‘S’
__（2）__对象存在，且类中定义了“__ __len____ ( )”内置方法，则对象的bool值取决于“__ __len____ ( )”内置方法的返回值。

| “__ __len____ ( )”内置函数的返回值 | 对象的bool值 |
| :------------------------------: |:----------:|
|0 | False|
| 非0 | True |
| True |True|
| False | False |

```python
class Test():
    def __len__(self):
        return 0

test = Test()

if test:
    print('S')
else:
    print('F')
```
运行结果：输出‘F’

__（3）__ 对象存在，且类中定义了“__ __len____ ( )”内置方法和“__ __bool____ ( )”内置方法，则对象的值取决于“__ __bool____ ( )”内置方法的返回值。（注意：“__ __bool____ ( )”的返回值只能是bool类型的，即True或False）

| “__ __bool__ __ ( )”内置函数的返回值 | 对象的bool值 |
| :------------------------------: |:----------:|
|True|True|
|False | False|

```python
class Test():
    def __bool__(self):
        return True
    def __len__(self):
        return 0

test = Test()
if test:
    print('S')
else:
    print('F')
```
运行结果：输出 ‘S’

__注：__
__（1）永远不要认为对象不为None（即对象存在），对象的bool值就一定是True __
__（2）“__ __bool__ __ ( )”内置方法是Python3中的方法，其在Python2中对应的内置方法是“__ __nonzero ____ ( )”方法__
__（3）'', [ ] , None 都表示False__

- **58. 全局函数**

```python
class Test():
    #全局函数必须在类中定义了，在类外部才能被调用。
    def __len__(self):
        return 0

print(len('a009'))  #调用全局函数len
#打印结果为4，即字符串长度为4个
```
- **59. 装饰器的副作用**

（1）未使用装饰器
```python
import time

def decorator(func):
    def wrapper():
        print(time.time())
        func()
    return wrapper
    
def f1():
    '''
     This is f1
    '''
    print(f1.__name__)

f1()
```
运行结果：
![Alt text](./TIM截图20180118210745.png)
（2）使用装饰器时，会改变函数的名字，变为装饰器中的闭包函数名字
```python
import time

def decorator(func):
    def wrapper():
        print(time.time())
        func()
    return wrapper
    
@decorator
def f1():
    '''
     This is f1
    '''
    print(f1.__name__)

f1()
```
运行结果为：
![Alt text](./TIM截图20180118211358.png)

（3）使用装饰器，为了不改变函数的名字，应该在闭包函数前使用装饰器（"@wraps( func )"）。
```python
import time
from functools import wraps  #首先要引入wraps装饰器

def decorator(func):
    @wraps(func)   #注意：不要忘了加func这个函数参数
    def wrapper():
        print(time.time())
        func()
    return wrapper
@decorator
def f1():
    '''
     This is f1
    '''
    print(f1.__name__)

f1()

```
运行结果：
![Alt text](./TIM截图20180118212122.png)

__注:__
（1）加上（“@wraps（func）“）这个装饰器之所以可以使原函数的名字不改变，是因为这里的func事实上就是f1函数，使用wraps这个装饰器后，就把f1函数的信息复制给了wrapper闭包函数，虽然在执行f1函数时实际执行的函数是wrapper，所以原函数的名字仍然为f1；若没有这个wraps装饰器，则因为实际执行的函数是wrapper，所以原函数的名字被改变为wrapper。
（2）函数名字改变会导致在后续编程中出现很多莫名其妙的错误，因此再使用装饰器的时候，要注意函数名字改变的问题。
