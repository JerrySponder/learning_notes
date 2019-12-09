# Python 碎片知识点

`split()`方法：根据字符串创建一个列表

```python
>>> title = "Alice in Wonderland"
>>> title.split() # 默认以以空格为分隔符

['Alice', 'in', 'Wonderland']
```

`pass`语句：充当占位符或者表示什么都不要做

`import`

```python
# 使用方式 1
import database
db = database.Database()

# 使用方式 2 
from database import Database
db = Database()

# 使用方式 3
from database import Database as DB
db = DB()

# 使用方式 4
from database import Database, Query

# 使用方式 5 
# 永远不要使用这种方式，可读性极差，其他人根本不知道你导入了database模块中的什么内容，包括两年后的你
from database import *
```



#### 转义序列

《Learn Python the hard way》作者认为都需要记住

| 转义字符   | 功能                                                       |
| ---------- | ---------------------------------------------------------- |
| \\\        | 反斜杠 ( \\ )                                              |
| \\'        | 单引号 ( ' )                                               |
| \\"        | 双引号 ( '' )                                              |
| \\a        | ASCII响铃符 (BEL)                                          |
| \\b        | ASCII退格符 (BS)                                           |
| \\f        | ASCII进纸符 (FF)                                           |
| \\n        | ASCII换行符  (LF)                                          |
| \\N{name}  | Unicode数据库中的字符名，其中name是它的名字，仅适用Unicode |
| \\r        | ASCII回车符 (CR)                                           |
| \\t        | ASCII水平制表符 (TAB)                                      |
| \\uxxxx    | 值为16位十六进制值xxxx的字符 (仅适用Unicode)               |
| \Uxxxxxxxx | 值为32位十六进制值xxxxxxxx的字符 (仅适用Unicode)           |
| \v         | ASCII垂直制表符 (VT)                                       |
| \ooo       | 值为八进制值ooo的字符                                      |
| \xhh       | 值为十六进制值hh的字符                                     |



#### 在字符串中嵌入值

```python
>>> my_score = 1000
>>> message = 'I scored %s points'
>>> print(message % my_score)
I scored 1000 points
------------------------------------------------------
>>> print('I scored %s points' % my_score) # 与上面等效
I scored 1000 points
------------------------------------------------------
"""一次性嵌入多个值"""
>>> nums = 'What did the number %s say to the number %s? Nice belt!!'
>>> print(nums % (0, 8))
What did the number 0 say to the number 8? Nice belt!!
```

%s可以看成一个占位符，占据需要嵌入值的位置

%告诉Python把%s替换成保存在my_score变量中的值



#### 字符串乘法

```python
"""重复打印字符串"""
>>> print(10 * 'a')
aaaaaaaaaa
```





#### 列表上的算术

```python
"""连接两个列表"""
What did the number 0 say to the number 8? Nice belt!!
>>> list1 = [1, 2]
>>> list2 = ['I', 'Love', 'Python']
>>> print(list1 + list2)
[1, 2, 'I', 'Love', 'Python']
```

```python
"""将列表翻n倍"""
>>> list1 = [1, 2]
>>> print(list1 * 5)
[1, 2, 1, 2, 1, 2, 1, 2, 1, 2]
```



#### None

```python
my_value = None # my_value的值为空
```



#### turtle模块

用乌龟🐢来作画

```python
>>> import turtle
>>> t = turtle.Pen()
>>> t.forward(50) # 前进50个像素单位
>>> t.left(90) # 左转90°
>>> t.right(90) # 右转
>>> t.backward(50) # 后退50
>>> t.up() # 拿起乌龟（此时指针前进，不留下痕迹）
>>> t.down() # 放下乌龟
```

用for循环和turtle模块来玩一些几何图形，会非常有趣！比如下面这段代码可以画出的图形：

```python
>>> t.reset()
>>> for x in range(1, 20):
	t.forward(100)
	t.left(95)
```

<img src="imgs\python碎片知识点_p1.png" alt="1571238887415" style="zoom:67%;" />

```python
>>> t.reset()
>>> for x in range(1, 38):
	t.forward(100)
	t.left(175)
```

<img src="imgs\python碎片知识点_p2.png" alt="1571238887415" style="zoom:67%;" />

```python
>>> t.reset()
>>> for x in range(1, 19):
	t.forward(100)
	if x % 2 ==0:
		t.left(175)
	else:
		t.left(225)
```

