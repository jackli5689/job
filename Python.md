#Python

什么是变量？
<pre>
>>> name=input()
>>>            ---等着你输入

>>> print(name)
Michael    ---输出你刚才输入的值

>>> print("My name is","Jackli")
My name is Jackli

#数据类型和变量：
####整数
Python可以处理任意大小的整数，当然包括负整数，在程序中的表示方法和数学上的写法一模一样，例如：1，100，-8080，0，等等。

计算机由于使用二进制，所以，有时候用十六进制表示整数比较方便，十六进制用0x前缀和0-9，a-f表示，例如：0xff00，0xa5b4c3d2，等等

###浮点数
浮点数也就是小数，之所以称为浮点数，是因为按照科学记数法表示时，一个浮点数的小数点位置是可变的，比如，1.23x109和12.3x108是完全相等的。

浮点数可以用数学写法，如1.23，3.14，-9.01，等等。但是对于很大或很小的浮点数，就必须用科学计数法表示，把10用e替代，1.23x10的9次方就是1.23e9，或者12.3e8，0.000012可以写成1.2e-5，等等

整数和浮点数：整数和浮点数在计算机内部存储的方式是不同的，整数运算永远是精确的（除法难道也是精确的？是的！），而浮点数运算则可能会有四舍五入的误差。

###字符串
字符串是以单引号'或双引号"括起来的任意文本
>>> print('I\'m \"OK\"')
I'm "OK"

如果字符串里面有很多字符都需要转义，就需要加很多\，为了简化，Python还允许用r''表示''内部的字符串默认不转义
>>> print(r'\\\\\\\t\\')
\\\\\\\t\\

如果字符串内部有很多换行，用\n写在一行里不好阅读，为了简化，Python允许用'''...'''的格式表示多行内容
>>> print('''line1
... line2
... line3''')
line1
line2
line3

多行字符串'''...'''还可以在前面加上r使用：
>>> print('''line1
... line2'''r'\\\\n\\')
line1
line2\\\\n\\

###布尔值
布尔值和布尔代数的表示完全一致，一个布尔值只有True、False两种值，要么是True，要么是False，在Python中，可以直接用True、False表示布尔值（请注意大小写），也可以通过布尔运算计算出来：
>>> True
True
>>> False
False
>>> 6>5
True
>>> 5>6
False

###布尔值可以用and、or和not运算。
and运算是与运算，只有所有都为True，and运算结果才是True：
>>> True and True
True
>>> False and False
False
>>> True and False
False
>>> 6>5 and 5<6
True
>>> 8>9 and 9>10
False
>>> 10>9 and 9>10
False
or运算是或运算，只要其中有一个为True，or运算结果就是True：
>>> 20>10 or 10>5
True
>>> 20>10 or 10<5
True
>>> 20<10 or 10<5
False

not运算是非运算，它是一个单目运算符，把True变成False，False变成True：
>>> not True
False
>>> not False
True
>>> not 1>2
True
>>> not 2>1
False
布尔值经常用在条件判断中，比如：

if age >= 18:
    print('adult')
else:
    print('teenager')

###空值
空值是Python里一个特殊的值，用None表示。None不能理解为0，因为0是有意义的，而None是一个特殊的空值。
此外，Python还提供了列表、字典等多种数据类型，还允许创建自定义数据类型。
>>> a=123
>>> a
123
>>> a='ABC'
>>> a
'ABC'

这种变量本身类型不固定的语言称之为动态语言，与之对应的是静态语言。静态语言在定义变量时必须指定变量类型，如果赋值的时候类型不匹配，就会报错。例如Java是静态语言，赋值语句如下（// 表示注释）：
int a = 123; // a是整数类型变量
a = "ABC"; // 错误：不能把字符串赋给整型变量

请不要把赋值语句的等号等同于数学的等号。比如下面的代码：
x = 10
x = x + 2
如果从数学上理解x = x + 2那无论如何是不成立的，在程序中，赋值语句先计算右侧的表达式x + 2，得到结果12，再赋给变量x。由于x之前的值是10，重新赋值后，x的值变成12。

最后，理解变量在计算机内存中的表示也非常重要。当我们写：
a = 'ABC'时，Python解释器干了两件事情：
1. 在内存中创建了一个'ABC'的字符串；
2. 在内存中创建了一个名为a的变量，并把它指向'ABC'。

>>> a='ABC'
>>> b=a
>>> b
'ABC'
>>> a='XYZ'
>>> b
'ABC'
解答：
1. 执行a = 'ABC'，解释器创建了字符串'ABC'和变量a，并把a指向'ABC'
2. 执行b = a，解释器创建了变量b，并把b指向a指向的字符串'ABC'
3. 执行a = 'XYZ'，解释器创建了字符串'XYZ'，并把a的指向改为'XYZ'，但b并没有更改

###常量
所谓常量就是不能变的变量，比如常用的数学常数π就是一个常量。在Python中，通常用全部大写的变量名表示常量：
但事实上PI仍然是一个变量，Python根本没有任何机制保证PI不会被改变，所以，**用全部大写的变量名表示常量只是一个习惯上的用法，如果你一定要改变变量pi的值，也没人能拦住你**。

###最后解释一下整数的除法为什么也是精确的。在Python中，有两种除法，一种除法是/：
>>> 10 / 3
3.3333333333333335
/除法计算结果是浮点数，即使是两个整数恰好整除，结果也是浮点数：
>>> 9 / 3
3.0

###还有一种除法是//，称为地板除，两个整数的除法仍然是整数：
>>> 10 // 3
3
你没有看错，整数的地板除//永远是整数，即使除不尽。要做精确的除法，使用/就可以。
**因为//除法只取结果的整数部分**，所以Python还提供一个余数运算，可以得到两个整数相除的余数：
>>> 10 % 3
1
无论整数做//除法还是取余数，结果永远是整数，所以，整数运算结果永远是精确的。

#########小结
Python支持多种数据类型，在计算机内部，可以把任何数据都看成一个“对象”，而变量就是在程序中用来指向这些数据对象的，对变量赋值就是把数据和变量给关联起来。
对变量赋值x = y是把变量x指向真正的对象，该对象是变量y所指向的。随后对变量y的赋值不影响变量的指向。
注意：Python的整数没有大小限制，而某些语言的整数根据其存储长度是有大小限制的，例如Java对32位整数的范围限制在-2147483648-2147483647。
Python的浮点数也没有大小限制，但是超出一定范围就直接表示为inf（无限大）


#字符串和编码
###字符编码
ASCII编码和Unicode编码的区别：ASCII编码是1个字节，而Unicode编码通常是2个字节。
字母A用ASCII编码是十进制的65，二进制的01000001；
字符0用ASCII编码是十进制的48，二进制的00110000，注意字符'0'和整数0是不同的；
汉字中已经超出了ASCII编码的范围，用Unicode编码是十进制的20013，二进制的01001110 00101101。
如果把ASCII编码的A用Unicode编码，只需要在前面补0就可以，因此，A的Unicode编码是00000000 01000001。

总结:先有ASCII编码——————随着中文的出现，ASCII编码不能满足中文，所以中国制定了GB2312编码——————但是全世界有上百种语言，日本把日文编到Shift_JIS里，韩国把韩文编到Euc-kr里，各国有各国的标准，就会不可避免地出现冲突，结果就是，在多语言混合的文本中，显示出来会有乱码——————因此，Unicode应运而生。Unicode把所有语言都统一到一套编码里，这样就不会再有乱码问题了——————本着节约的精神，又出现了把Unicode编码转化为“可变长编码”的UTF-8编码
字符		ASCII			Unicode				UTF-8
A		01000001		00000000 01000001		01000001
中		x			01001110 00101101		11100100 10111000 10101101

从上面的表格还可以发现，UTF-8编码有一个额外的好处，就是ASCII编码实际上可以被看成是UTF-8编码的一部分，所以，大量只支持ASCII编码的历史遗留软件可以在UTF-8编码下继续工作。

搞清楚了ASCII、Unicode和UTF-8的关系，我们就可以总结一下现在计算机系统通用的字符编码工作方式：
1. 在计算机内存中，统一使用Unicode编码，当需要保存到硬盘或者需要传输的时候，就转换为UTF-8编码。
   用记事本编辑的时候，从文件读取的UTF-8字符被转换为Unicode字符到内存里，编辑完成后，保存的时候再把Unicode转换为UTF-8保存到文件
2. 浏览网页的时候，服务器会把动态生成的Unicode内容转换为UTF-8再传输到浏览器


在最新的Python 3版本中，字符串是以Unicode编码的，也就是说，Python的字符串支持多语言，例如：
>>> print('包含中文的str')
包含中文的str

对于单个字符的编码，Python提供了ord()函数获取字符的整数表示，chr()函数把编码转换为对应的字符：
>>> ord('A')
65
>>> ord('中')
20013
>>> chr(66)
'B'
>>> chr(25991)
'文'

如果知道字符的整数编码，还可以用十六进制这么写str：
>>> '\u4e2d\u6587'
'中文'


要注意区分'ABC'和b'ABC'，前者是str，后者虽然内容显示得和前者一样，但bytes的每个字符都只占用一个字节。
以Unicode表示的str通过encode()方法可以编码为指定的bytes，例如：
>>> 'ABC'.encode('ascii')
b'ABC'
>>> '中文'.encode('utf-8')
b'\xe4\xb8\xad\xe6\x96\x87'
>>> '中文'.encode('ascii')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
UnicodeEncodeError: 'ascii' codec can't encode characters in position 0-1: ordinal not in range(128)

反过来，如果我们从网络或磁盘上读取了字节流，那么读到的数据就是bytes。要把bytes变为str，就需要用decode()方法：
>>> b'ABC'.decode('ascii')
'ABC'
>>> b'\xe4\xb8\xad\xe6\x96\x87'.decode('utf-8')
'中文'

有些时候，字符串里面的%是一个普通字符怎么办？这个时候就需要转义，用%%来表示一个%：
>>> 'growth rate: %d %%' % 7
'growth rate: 7 %'

#使用list和tuple

#list--Python内置的一种数据类型是列表：list。list是一种有序的集合，可以随时添加和删除其中的元素。

比如，列出班里所有同学的名字，就可以用一个list表示：

>>> classmates = ['Michael', 'Bob', 'Tracy']
>>> classmates
['Michael', 'Bob', 'Tracy']
变量classmates就是一个list。用len()函数可以获得list元素的个数：
>>> len(classmates)
3

用索引来访问list中每一个位置的元素，记得索引是从0开始的：
>>> classmates[0]
'Michael'
>>> classmates[1]
'Bob'
>>> classmates[2]
'Tracy'
>>> classmates[3]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: list index out of range
当索引超出了范围时，Python会报一个IndexError错误，所以，要确保索引不要越界，记得最后一个元素的索引是len(classmates) - 1。
如果要取最后一个元素，除了计算索引位置外，还可以用-1做索引，直接获取最后一个元素：
>>> classmates[-1]
'Tracy'
以此类推，可以获取倒数第2个、倒数第3个：
>>> classmates[-2]
'Bob'
>>> classmates[-3]
'Michael'
>>> classmates[-4]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: list index out of range
当然，倒数第4个就越界了。