<img src="imgs\python碎片知识点_p3.png" alt="1571239101711" style="zoom:50%;" />

#### time模块

该模块可以计算当前日期和时间

```python
>>> import time
>>> time.asctime()
'Wed Oct 16 22:05:10 2019'
```



#### Python的内建函数

```python
"""abs()函数"""
>>> print(abs(-3)) 
3

"""bool()函数"""
# 对于数字，bool()函数只有当参数为0时，才返回False，其他任何值都返回True
# 对于字符串，只有参数为空字符串和None才会返回False
# 对于列表、元组和字典，只有传入空列表或元组或字典的时候，才会返回False
>>> print(bool(0)) 
False
>>> print(bool(-1))
True
>>> print(bool(20.2))
True

???"""dir()函数"""
# dir是directory的简写
# dir()函数的用法：可以快速找出在一个变量上做些什么;然后就可以用help()函数来获得列表中某个函数的简短描述，比如下面列表中的upper
>>> popcorn = 'I love popcorn!'
>>> dir(popcorn)
['__add__', '__class__', '__contains__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__', '__getnewargs__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__iter__', '__le__', '__len__', '__lt__', '__mod__', '__mul__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__rmod__', '__rmul__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', 'capitalize', 'casefold', 'center', 'count', 'encode', 'endswith', 'expandtabs', 'find', 'format', 'format_map', 'index', 'isalnum', 'isalpha', 'isdecimal', 'isdigit', 'isidentifier', 'islower', 'isnumeric', 'isprintable', 'isspace', 'istitle', 'isupper', 'join', 'ljust', 'lower', 'lstrip', 'maketrans', 'partition', 'replace', 'rfind', 'rindex', 'rjust', 'rpartition', 'rsplit', 'rstrip', 'split', 'splitlines', 'startswith', 'strip', 'swapcase', 'title', 'translate', 'upper', 'zfill']
>>> help(popcorn.upper)
Help on built-in function upper:

upper(...) method of builtins.str instance
    S.upper() -> str
    Return a copy of S converted to uppercase.
 


```



## 文件

对于你想处理的数据量，Python并没有限制，只要你内存够大，想处理多少就处理多少

#### 目标文件

文件名：pi_string.txt

```
3.1415926535
    8979323846
    2643383279
```



#### 文件路径

`相对路径`：相对于程序当前所在的位置，同级则直接输入文件名即可

`绝对路径`：如 C:\User\ehmatthes\other_files\text_files\filename.txt （Windows）

Python中，Windows系统文件路径中使用的是反斜杠，但由于反斜杠可能会被误判为转义标记，所以最好使用**原始字符串**的方式指定路径



### 读取文件

#### 读取文件全部内容

```python
with open('pi_digits.txt') as file_object:
    contents = file_object.read() # 调用read()方法读取文件内容
    print(contents)
```

`open()`函数接受一个参数，即要打开的文件的名称，Python将会在当前程序运行的目录中查找指定的文件，然后将表示文件的对象返回file_object变量

在这个程序中，虽然没有调用使文件关闭的函数，但使用`with`语句块可以自动将文件关闭。

使用with语句块，即使在语句块中操作文件的过程中产生一些异常时，with语句块依然能妥当地将文件关闭

```python
# 可通过f.closed来查看文件是否已经关闭
>>> f.closed
True
```

也可以使用`close()`函数主动将文件在合适的时候关闭



#### 逐行读取文件内容

```python
with open('pi_digits.txt') as file_object:
    for line in file_object:
        print(line)
```

输出结果为

```
3.1415926535

    8979323846

    2643383279

# 因为在txt文件中每行内容后面都一个看不见的换行符，又print()函数也会加上一个换行符，所以有了空行，要想去掉空行，则可以在将上面程序的最后一个语句改为print(line.rstrip())
```



#### 创建包含文件内容的列表

open()返回的文件对象只能在with代码块中使用，如果想在with代码块外访问文件的内容的话，可以通过在代码块中将文件内容保存到列表中，然后在代码块外使用列表。

```python
file_name = 'pi_digits.txt'

with open(file_name) as file_object:
    lines = file_object.readlines() # readlines()读取文件的每行，并存储在列表中

for line in lines:
    print(line.rstrip())
```