list是一个可变的有序表，所以，可以往list中追加元素到末尾：
>>> classmates.append('Adam')
>>> classmates
['Michael', 'Bob', 'Tracy', 'Adam']
也可以把元素插入到指定的位置，比如索引号为1的位置：
>>> classmates.insert(1, 'Jack')
>>> classmates
['Michael', 'Jack', 'Bob', 'Tracy', 'Adam']
要删除list末尾的元素，用pop()方法：
>>> classmates.pop()
'Adam'
>>> classmates
['Michael', 'Jack', 'Bob', 'Tracy']
要删除指定位置的元素，用pop(i)方法，其中i是索引位置：
>>> classmates.pop(1)
'Jack'
>>> classmates
['Michael', 'Bob', 'Tracy']
要把某个元素替换成别的元素，可以直接赋值给对应的索引位置：
>>> classmates[1] = 'Sarah'
>>> classmates
['Michael', 'Sarah', 'Tracy']
list里面的元素的数据类型也可以不同，比如：
>>> L = ['Apple', 123, True]

list元素也可以是另一个list，比如：
>>> s = ['python', 'java', ['asp', 'php'], 'scheme']
>>> len(s)
4
要注意s只有4个元素，其中s[2]又是一个list，如果拆开写就更容易理解了：
>>> p = ['asp', 'php']
>>> s = ['python', 'java', p, 'scheme']
要拿到'php'可以写p[1]或者s[2][1]，因此s可以看成是一个二维数组，类似的还有三维、四维……数组，不过很少用到。
如果一个list中一个元素也没有，就是一个空的list，它的长度为0：
>>> L = []
>>> len(L)
0

#tuple--一种有序列表也叫元组：tuple。tuple和list非常类似，但是tuple一旦初始化就不能修改，比如同样是列出同学的名字：
另一种有序列表叫元组：tuple。tuple和list非常类似，但是tuple一旦初始化就不能修改，比如同样是列出同学的名字：
>>> classmates = ('Michael', 'Bob', 'Tracy')
现在，classmates这个tuple不能变了，它也没有append()，insert()这样的方法。其他获取元素的方法和list是一样的，你可以正常地使用classmates[0]，classmates[-1]，但不能赋值成另外的元素。

不可变的tuple有什么意义？因为tuple不可变，所以代码更安全。如果可能，能用tuple代替list就尽量用tuple。
tuple的陷阱：当你定义一个tuple时，在定义的时候，tuple的元素就必须被确定下来，比如：

>>> t = (1, 2)
>>> t
(1, 2)
如果要定义一个空的tuple，可以写成()：
>>> t = ()
>>> t
()
但是，要定义一个只有1个元素的tuple，如果你这么定义：
>>> t = (1)
>>> t
1
定义的不是tuple，是1这个数！这是因为括号()既可以表示tuple，又可以表示数学公式中的小括号，这就产生了歧义，因此，Python规定，这种情况下，按小括号进行计算，计算结果自然是1。
所以，只有1个元素的tuple定义时必须加一个逗号,，来消除歧义：
>>> t = (1,)
>>> t
(1,)
Python在显示只有1个元素的tuple时，也会加一个逗号,，以免你误解成数学计算意义上的括号。

最后来看一个“可变的”tuple：
>>> t = ('a', 'b', ['A', 'B'])
>>> t[2][0] = 'X'
>>> t[2][1] = 'Y'
>>> t
('a', 'b', ['X', 'Y'])
这个tuple定义的时候有3个元素，分别是'a'，'b'和一个list。不是说tuple一旦定义后就不可变了吗？怎么后来又变了？

表面上看，tuple的元素确实变了，但其实变的不是tuple的元素，而是list的元素。tuple一开始指向的list并没有改成别的list，所以，tuple所谓的“不变”是说，tuple的每个元素，指向永远不变。即指向'a'，就不能改成指向'b'，指向一个list，就不能改成指向其他对象，但指向的这个list本身是可变的！
理解了“指向不变”后，要创建一个内容也不变的tuple怎么做？那就必须保证tuple的每一个元素本身也不能变。

#条件判断
输入用户年龄，根据年龄打印不同的内容，在Python程序中，用if语句实现：
age = 20
if age >= 18:
    print('your age is', age)
    print('adult')
根据Python的缩进规则，如果if语句判断是True，就把缩进的两行print语句执行了，否则，什么也不做。

给if添加一个else语句，意思是，如果if判断是False，不要执行if的内容，去把else执行了：
age = 3
if age >= 18:
    print('your age is', age)
    print('adult')
else:
    print('your age is', age)
    print('teenager')

注意不要少写了冒号:。

当然上面的判断是很粗略的，完全可以用elif做更细致的判断：
age = 3
if age >= 18:
    print('adult')
elif age >= 6:
    print('teenager')
else:
    print('kid')
elif是else if的缩写，完全可以有多个elif，所以if语句的完整形式就是：
if <条件判断1>:
    <执行1>
elif <条件判断2>:
    <执行2>
elif <条件判断3>:
    <执行3>
else:
    <执行4>

if语句执行有个特点，它是从上往下判断，如果在某个判断上是True，把该判断对应的语句执行后，就忽略掉剩下的elif和else，所以，请测试并解释为什么下面的程序打印的是teenager：
age = 20
if age >= 6:
    print('teenager')
elif age >= 18:
    print('adult')
else:
    print('kid')

if判断条件还可以简写，比如写：
if x:
    print('True')
只要x是非零数值、非空字符串、非空list等，就判断为True，否则为False。

再议 input
最后看一个有问题的条件判断。很多同学会用input()读取用户的输入，这样可以自己输入，程序运行得更有意思：

birth = input('birth: ')
if birth < 2000:
    print('00前')
else:
    print('00后')
输入1982，结果报错：
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unorderable types: str() > int()
这是因为input()返回的数据类型是str，str不能直接和整数比较，必须先把str转换成整数。Python提供了int()函数来完成这件事情：
s = input('birth: ')
birth = int(s)
if birth < 2000:
    print('00前')
else:
    print('00后')
再次运行，就可以得到正确地结果。

练习BMI:
# -*- coding: utf-8 -*-
h = float(input("your height('m'):"))
zl = float(input("your weight('kg'):"))
avg = (zl/(h*h))
print('%.2f' % (avg))
if avg < 18.5:
	print("过轻")
elif avg < 25:
	print("正常")
elif avg < 28:
	print("过重")
elif avg < 32:
	print("肥胖")
else :
	print("严重肥胖")
	
#循环
Python的循环有两种，一种是for...in循环，依次把list或tuple中的每个元素迭代出来，看例子：
names = ['Michael', 'Bob', 'Tracy']
for name in names:
    print(name)
例二：
sum = 0
for x in [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]:
    sum = sum + x
print(sum)

如果要计算1-100的整数之和，从1写到100有点困难，幸好Python提供一个range()函数，可以生成一个整数序列，再通过list()函数可以转换为list。比如range(5)生成的序列是从0开始小于5的整数：
>>> list(range(5))
[0, 1, 2, 3, 4]

第二种循环是while循环，只要条件满足，就不断循环，条件不满足时退出循环。比如我们要计算100以内所有奇数之和，可以用while循环实现：
sum = 0
n = 99
while n > 0:
    sum = sum + n
    n = n - 2
print(sum)
在循环内部变量n不断自减，直到变为-1时，不再满足while条件，循环退出。
#break提前跳出
如果要提前结束循环，可以用break语句：
n = 1
while n <= 100:
    if n > 10: # 当n = 11时，条件满足，执行break语句
        break # break语句会结束当前循环
    print(n)
    n = n + 1
print('END')

#continue
在循环过程中，也可以通过continue语句，跳过当前的这次循环，直接开始下一次循环。
上面的程序可以打印出1～10。但是，如果我们想只打印奇数，可以用continue语句跳过某些循环：

n = 0
while n < 10:
    n = n + 1
    if n % 2 == 0: # 如果n是偶数，执行continue语句
        continue # continue语句会直接继续下一轮循环，后续的print()语句不会执行
    print(n)

小结
循环是让计算机做重复任务的有效的方法。
break语句可以在循环过程中直接退出循环，而continue语句可以提前结束本轮循环，并直接开始下一轮循环。这两个语句通常都必须配合if语句使用。
要特别注意，不要滥用break和continue语句。break和continue会造成代码执行逻辑分叉过多，容易出错。大多数循环并不需要用到break和continue语句，上面的两个例子，都可以通过改写循环条件或者修改循环逻辑，去掉break和continue语句。
有些时候，如果代码写得有问题，会让程序陷入“死循环”，也就是永远循环下去。这时可以用Ctrl+C退出程序，或者强制结束Python进程。

#dict--Python内置了字典：dict的支持，dict全称dictionary，在其他语言中也称为map，使用键-值（key-value）存储，具有极快的查找速度。

举个例子，假设要根据同学的名字查找对应的成绩，如果用list实现，需要两个list：
names = ['Michael', 'Bob', 'Tracy']
scores = [95, 75, 85]
给定一个名字，要查找对应的成绩，就先要在names中找到对应的位置，再从scores取出对应的成绩，list越长，耗时越长。
如果用dict实现，只需要一个“名字”-“成绩”的对照表，直接根据名字查找成绩，无论这个表有多大，查找速度都不会变慢。用Python写一个dict如下：
>>> d = {'Michael': 95, 'Bob': 75, 'Tracy': 85}
>>> d['Michael']
95
为什么dict查找速度这么快？因为dict的实现原理和查字典是一样的。假设字典包含了1万个汉字，我们要查某一个字，一个办法是把字典从第一页往后翻，直到找到我们想要的字为止，这种方法就是在list中查找元素的方法，list越大，查找越慢。

第二种方法是先在字典的索引表里（比如部首表）查这个字对应的页码，然后直接翻到该页，找到这个字。无论找哪个字，这种查找速度都非常快，不会随着字典大小的增加而变慢。

dict就是第二种实现方式，给定一个名字，比如'Michael'，dict在内部就可以直接计算出Michael对应的存放成绩的“页码”，也就是95这个数字存放的内存地址，直接取出来，所以速度非常快。

把数据放入dict的方法，除了初始化时指定外，还可以通过key放入：
>>> d['Adam'] = 67
>>> d['Adam']
67

由于一个key只能对应一个value，所以，多次对一个key放入value，后面的值会把前面的值冲掉：
>>> d['Jack'] = 90
>>> d['Jack']
90
>>> d['Jack'] = 88
>>> d['Jack']
88

如果key不存在，dict就会报错：
>>> d['Thomas']
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
KeyError: 'Thomas'
要避免key不存在的错误，有两种办法，一是通过in判断key是否存在：
>>> 'Thomas' in d
False
二是通过dict提供的get()方法，如果key不存在，可以返回None，或者自己指定的value：
>>> d.get('Thomas')
>>> d.get('Thomas', -1)
-1
注意：返回None的时候Python的交互环境不显示结果。

要删除一个key，用pop(key)方法，对应的value也会从dict中删除：
>>> d.pop('Bob')
75
>>> d
{'Michael': 95, 'Tracy': 85}

请务必注意，dict内部存放的顺序和key放入的顺序是没有关系的。

和list比较，dict有以下几个特点：
查找和插入的速度极快，不会随着key的增加而变慢；
需要占用大量的内存，内存浪费多。

而list相反：
查找和插入的时间随着元素的增加而增加；
占用空间小，浪费内存很少。