```python
>>> lines
['3.1415926535\n', '    8979323846\n', '    2643383279']
```



### 写入文件

#### 写入空文件

```python
file_name = 'programming.txt'

# 如果文件不存在，open()会自动创建它（默认为当前目录下）
with open(file_name, 'w') as file_object: 
    file_object.write('I love Python!!!!')
```

| Mode     | Function                                                     |
| -------- | ------------------------------------------------------------ |
| **'r'**  | **read 读取模式**                                            |
| **‘w’**  | **(wipe and) write 写入模式**（会清空目标文件的内容后再写入） |
| **‘a’**  | **add 附加模式**（在原来文件内容的末尾处附加内容）           |
| **'r+'** | **读写模式**                                                 |



```Python
# Python只能将字符串写入文件中，想写入数据的话，就得先将数据转换为字符串。
file_name = 'programming.txt'

num = 1
with open(file_name, 'w') as file_object:
    file_object.write(num)
```

结果：

```
Traceback (most recent call last):
  File "C:/Users/lenovo/Desktop/file_reader.py", line 5, in <module>
    file_object.write(num)
TypeError: write() argument must be str, not int
```



#### 附加到文件

```python
file_name = 'programming.txt'

num = 1
with open(file_name, 'a') as file_object:
    file_object.write('I love Python!!!!')
```

文件原来内容：

```
I love Python!
```

运行代码后：

```
I love Python!I love Python!!!!
```

### Methods of File Objects

`f.read(size)`：读取`size`个的字符，并返回

当size为`负数`或者`被忽略`时，就意味着读取全部文件内容

```python
# pi_digits: 'This is the entire file.'
>>> f = open('pi_digits.txt')
>>> f.read(5)
'This '
>>> f.read(5)
'is th'
>>> f.read()
'e entire file.'
>>> f.read()
''
# 不断调用f.read()直到读完文件内容，读完以后再次调用，只会返回空字符串
```

`f.readline()`：读取单行文件内容

```python
'This is the first line of the file.\n'
>>> f.readline()
'Second line of the file\n'
>>> f.readline() # 读到文件末尾后，再次单行读取只返回一个空字符串
''
```

若想将所有文件内容分行保存在`list`中，则可以调用`f.readlines`或者`list(f)`

`f.write(string)` ：将string参数内容写入文件中，并返回写入了几个字符

```python
>>> f.write('This is a test\n') # newline character算一个字符
15
```



## 异常

异常用来管理程序执行期间发生的错误

Python中异常用`try-except代码块`或者`try-except-else代码块`处理

不要轻易地将异常的内容暴露给用户，如果用户比较坏，可能就会用这些信息搞破坏



```python
>>> print(5/0)

Traceback (most recent call last):
  File "<pyshell#0>", line 1, in <module>
    print(5/0)
ZeroDivisionError: division by zero
```

上面代码中`ZeroDivisionError`是一个`异常对象`，Python执行程序的过程中发生异常时，就会返回异常对象



#### try-except

```python
try:
    print(5/0)
except ZeroDivisionError:
    print("You can't divide by zero!")
```

结果：

```
You can't divide by zero!
```



#### try-except-else

将可能发生异常的代码放在`try代码块`中

将依赖于try代码块成功执行的代码放在`else语句块`中

将发生错误时描述该如何做的代码放在`except代码块`中

```python
first_number = input()
second_number = input()
try:
    answer = first_number / second_number
except ZeroDivisionError:
    print("You can't divide by zero!")
else:
	print(answer)
```



## 存储数据

### 存储 JSON 数据

`JSON`：`JavaScript Object Notation` 格式最初是为JavaScript开发的，但随后变成一种常见格式，被众多语言采用

`JSON`格式的数据可以与使用其他编程语言的人共享

`json`模块可将Python的数据结构转储到文件中，并可在程序再次运行时加载数据。



`dump()`方法：接受两个参数，要存储的数据和存储数据的文件对象

```python
import json

numbers = [2, 3, 5, 7, 11, 13]

filename = 'numbers.json'
with open(filename, 'w') as f_obj:
    json.dump(numbers, f_obj) # dump 转储
```



`load()`方法：加载存储在指定 json 文件中的信息

```python
import json

filename = 'numbers.json'
with open(filename) as f_obj:
    numbers = json.load(f_obj)

print(numbers)
```