所以，dict是用空间来换取时间的一种方法。
dict可以用在需要高速查找的很多地方，在Python代码中几乎无处不在，正确使用dict非常重要，需要牢记的第一条就是dict的key必须是不可变对象。
这是因为dict根据key来计算value的存储位置，如果每次计算相同的key得出的结果不同，那dict内部就完全混乱了。这个通过key计算位置的算法称为哈希算法（Hash）。

要保证hash的正确性，作为key的对象就不能变。在Python中，字符串、整数等都是不可变的，因此，可以放心地作为key。而list是可变的，就不能作为key：
>>> key = [1, 2, 3]
>>> d[key] = 'a list'
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unhashable type: 'list'

#set--set和dict类似，也是一组key的集合，但不存储value。由于key不能重复，所以，在set中，没有重复的key。
要创建一个set，需要提供一个list作为输入集合：

>>> s = set([1, 2, 3])
>>> s
{1, 2, 3}
注意，传入的参数[1, 2, 3]是一个list，而显示的{1, 2, 3}只是告诉你这个set内部有1，2，3这3个元素，显示的顺序也不表示set是有序的。

重复元素在set中自动被过滤：
>>> s = set([1, 1, 2, 2, 3, 3])
>>> s
{1, 2, 3}

通过add(key)方法可以添加元素到set中，可以重复添加，但不会有效果：
>>> s.add(4)
>>> s
{1, 2, 3, 4}
>>> s.add(4)
>>> s
{1, 2, 3, 4}
通过remove(key)方法可以删除元素：
>>> s.remove(4)
>>> s
{1, 2, 3}

set可以看成数学意义上的无序和无重复元素的集合，因此，两个set可以做数学意义上的交集、并集等操作：

>>> s1 = set([1, 2, 3])
>>> s2 = set([2, 3, 4])
>>> s1 & s2  #交集输出
{2, 3}
>>> s1 | s2  #并集输出
{1, 2, 3, 4}
set和dict的唯一区别仅在于没有存储对应的value，但是，set的原理和dict一样，所以，同样不可以放入可变对象，因为无法判断两个可变对象是否相等，也就无法保证set内部“不会有重复元素”。把list放入set，也会跟dict一样报错。

#再议不可变对象
上面我们讲了，str是不变对象，而list是可变对象。
对于可变对象，比如list，对list进行操作，list内部的内容是会变化的，比如：
>>> a = ['c', 'b', 'a']
>>> a.sort()
>>> a   #因为list是可变对象所以会成功改变
['a', 'b', 'c']

而对于不可变对象，比如str，对str进行操作呢：
>>> a = 'abc'
>>> a.replace('a', 'A')
'Abc'
>>> a  #因为字符串是不可变对象所以不会成功改变
'abc'

所以，对于不变对象来说，调用对象自身的任意方法，也不会改变该对象自身的内容。相反，这些方法会创建新的对象并返回，这样，就保证了不可变对象本身永远是不可变的。

小结
使用key-value存储结构的dict在Python中非常有用，选择不可变对象作为key很重要，最常用的key是字符串。


#调用函数
Python内置了很多有用的函数，我们可以直接调用。
要调用一个函数，需要知道函数的名称和参数，比如求绝对值的函数abs，只有一个参数。可以直接从Python的官方网站查看文档：
http://docs.python.org/3/library/functions.html#abs
也可以在交互式命令行通过help(abs)查看abs函数的帮助信息。
调用abs函数：
>>> abs(100)
100
>>> abs(-20)
20
>>> abs(12.34)
12.34

而max函数max()可以接收任意多个参数，并返回最大的那个：
>>> max(1, 2)
2
>>> max(2, 3, 1, -5)
3

###数据类型转换
Python内置的常用函数还包括数据类型转换函数，比如int()函数可以把其他数据类型转换为整数：
>>> int('123')
123
>>> int(12.34)
12
>>> float('12.34')
12.34
>>> str(1.23)
'1.23'
>>> str(100)
'100'
>>> bool(1)
True
>>> bool('')
False

函数名其实就是指向一个函数对象的引用，完全可以把函数名赋给一个变量，相当于给这个函数起了一个“别名”：
>>> a = abs # 变量a指向abs函数
>>> a(-1) # 所以也可以通过a调用abs函数
1

hex()函数把一个整数转换成十六进制表示的字符串：
>>> hex(255)
'0xff'
>>> hex(1000)
'0x3e8'

我们以自定义一个求绝对值的my_abs函数为例：
def my_abs(x):
    if x >= 0:
        return x
    else:
        return -x
print(my_abs(-99))

如果没有return语句，函数执行完毕后也会返回结果，只是结果为None。return None可以简写为return。
在Python交互环境中定义函数时，注意Python会出现...的提示。函数定义结束后需要按两次回车重新回到>>>提示符下

空函数
如果想定义一个什么事也不做的空函数，可以用pass语句：
def nop():
    pass
pass语句什么都不做，那有什么用？实际上pass可以用来作为占位符，比如现在还没想好怎么写函数的代码，就可以先放一个pass，让代码能运行起来。
pass还可以用在其他语句里，比如：
if age >= 18:
    pass
缺少了pass，代码运行就会有语法错误。

###返回多个值
import math

def move(x, y, step, angle=0):
    nx = x + step * math.cos(angle)
    ny = y - step * math.sin(angle)
    return nx, ny
import math语句表示导入math包，并允许后续代码引用math包里的sin、cos等函数。

然后，我们就可以同时获得返回值：
>>> x, y = move(100, 100, 60, math.pi / 6)
>>> print(x, y)
151.96152422706632 70.0
但其实这只是一种假象，Python函数返回的仍然是单一值：
>>> r = move(100, 100, 60, math.pi / 6)
>>> print(r)
(151.96152422706632, 70.0)
原来返回值是一个tuple！但是，在语法上，返回一个tuple可以省略括号，而多个变量可以同时接收一个tuple，按位置赋给对应的值，所以，Python的函数返回多值其实就是返回一个tuple，但写起来更方便。

小结：
定义函数时，需要确定函数名和参数个数；
如果有必要，可以先对参数的数据类型做检查；
函数体内部可以用return随时返回函数结果；
函数执行完毕也没有return语句时，自动return None。
函数可以同时返回多个值，但其实就是一个tuple。

###函数的参数
你也许想到了，可以把power(x)修改为power(x, n)，用来计算xn，说干就干：
def power(x, n):
    s = 1
    while n > 0:
        n = n - 1
        s = s * x
    return s
对于这个修改后的power(x, n)函数，可以计算任意n次方：
>>> power(5, 2)
25
>>> power(5, 3)
125
修改后的power(x, n)函数有两个参数：x和n，这两个参数都是位置参数，调用函数时，传入的两个值按照位置顺序依次赋给参数x和n。

###默认参数
新的power(x, n)函数定义没有问题，但是，旧的调用代码失败了，原因是我们增加了一个参数，导致旧的代码因为缺少一个参数而无法正常调用：
>>> power(5)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: power() missing 1 required positional argument: 'n'
Python的错误信息很明确：调用函数power()缺少了一个位置参数n。

这个时候，默认参数就排上用场了。由于我们经常计算x2，所以，完全可以把第二个参数n的默认值设定为2：
def power(x, n=2):
    s = 1
    while n > 0:
        n = n - 1
        s = s * x
    return s
这样，当我们调用power(5)时，相当于调用power(5, 2)：
>>> power(5)
25
>>> power(5, 2)
25

举个例子，我们写个一年级小学生注册的函数，需要传入name和gender两个参数，如果要继续传入年龄、城市等信息怎么办？这样会使得调用函数的复杂度大大增加。
我们可以把年龄和城市设为默认参数：
def enroll(name, gender, age=6, city='Beijing'):
    print('name:', name)
    print('gender:', gender)
    print('age:', age)
    print('city:', city)
这样，大多数学生注册时不需要提供年龄和城市，只提供必须的两个参数：
>>> enroll('Sarah', 'F')
name: Sarah
gender: F
age: 6
city: Beijing
只有与默认参数不符的学生才需要提供额外的信息：
enroll('Bob', 'M', 7)
enroll('Adam', 'M', city='Tianjin')

有多个默认参数时，调用的时候，既可以按顺序提供默认参数，比如调用enroll('Bob', 'M', 7)，意思是，除了name，gender这两个参数外，最后1个参数应用在参数age上，city参数由于没有提供，仍然使用默认值。

也可以不按顺序提供部分默认参数。当不按顺序提供部分默认参数时，需要把参数名写上。比如调用enroll('Adam', 'M', city='Tianjin')，意思是，city参数用传进去的值，其他默认参数继续使用默认值。

###坑例子：
默认参数很有用，但使用不当，也会掉坑里。默认参数有个最大的坑，演示如下：

先定义一个函数，传入一个list，添加一个END再返回：

def add_end(L=[]):
    L.append('END')
    return L
当你正常调用时，结果似乎不错：

>>> add_end([1, 2, 3])
[1, 2, 3, 'END']
>>> add_end(['x', 'y', 'z'])
['x', 'y', 'z', 'END']
当你使用默认参数调用时，一开始结果也是对的：

>>> add_end()
['END']
但是，再次调用add_end()时，结果就不对了：

>>> add_end()
['END', 'END']
>>> add_end()
['END', 'END', 'END']
很多初学者很疑惑，默认参数是[]，但是函数似乎每次都“记住了”上次添加了'END'后的list。

原因解释如下：
Python函数在定义的时候，默认参数L的值就被计算出来了，即[]，因为默认参数L也是一个变量，它指向对象[]，每次调用该函数，如果改变了L的内容，则下次调用时，默认参数的内容就变了，不再是函数定义时的[]了。

 ！！！！！！定义默认参数要牢记一点：默认参数必须指向不变对象！


函数的参数
阅读: 605027
定义函数的时候，我们把参数的名字和位置确定下来，函数的接口定义就完成了。对于函数的调用者来说，只需要知道如何传递正确的参数，以及函数将返回什么样的值就够了，函数内部的复杂逻辑被封装起来，调用者无需了解。

Python的函数定义非常简单，但灵活度却非常大。除了正常定义的必选参数外，还可以使用默认参数、可变参数和关键字参数，使得函数定义出来的接口，不但能处理复杂的参数，还可以简化调用者的代码。

位置参数
我们先写一个计算x2的函数：

def power(x):
    return x * x
对于power(x)函数，参数x就是一个位置参数。

当我们调用power函数时，必须传入有且仅有的一个参数x：

>>> power(5)
25
>>> power(15)
225
现在，如果我们要计算x3怎么办？可以再定义一个power3函数，但是如果要计算x4、x5……怎么办？我们不可能定义无限多个函数。

你也许想到了，可以把power(x)修改为power(x, n)，用来计算xn，说干就干：

def power(x, n):
    s = 1
    while n > 0:
        n = n - 1
        s = s * x
    return s
对于这个修改后的power(x, n)函数，可以计算任意n次方：

>>> power(5, 2)
25
>>> power(5, 3)
125
修改后的power(x, n)函数有两个参数：x和n，这两个参数都是位置参数，调用函数时，传入的两个值按照位置顺序依次赋给参数x和n。

默认参数
新的power(x, n)函数定义没有问题，但是，旧的调用代码失败了，原因是我们增加了一个参数，导致旧的代码因为缺少一个参数而无法正常调用：

>>> power(5)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: power() missing 1 required positional argument: 'n'
Python的错误信息很明确：调用函数power()缺少了一个位置参数n。