其他例子

```python
import json

username = input("What is your name?")

filename = 'username.json'
with open(filename, 'w') as f_obj:
    json.dump(username, f_obj)
    print("We'll remember you when you come back, " + username + "!")
```



```python
import json

filename = 'username.json'

with open(filename) as f_obj:
    username = json.load(f_obj)
    print("Welcome back, " + username + "!")
```



## 测试



`断言方法`

各种断言方法：

| 方法                      | 用途               |
| ------------------------- | ------------------ |
| `assertEqual(a, b)`       | 核实 a == b        |
| `assertNotEqual(a, b)`    | 核实 a != b        |
| `assertTrue(x)`           | 核实x为True        |
| `assertFalse(x)`          | 核实x为False       |
| `assertIn(item, list)`    | 核实item在list中   |
| `assertNotIn(item, list)` | 核实item不在list中 |



```python
def get_formatted_name(first, last):
    full_name = first + ' ' + last
    return full_name.title()
```

针对函数的测试

测试通过时

```python
import unittest

class NamesTestCase(unittest.TestCase):
    """"""
    def test_first_last_name(self):
        formatted_name = get_formatted_name('janis', 'joplin')
        self.assertEqual(formatted_name, 'Janis Joplin')

def get_formatted_name(first, last):
    full_name = first + ' ' + last
    return full_name.title()

unittest.main()
```



```
.
----------------------------------------------------------------------
Ran 1 test in 0.042s

OK
```



测试不通过时



针对类的测试

```python
class AnonymousSurvey():
    """收集匿名调查问卷的答案"""

    def __init__(self, question):
        """存储一个问题，并为存储答案做准备"""
        self.question = question
        self.responses = []

    def show_question(self):
        """显示调查问卷"""
        print(self.question)
    
    def store_response(self, new_response):
        """存储单份调查问卷"""
        self.responses.append(new_response)

    def show_results(self):
        """显示收集到的所有答卷"""
        print("Survey results:")
        for response in self.responses:
            print('- ' + response)

question = "What language did you first learn to speak?"
my_survey = AnonymousSurvey(question)

my_survey.show_question()
print("Enter 'q' at any time to quit.\n")
while True:
    response = input("Language: ")
    if response == 'q':
        break
    my_survey.store_response(response)

print("\nThank you to everyone who participated in the survey!")
my_survey.show_results()
```



```python
import unittest

class AnonymousSurvey():
    """收集匿名调查问卷的答案"""

    def __init__(self, question):
        """存储一个问题，并为存储答案做准备"""
        self.question = question
        self.responses = []

    def show_question(self):
        """显示调查问卷"""
        print(self.question)
    
    def store_response(self, new_response):
        """存储单份调查问卷"""
        self.responses.append(new_response)

    def show_results(self):
        """显示收集到的所有答卷"""
        print("Survey results:")
        for response in self.responses:
            print('- ' + response)


class TestAnonymousSurvey(unittest.TestCase):
    """针对AnonymousSurvey类的测试"""
    def test_store_single_response(self): # 对AnonymousSurvey类的行为的一方面进行验证
        question = "What language did you first learn to speak?"
        my_survey = AnonymousSurvey(question)
        my_survey.store_response('English')

        self.assertIn('English', my_survey.responses)

    def test_store_three_response(self):
        question = "What language did you first learn to speak?"
        my_survey = AnonymousSurvey(question)
        responses = ['English', 'Spanish', 'Mandarin']
        for response in responses:
            my_survey.store_response(response)
        for response in responses:
            self.assertIn(response, my_survey.responses)

unittest.main()
```



# 一起来做游戏！！

使用的模块：`Pygame`

## 关于游戏素材

为游戏选择素材时，务必注意是否获得了许可！！