这个时候，默认参数就排上用场了。由于我们经常计算x2，所以，完全可以把第二个参数n的默认值设定为2：

def power(x, n=2):
    s = 1
    while n > 0:
        n = n - 1
        s = s * x
    return s
这样，当我们调用power(5)时，相当于调用power(5, 2)：

>>> power(5)
25
>>> power(5, 2)
25
而对于n > 2的其他情况，就必须明确地传入n，比如power(5, 3)。

从上面的例子可以看出，默认参数可以简化函数的调用。设置默认参数时，有几点要注意：

一是必选参数在前，默认参数在后，否则Python的解释器会报错（思考一下为什么默认参数不能放在必选参数前面）；

二是如何设置默认参数。

当函数有多个参数时，把变化大的参数放前面，变化小的参数放后面。变化小的参数就可以作为默认参数。

使用默认参数有什么好处？最大的好处是能降低调用函数的难度。

举个例子，我们写个一年级小学生注册的函数，需要传入name和gender两个参数：

def enroll(name, gender):
    print('name:', name)
    print('gender:', gender)
这样，调用enroll()函数只需要传入两个参数：

>>> enroll('Sarah', 'F')
name: Sarah
gender: F
如果要继续传入年龄、城市等信息怎么办？这样会使得调用函数的复杂度大大增加。

我们可以把年龄和城市设为默认参数：

def enroll(name, gender, age=6, city='Beijing'):
    print('name:', name)
    print('gender:', gender)
    print('age:', age)
    print('city:', city)
这样，大多数学生注册时不需要提供年龄和城市，只提供必须的两个参数：

>>> enroll('Sarah', 'F')
name: Sarah
gender: F
age: 6
city: Beijing
只有与默认参数不符的学生才需要提供额外的信息：

enroll('Bob', 'M', 7)
enroll('Adam', 'M', city='Tianjin')
可见，默认参数降低了函数调用的难度，而一旦需要更复杂的调用时，又可以传递更多的参数来实现。无论是简单调用还是复杂调用，函数只需要定义一个。

有多个默认参数时，调用的时候，既可以按顺序提供默认参数，比如调用enroll('Bob', 'M', 7)，意思是，除了name，gender这两个参数外，最后1个参数应用在参数age上，city参数由于没有提供，仍然使用默认值。

也可以不按顺序提供部分默认参数。当不按顺序提供部分默认参数时，需要把参数名写上。比如调用enroll('Adam', 'M', city='Tianjin')，意思是，city参数用传进去的值，其他默认参数继续使用默认值。

默认参数很有用，但使用不当，也会掉坑里。默认参数有个最大的坑，演示如下：

先定义一个函数，传入一个list，添加一个END再返回：

def add_end(L=[]):
    L.append('END')
    return L
当你正常调用时，结果似乎不错：

>>> add_end([1, 2, 3])
[1, 2, 3, 'END']
>>> add_end(['x', 'y', 'z'])
['x', 'y', 'z', 'END']
当你使用默认参数调用时，一开始结果也是对的：

>>> add_end()
['END']
但是，再次调用add_end()时，结果就不对了：

>>> add_end()
['END', 'END']
>>> add_end()
['END', 'END', 'END']
很多初学者很疑惑，默认参数是[]，但是函数似乎每次都“记住了”上次添加了'END'后的list。

原因解释如下：

Python函数在定义的时候，默认参数L的值就被计算出来了，即[]，因为默认参数L也是一个变量，它指向对象[]，每次调用该函数，如果改变了L的内容，则下次调用时，默认参数的内容就变了，不再是函数定义时的[]了。

！！！！定义默认参数要牢记一点：默认参数必须指向不变对象！

要修改上面的例子，我们可以用None这个不变对象来实现：
def add_end(L=None):
    if L is None:
        L = []
    L.append('END')
    return L
现在，无论调用多少次，都不会有问题：

>>> add_end()
['END']
>>> add_end()
['END']
为什么要设计str、None这样的不变对象呢？因为不变对象一旦创建，对象内部的数据就不能修改，这样就减少了由于修改数据导致的错误。此外，由于对象不变，多任务环境下同时读取对象不需要加锁，同时读一点问题都没有。我们在编写程序时，如果可以设计一个不变对象，那就尽量设计成不变对象。

###可变参数
在Python函数中，还可以定义可变参数。顾名思义，可变参数就是传入的参数个数是可变的，可以是1个、2个到任意个，还可以是0个。
我们以数学题为例子，给定一组数字a，b，c……，请计算a2 + b2 + c2 + ……。

要定义出这个函数，我们必须确定输入的参数。由于参数个数不确定，我们首先想到可以把a，b，c……作为一个list或tuple传进来，这样，函数可以定义如下：
def calc(numbers):
    sum = 0
    for n in numbers:
        sum = sum + n * n
    return sum
但是调用的时候，需要先组装出一个list或tuple：
>>> calc([1, 2, 3])
14
>>> calc((1, 3, 5, 7))
84

所以，我们把函数的参数改为可变参数：
def calc(*numbers):
    sum = 0
    for n in numbers:
        sum = sum + n * n
    return sum
定义可变参数和定义一个list或tuple参数相比，仅仅在参数前面加了一个*号。在函数内部，参数numbers接收到的是一个tuple，因此，函数代码完全不变。但是，调用该函数时，可以传入任意个参数，包括0个参数：
>>> calc(1, 2)
5
>>> calc()
0
如果已经有一个list或者tuple，要调用一个可变参数怎么办？可以这样做：
>>> nums = [1, 2, 3]
>>> calc(nums[0], nums[1], nums[2])
14
这种写法当然是可行的，问题是太繁琐，所以Python允许你在list或tuple前面加一个*号，把list或tuple的元素变成可变参数传进去：
>>> nums = [1, 2, 3]
>>> calc(*nums)
14
*nums表示把nums这个list的所有元素作为可变参数传进去。这种写法相当有用，而且很常见。

例：
# -*- coding: utf-8 -*- 
def calc(*numbers):   ##numbers为变量名
	sum = 0
	for n in numbers:
		sum = sum + n*n
	return sum 
	
sz=[10,20,30]	  #sz可以为list或tuple
print(calc(*sz))	
>>>Python py.py
1400

###关键字参数
可变参数允许你传入0个或任意个参数，这些可变参数在函数调用时自动组装为一个tuple。而关键字参数允许你传入0个或任意个含参数名的参数，这些关键字参数在函数内部自动组装为一个dict。请看示例：
def person(name, age, **kw):
    print('name:', name, 'age:', age, 'other:', kw)
函数person除了必选参数name和age外，还接受关键字参数kw。在调用该函数时，可以只传入必选参数：
>>> person('Michael', 30)
name: Michael age: 30 other: {}
也可以传入任意个数的关键字参数：
>>> person('Bob', 35, city='Beijing')
name: Bob age: 35 other: {'city': 'Beijing'}
>>> person('Adam', 45, gender='M', job='Engineer')
name: Adam age: 45 other: {'gender': 'M', 'job': 'Engineer'}

###命名关键字参数
如果要限制关键字参数的名字，就可以用命名关键字参数，例如，只接收city和job作为关键字参数。这种方式定义的函数如下：
def person(name, age, *, city, job):
    print(name, age, city, job)
和关键字参数**kw不同，命名关键字参数需要一个特殊分隔符*，*后面的参数被视为命名关键字参数。

调用方式如下：
>>> person('Jack', 24, city='Beijing', job='Engineer')
Jack 24 Beijing Engineer

如果函数定义中已经有了一个可变参数，后面跟着的命名关键字参数就不再需要一个特殊分隔符*了：
def person(name, age, *args, city, job):
    print(name, age, args, city, job)

命名关键字参数可以有缺省值，从而简化调用：
def person(name, age, *, city='Beijing', job):
    print(name, age, city, job)
由于命名关键字参数city具有默认值，调用时，可不传入city参数：
>>> person('Jack', 24, job='Engineer')
Jack 24 Beijing Engineer

使用命名关键字参数时，要特别注意，如果没有可变参数，就必须加一个*作为特殊分隔符。如果缺少*，Python解释器将无法识别位置参数和命名关键字参数：

def person(name, age, city, job):
    # 缺少 *，city和job被视为位置参数
    pass
###参数组合
!!!在Python中定义函数，可以用必选参数、默认参数、可变参数、关键字参数和命名关键字参数，这5种参数都可以组合使用。但是请注意，参数定义的顺序必须是：必选参数、默认参数、可变参数、命名关键字参数和关键字参数。
比如定义一个函数，包含上述若干种参数：
def f1(a, b, c=0, *args, **kw):
    print('a =', a, 'b =', b, 'c =', c, 'args =', args, 'kw =', kw)

def f2(a, b, c=0, *, d, **kw):
    print('a =', a, 'b =', b, 'c =', c, 'd =', d, 'kw =', kw)
在函数调用的时候，Python解释器自动按照参数位置和参数名把对应的参数传进去。

>>> f1(1, 2)
a = 1 b = 2 c = 0 args = () kw = {}
>>> f1(1, 2, c=3)
a = 1 b = 2 c = 3 args = () kw = {}
>>> f1(1, 2, 3, 'a', 'b')
a = 1 b = 2 c = 3 args = ('a', 'b') kw = {}
>>> f1(1, 2, 3, 'a', 'b', x=99)
a = 1 b = 2 c = 3 args = ('a', 'b') kw = {'x': 99}
>>> f2(1, 2, d=99, ext=None)
a = 1 b = 2 c = 0 d = 99 kw = {'ext': None}
最神奇的是通过一个tuple和dict，你也可以调用上述函数：

>>> args = (1, 2, 3, 4)
>>> kw = {'d': 99, 'x': '#'}
>>> f1(*args, **kw)
a = 1 b = 2 c = 3 args = (4,) kw = {'d': 99, 'x': '#'}
>>> args = (1, 2, 3)
>>> kw = {'d': 88, 'x': '#'}
>>> f2(*args, **kw)
a = 1 b = 2 c = 3 d = 88 kw = {'x': '#'}
所以，对于任意函数，都可以通过类似func(*args, **kw)的形式调用它，无论它的参数是如何定义的。
##iiii 虽然可以组合多达5种参数，但不要同时使用太多的组合，否则函数接口的可理解性很差。

#####小结
Python的函数具有非常灵活的参数形态，既可以实现简单的调用，又可以传入非常复杂的参数。
默认参数一定要用不可变对象，如果是可变对象，程序运行时会有逻辑错误！
要注意定义可变参数和关键字参数的语法：
*args是可变参数，args接收的是一个tuple；
**kw是关键字参数，kw接收的是一个dict。
以及调用函数时如何传入可变参数和关键字参数的语法：
可变参数既可以直接传入：func(1, 2, 3)，又可以先组装list或tuple，再通过*args传入：func(*(1, 2, 3))；
关键字参数既可以直接传入：func(a=1, b=2)，又可以先组装dict，再通过**kw传入：func(**{'a': 1, 'b': 2})。
使用*args和**kw是Python的习惯写法，当然也可以用其他参数名，但最好使用习惯用法。
命名的关键字参数是为了限制调用者可以传入的参数名，同时可以提供默认值。
定义命名的关键字参数在没有可变参数的情况下不要忘了写分隔符*，否则定义的将是位置参数。




</pre>