推荐素材网站：[https://pixabay.com](https://pixabay.com/) 无需许可即可使用，但别忘了感谢作者！

## 面向对象

在软件开发中，`对象`是指数据及其行为的集合

`数据`描述的是对象的个体特征

`方法`描述的是个体的行为

描述对象的类型称为`类`

`面向对象`的意思可以理解为`功能性地指向建模对象`

面向对象包括：

- `面向对象分析` `OOA`：是针对问题、系统或任务的过程，并确定所需的对象及对象之间的交互关系。该阶段关注的是**需要完成什么**，输出的是**一系列的需求**
- `面向对象设计` `OOD`：是将需求转化为实现方案的过程。该阶段关注的是**如何完成**，输出的是**实现方案**
- `面向对象编程``OOP`：是将经过定义的设计转换为可运行程序的过程

- [ ] `UML`：`统一建模语言`，用于描述类之间的关系，浅显直观，便于沟通交流，**另作了解**

- [ ] `对象图`：又称`实例图`，描述的是系统在某一时刻的特定状态，以及对象的特定实例，而不是类与类之间的交互，**另作了解**

`封装`：

被封装的数据不一定非要是隐藏的。封装好比是创建一个容器，不妨将其想象成一个时间胶囊，可以把一堆信息放在胶囊里，掩埋起来，不让别人看见内部的细节；也可以不掩藏起来，好比是用透明材料制作的一般。



`抽象`：

只处理与给定任务相关的最必要的一层细节，是从内部细节中提取公共接口的过程。

**Q：什么是公共接口？**

在设计接口时，尽量将自己置身于对象的位置，并想象对象对**隐私**有特别强烈的偏好。尽可能不要让对象获取你的数据，除非你觉得这么做对你有利。不要让它们能够强制你执行特定任务的接口。



类之间的基本关系：

`关联`：如同学与同学之间的关系

`组合`：如一辆车和车的配件之间的关系

`聚合`：与组合这个概念的区别在于，聚合的对象可以单独存在，如棋盘和棋子的关系，棋盘没了，棋子还是可以存在的。但如果是组合关系的话，一个人死去了，他的器官也会跟着死去

`继承`：“是一个”。比如小明是一个人，意味着`小明`这个类可以继承自`人`这个类



**文档注释**

《Python面向对象编程》p34

```python
class Point:
    'hello.....'
    "hello...."
```



调用`help()`函数，将会看到类的具体格式文档

```
>>> help(Point)
Help on class Point in module __main__:

class Point(builtins.object)
 |  hello.....
 |  
 |  Data descriptors defined here:
 |  
 |  __dict__
 |      dictionary for instance variables (if defined)
 |  
 |  __weakref__
 |      list of weak references to the object (if defined)
```

### 面向对象编程

**可以自由地给一个实例变量绑定属性，并且绑定后的属性只与该实例变量有关**

```python
class Student(object):
    pass
    
>>> bart = Student()
>>> bart.name = 'Bart Simpson'
>>> bart.name
'Bart Simpson'
```



`__init__`方法

```python
class Student(object):

    def __init__(self, name, score): # self参数表示创建的实例本身
        self.name = name
        self.score = score
```

`__init__`方法与普通函数的区别：前者与后者的区别在于，前者的第一个参数永远必须是`self`，除此之外两者没什么区别，也就是说，前者可以像后者一样使用默认参数、可变参数、关键字参数和命名关键字参数



### 访问限制

如果想让内部属性不被外部访问，可以在属性名前加上两个下划线`__`，这意味着该属性变成了`私有变量`，只有内部可以访问

（双下划线开头的属性其实仍可以从外部访问，`__name`不能直接从外部访问，其实是Python把该变量的变量名改为了 `_String__name `，但是得注意，不同版本的Python解释器可能会把`__name`改成不同的变量名）

```python
class Student(object):

    def __init__(self, name, score):
        self.__name = name
        self.__score = score

    def print_score(self):
        print('%s: %s' % (self.__name, self.__score))
        
        
        
>>> bart = Student('Bart Simpson', 59)
>>> bart.__name
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'Student' object has no attribute '__name'
```

若外部代码想要获取或者修改内部属性值的话，可以给类添加`getter`和`setter`方法

```python
class Student(object):
    ...

    def get_name(self):
        return self.__name

    def get_score(self):
        return self.__score
        
    def set_score(self, score):
        self.__score = score
```

使用`setter`的一个好处是可以对传入的参数进行检查，避免传入无效的参数

Python中，变量名类似`__xxx__`的变量属于特殊的变量，在外部是可以访问的

以一个下划线开头的内部属性，如`_score`其在外部是可以访问的，但是按照约定俗成的规定，这样的变量的意思是，“虽然可以访问，但是请把我当成私有变量，最好不要访问我”

Python本身没有任何机制组织你干坏事，一切都靠自觉

