# Python笔记

> 参考自 官方文档（读官方文档才是最好的，第一手资料，也是我最需要反复阅读的）



## 基础

`>>>`: 提示符

`type()`函数: 返回参数的数据类型

```python
>>> type(2)
<class 'int'>
```



## 基础数据结构

### 列表

`List`

#### Use List as a stack

```python
>>> stack = [3, 4, 5]
>>> stack.append(6) # add an item to the top of the stack
>>> stack.append(7)
>>> stack
[3, 4, 5, 6, 7]
>>> stack.pop() # retrieve an item from the top of the stack
7
>>> stack
[3, 4, 5, 6]
>>> stack.pop()
6
>>> stack.pop()
5
>>> stack
[3, 4]
```



## 文件读写

`open()`会返回一个`文件对象`，open()一般会接受两个参数

```python
f = open('workfile', 'w') # 参数1是文件名，参数2是文件操作模式
```

| mode | function                     |
| ---- | ---------------------------- |
| r    | 只可读（默认模式）           |
| w    | 只可写，文件内容将会先被清空 |
| a    | 可在原内容的末尾开始添加内容 |
| r+   | 可读可写                     |



使用`with`关键字是在进行文件读写时的一个好习惯，它会在执行完成后自动地关闭文件



## Python 进程和线程



- [x] **并发和多线程的区别是什么？**

多线程，生活中地铁站开放多个检票通道就是多线程的例子，如果仅仅开放一个检票通道，由于检票所需的时间是固定的，一旦人流量一大，就可能会发生堵塞的情况，这样就很影响效率。

- [x] **并发和并行的区别**：

并发强调的是处理多个任务的能力，不强调同时性

并行强调**同时**处理多个任务的能力

我在吃饭时，电话响了，我等吃完饭才去接电话，说明我不支持并行，也不支持并发

我在吃饭时，电话响了，拿起手机，边打电话边吃饭，说明我支持**并行**，强调的是**同时**

我在吃饭时，电话响了，我放下筷子，接完了电话后，再继续吃饭，说明我支持**并发**

### 多进程

#### 模拟下载文件

**不使用多进程的情况**

```python
from random import randint
from time import time, sleep


def download_task(filename):
    print('开始下载%s...' % filename)
    time_to_download = randint(5, 10)
    sleep(time_to_download)
    print('%s下载完成！耗费了%d秒' % (filename, time_to_download))


def main():
    start = time()
    download_task('Python思想大全.pdf')
    download_task('Python数据结构.pdf')
    end = time()
    print('总共耗费了%.2f秒' % (end - start))


if __name__ == '__main__':
    main()
```

结果

```
开始下载Python思想大全.pdf...
Python思想大全.pdf下载完成！耗费了5秒
开始下载Python数据结构.pdf...
Python数据结构.pdf下载完成！耗费了5秒
总共耗费了10.12秒
>>> 
```

从这个程序中可以学到的东西，

- `random模块`中`randint`的使用

```python
a = random.randint(1, 10) # 生成在[1, 10]之间的一个整数
```

- `time模块`中`time`和`sleep`的用法

```python

```



**使用多进程的情况**

```python
from multiprocessing import Process # 从multiprocessing模块导入Process类
from os import getpid # 
from random import randint
from time import time, sleep


def download_task(filename):
    print('启动下载进程，进程号[%d].' % getpid())
    print('开始下载%s...' % filename)
    time_to_download = randint(5, 10)
    sleep(time_to_download)
    print('%s下载完成！耗费了%d秒' % (filename, time_to_download))


def main():
    start = time()
    
    # 创建进程对象
    # target参数传入的是进程运行后要执行的代码
    # args要求传入一个元组，元组中的元素是将要传递给函数的参数
    p1 = Process(target=download_task, args=('Python思想大全.pdf',))
    # 调用start()方法启动进程
    p1.start()
    
    p2 = Process(target=download_task, args=('Python数据结构.pdf', ))
    p2.start()
    
    # join()方法表示等待进程结束
    p1.join()
    p2.join()
    end = time()
    print('总耗费了%.2f秒.' % (end - start))


if __name__ == '__main__':
    main()
```

结果：下载任务同时启动

```
启动下载进程，进程号[16404].
开始下载Python思想大全.pdf...
启动下载进程，进程号[8304].
开始下载Python数据结构.pdf...
Python思想大全.pdf下载完成！耗费了6秒
Python数据结构.pdf下载完成！耗费了9秒
总耗费了9.24秒.
```



## 面向对象（高级）

### @property装饰器

在属性前加上单个下划线是在暗示属性是受保护的，不建议外界直接访问，但是在某些情况下，这样是不够安全的。

为了解决这个问题，可以使用`@property包装器`来包装`getter`和`setter`方法，使得对属性的访问安全且方便。

```python
class Person(object):

    def __init__(self, name, age):
        self._name = name
        self._age = age

    # 访问器 - getter方法
    @property
    def name(self):
        return self._name

    # 访问器 - getter方法
    @property
    def age(self):
        return self._age

    # 修改器 - setter方法
    @age.setter
    def age(self, age):
        self._age = age

    def play(self):
        if self._age <= 16:
            print('%s正在玩大富翁.' % self._name)
        else:
            print('%s正在玩斗地主.' % self._name)


def main():
    person = Person('卢昱晓', 12)
    person.play()
    person.age = 22
    person.play()
    person.name = '昱晓'

if __name__ == '__main__':
    main()
```

结果

```
卢昱晓正在玩大富翁.
卢昱晓正在玩斗地主.
Traceback (most recent call last):
	...
AttributeError: can't set attribute
```



### \__slots__魔法

Python支持在运行过程中为对象绑定新的属性和方法，如果想要限制对象可绑定的属性和方法的范围，可以用使用`__slots__ `变量来限定可绑定的属性和方法

```python
class Person(object):

    # 限定Person对象只能绑定_name, _age和_gender属性
    __slots__ = ('_name', '_age', '_gender')

    def __init__(self, name, age):
        self._name = name
        self._age = age

    @property
    def name(self):
        return self._name

    @property
    def age(self):
        return self._age

    @age.setter
    def age(self, age):
        self._age = age

    def play(self):
        if self._age <= 16:
            print('%s正在玩大富翁.' % self._name)
        else:
            print('%s正在玩斗地主.' % self._name)


def main():
    person = Person('特朗普', 12)
    person.play()
    person._gender = '男'
    person._is_gay = True


if __name__ == '__main__':
    main()
```

结果

```
...
特朗普正在玩大富翁.
...
AttributeError: 'Person' object has no attribute '_is_gay'
```



### 静态方法

静态方法可以直接被类调用，是属于类的方法

`静态方法`和`类方法`都可通过类直接调用

```python
from math import sqrt


class Triangle(object):

    def __init__(self, a, b, c):
        self._a = a
        self._b = b
        self._c = c

    # 包装为静态方法
    # 判断是否为三角形
    @staticmethod 
    def is_valid(a, b, c):
        return a + b > c and b + c > a and a + c > b

    def perimeter(self):
        return self._a + self._b + self._c

    def area(self):
        half = self.perimeter() / 2
        return sqrt(half * (half - self._a) *
                    (half - self._b) * (half - self._c))


def main():
    a, b, c = 3, 4, 5
    # 调用静态方法
    if Triangle.is_valid(a, b, c):
        t = Triangle(a, b, c)
        print(t.perimeter())
        # 可以通过给类发送参数来调用对象方法，但得传入该类对应的对象作为参数
        print(Triangle.perimeter(t))
        print(t.area())
        print(Triangle.area(t))
    else:
        print('无法构成三角形')


if __name__ == '__main__':
    main()
```

- [ ] 静态方法和类方法有什么区别？

### 类方法

类方法的第一个参数约定名为`cls`，代表当前类

> 类本身也是一个对象

```python
from time import time, localtime, sleep


class Clock(object):
	"""数字时钟"""
    def __init__(self, hour=0, minute=0, second=0):
        self._hour = hour
        self._minute = minute
        self._second = second

    # 包装类方法
    @classmethod 
    def now(cls):
        ctime = localtime(time())
        # 会发现，括号内的参数与对象的初始化方法中的形参一一对应
        return cls(ctime.tm_hour, ctime.tm_min, ctime.tm_sec)

    def run(self):
        self._second += 1
        if self._second == 60:
            self._second = 0
            self._minute += 1
            if self._minute == 60:
                self._minute = 0
                self._hour += 1
                if self._hour == 24:
                    self._hour = 0

    def show(self):
        """显示时间"""
        # 02d是啥意思
        return '%02d:%02d:%02d' % (self._hour, self._minute, self._second)

def main():
    clock = Clock.now()
    while True:
        print(clock.show())
        sleep(1)
        clock.run()


if __name__ == '__main__':
    main()
```



### 类之间的关系

类与类之间的关系有三种：

`is-a`：继承关系

`has-a`：关联关系，如汽车和引擎

`use-a`：依赖关系，如司机有一个驾驶汽车的方法，其中的参数用到了汽车，那么司机和汽车就是依赖关系

可以使用UML来进行面向对象建模，将类与类之间的关系用标准化的图形符号描述

可参阅： [《UML面向对象设计基础》](https://e.jd.com/30392949.html) 



### 继承

子类可以继承父类的属性和方法

```python
class Person(object):

    def __init__(self, name, age):
        self._name = name
        self._age = age

    @property
    def name(self):
        return self._name

    @property
    def age(self):
        return self._age

    @age.setter
    def age(self, age):
        self._age = age

    def play(self):
        print("%s正在玩耍" % self._name)

    def watch(self):
        if self._age >= 18:
            print('%s正在看电影' % self._name)
        else:
            print('%s在看动画片' % self._name)


class Student(Person):

    def __init__(self, name, age, grade):
        super().__init__(name, age)
        self._grade = grade

    @property
    def grade(self):
        return self._grade

    @grade.setter
    def grade(self, grade):
        self._grade = grade

    def study(self, course):
        print('%s的%s正在学习%s' % (self._grade, self._name, course))


class Teacher(Person):

    def __init__(self, name, age, title):
        super().__init__(name, age)
        self._title = title

    @property
    def title(self):
        return self.title

    @title.setter
    def title(self, title):
        self._title = title

    def teach(self, course):
        print('%s%s正在讲%s' % (self._name, self._title, course))


def main():
    stu = Student('王大锤', 15, '初三')
    stu.study('数学')
    stu.watch()
    t = Teacher('老王', 38, '砖家')
    t.teach('Python程序设计')
    t.watch()


if __name__ == '__main__':
    main()
```

结果

```
初三的王大锤正在学习数学
王大锤在看动画片
老王砖家正在讲Python程序设计
老王正在看电影

Process finished with exit code 0
```

从这个程序中可以学到的东西：

- 继承时，`super().__init__()`的调用



### 多态

多态和抽象类

```python
from abc import ABCMeta, abstractmethod

# 通过abc模块的ABCMeta元类和abstractmethod包装器来达到抽象类的效果
# Pet类是个抽象类
class Pet(object, metaclass=ABCMeta):

    def __init__(self, nickname):
        self._nickname = nickname

    # 包装成抽象方法
    @abstractmethod
    def make_voice(self):
        """发生声音"""
        pass


class Dog(Pet):
    """dog"""

    # 对Pet类中的make_voice方法进行重写
    def make_voice(self):
        print('%s: 汪汪汪...' % self._nickname)


class Cat(Pet):
    """cat"""

    # 对Pet类中的make_voice方法进行重写
    def make_voice(self):
        print('%s: 喵喵喵...' % self._nickname)


def main():
    pets = [Dog('旺财'), Cat('凯蒂'), Dog('大黄')]
    for pet in pets:
        pet.make_voice()


if __name__ == '__main__':
    main()
```

结果

```
旺财: 汪汪汪...
凯蒂: 喵喵喵...
大黄: 汪汪汪...
```



## 高级特性

### 切片

`slice`

为什么会有`切片`？

对于取指定范围索引的操作，如果用`for循环`或者`直接用手动输入`就太繁琐了，Python提供`切片`简化了这个过程。

```python
>>> L = ['Michael', 'Sarah', 'Tracy', 'Bob', 'Jack']

# 从索引0开始取，直到索引3为止，不包括3
>>> L[0:3]
['Michael', 'Sarah', 'Tracy']

>>> L[:3]
['Michael', 'Sarah', 'Tracy']
```

```python
>>> L = list(range(100))

# 所有数，每5个取一个
>>> L[::5]
```

`tuple`也可以用切片操作，操作的结果仍是tuple

```python
>>> (0, 1, 2, 3, 4, 5)[:3]
(0, 1, 2)
```

`字符串` `‘xxx’`看成是一种`list`，也可以进行切片操作，每个元素即为一个字符，操作结果仍为字符串

```python
>>> 'ABCDEFG'[:3]
'ABC'

# 每隔1个元素取一个
>>> 'ABCDEFG'[::2]
'ACEG'
```



### 迭代

用for循环遍历一个list或者tuple等`可迭代对象`的过程，称为`迭代`

Python的for循环抽象程度要高于C语言的for循环

在Python中迭代通过`for...in`来完成

只要是可迭代对象，无论是否具有该对象是否支持索引，都可以进行迭代，如字典

**对字典进行迭代**

```python
>>> d = {'a': 1, 'b': 2, 'c': 3}
>>> for key in d:
	     print(key)

a
c
b


```

默认情况下，`dict`迭代的是key，要对value进行迭代，可以使用

`for value in d.values()`

若要迭代key和value，可以用

`for k, v in d.items()`

**对字符串进行迭代**

字符串也是可迭代对象

```python
>>> for ch in 'ABC':
...     print(ch)
...
A
B
C
```

**判断一个对象是否为可迭代对象**

通过使用`collections`模块的`Iterable`类型来判断

```python
>>> from collections import Iterable
>>> isinstance('abc', Iterable) # str是否可迭代
True
>>> isinstance([1,2,3], Iterable) # list是否可迭代
True
>>> isinstance(123, Iterable) # 整数是否可迭代
False
```

**执行下标循环**

Python内置的`enumerate`函数可以把一个list变成`索引-元素对`

```python
>>> for i, value in enumerate(['A', 'B', 'C']):
...     print(i, value)
...
0 A
1 B
2 C
```



### 列表生成式

`list comprehensions`

Python内置的简单而强大的list生成式

本需要用多条语句组成的for循环结构生成的列表，现在只需要一条语句即可

```python
>>> [x * x for x in range(1, 11)]
[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]

# 可添加if条件判断，进行筛选
>>> [x * x for x in range(1, 11) if x % 2 == 0]
[4, 16, 36, 64, 100]

# 可使用两层循环
# 对于这个机制的一个应用是生成全排列
>>> [m + n for m in 'ABC' for n in 'XYZ']
['AX', 'AY', 'AZ', 'BX', 'BY', 'BZ', 'CX', 'CY', 'CZ']

# 可以使用多个变量来生成list
>>> d = {'x': 'A', 'y': 'B', 'z': 'C' }
>>> [k + '=' + v for k, v in d.items()]
['y=B', 'x=A', 'z=C']

# extra：将list中的所有字符串变成小写
>>> L = ['Hello', 'World', 'IBM', 'Apple']
>>> [s.lower() for s in L]
['hello', 'world', 'ibm', 'apple']
```

### 生成器

`generator`

我这笔记写得还不如直接看廖雪峰的资料，写得还比我这样写来的详细。我这笔记写得太多了，这也就意味着我还没有彻底理解生成器，直接看廖雪峰的教程吧。记笔记不需要急，除非有什么要特别记忆的东西，那么可以记到笔记中，否则没必要记。

**作用**：

列表生成式能够生成满足指定条件的一系列元素，但有时候我们只需要访问生成后的列表中的前几个元素，对于多生成的部分，就造成了内存空间的浪费。

生成器可以一个个地生成元素，不需要保存在列表中

**创建生成器的两个方法**：

方法一：将列表生成式改成生成器（`[]`换成`()`）

```
g = ((x * x) for x in range(10))
```

然后调用next()函数或者for循环来打印输出，一般用for循环来迭代会更方便些

```python
# 使用next()
# 每次调用next()，generator都会计算下一个值，直到计算到最后一个元素
# 如果已经到底，就会报错StopIteration
>>> next(g)
0
>>> next(g)
1
>>> next(g)
4
>>> next(g)
9
.
.
.
>>> next(g)
81
>>> next(g)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
StopIteration
```

```python
# for loop
>>> g = (x * x for x in range(10))
>>> for n in g:
...     print(n)
```



方法二：使用`yield`关键字（用函数实现包含复杂逻辑的generator）

如果函数定义中包含`yield`关键字，该函数就是一个`generator`

```python
def odd():
    print('step 1')
    yield 1
    print('step 2')
    yield(3)
    print('step 3')
    yield(5)
```

每次调用`next()`函数，执行到`yield`语句就返回，再次执行时从上次返回的`yield`语句处继续运行

```python
# 计算斐波那契数列
def fib(max):
    n, a, b = 0, 0, 1
    while n < max:
        yield b
        a, b = b, a + b
        n = n + 1
    return 'done'
```

```
>>> for n in fib(6):
...     print(n)
...
1
1
2
3
5
8
```

会发现上面程序执行完毕后没有打印返回值`'done'`，如果想要打印返回值，则需要捕获`StopIteration`错误，返回值包含在`StopIteration`的`value`中

```python
>>> g = fib(6)
>>> while True:
...     try:
...         x = next(g)
...         print('g:', x)
...     except StopIteration as e:
...         print('Generator return value:', e.value)
...         break
```



## 函数式编程

`Functional Programming `

### 高阶函数

#### map

`map()`函数

map()接收两个参数，一个是函数，一个是`Iterable`

map()的作用是将函数作用于`Iterable`中的每一个元素，并将结果作为`Iterator`返回

```python
>>> r = map(str, [1, 2, 3, 4, 5, 6])				       
>>> list(r) # r为Iterator，可以转换成list（也就是将结果全部都先计算出来）显示

['1', '2', '3', '4', '5', '6']
```

#### reduce



#### filter

`filter()`函数接收两个参数，一个是函数，一个是序列，filter将函数作用于序列中的每个元素，并根据返回值是`True`还是`False`来决定元素的去留，`False`则丢弃。

我称它为`过滤器`

```python
def is_odd(n):
    return n % 2 == 1

>>> list(filter(is_odd, [1, 2, 4, 5, 6, 9 ,10 ,15]))
[1, 5, 9, 15]
```



#### sorted

`sorted()`函数

```python
# sorted()函数可对list进行排序
>>> sorted([36, 5, -12, 9, -21])
[-21, -12, 5, 9, 36]

# 可接收一个key函数来自定义排序
>>> sorted([36, 5, -12, 9, -21], key=abs)
[5, 9, -12, -21, 36]

# 反向排序
>>> sorted([36, 5, -12, 9, -21], key=abs, reverse=True)
[36, -21, -12, 9, 5]

# 字符串排序（按ASCII码的大小进行排序）
>>> sorted(['bob', 'about', 'Zoo', 'Credit'])
['Credit', 'Zoo', 'about', 'bob']
```



## Python 算法

### 排序

#### 选择排序

`selection sort`	

**时间复杂度**：`O(n^2)`

n-1 + n-2 + n-3 + ... +1 约等于 n^2

**最坏的情况**：将一个反向排列的数组，排列成正向。如将 [3, 2, 1] 排列成 [1, 2, 3]

**代码实现**

```python
def selection_sort(list):
    i = 0
    while i < len(list) - 1: # do n - 1 searches
        min_index = i
        j = i + 1
        while j < len(list):
            if list[j] < list[min_index]:
                min_index = j
            j += 1
        if min_index != 1:
            temp = list[min_index]
            list[min_index] = list[i]
            list[i] = temp
        i += 1

list = [3, 2, 5, 1, 4]
selection_sort(list)
print(list)
```



#### 冒泡排序

`bubble sort`

```python
def selectionSort(list):
    i = 0
    while i < len(list) - 1: # do n - 1 searches
        minIndex = i
        j = i + 1
        while j < len(list):
            if list[j] < list[minIndex]:
                minIndex = j
            j += 1
        if minIndex != 1:
            temp = list[minIndex]
            list[minIndex] = list[i]
            list[i] = temp
        i += 1

list = [3, 2, 5, 1, 4]
selectionSort(list)
print(list)
```

### 冒泡排序

**时间复杂度**：`O(n^2)`

**代码实现**

```python
def bubble_sort(list):
    n = len(list)
    # do n - 1 bubbles
    while n > 1:
        i = 1
        while i < n:
            if list[i] < list[i - 1]:
                temp = list[i]
                list[i] = list[i - 1]
                list[i - 1] = temp
            i += 1
        n -= 1

list = [3, 2, 5, 1, 4]
bubble_sort(list)
print(list)
```



#### 插入排序

`insertion sort`

**时间复杂度**：`O(n^2)`

1 + 2 + ... + n-1 + n 约等于 n^2

**最坏的情况**：将倒序排列的序列排列成正序的，如 [4, 3, 2, 1] --> [1, 2, 3, 4]

**代码实现**

```python
def insertion_sort(list):
    i = 1
    while i < len(list):
        item_to_insert = list[i]
        j = i - 1
        while j >= 0:
            if item_to_insert < list[j]:
                list[j + 1] = list[j]
                j -= 1
            else:
                break
        list[j + 1] = item_to_insert
        i += 1

list = [3, 2, 5, 1, 4]
insertion_sort(list)
print(list)
```



## Python数据结构

### 链表

#### 单链表

```python
class Node:
	"""represents a singly linked node"""
    def __init__(self, data, next = None):
        self.data = data
        self.next = next


# an empty node
node1 = None
# a node containing data and an empty link
node2 = Node("A", None)
# containing data and a link to node2
node3 = Node("B", node2)

head = None
for count in range(1, 6):
    head = Node(count, head)
    
    
# 遍历并打印链表内的数据
# 注意，循环结束后链表中的元素都被删除了
while head != None:
    print(head.data)
    head = head.next


# 遍历链表
# 声明一个探针
probe = head
while probe != None:
    probe = probe.next
```

结果

```
5
4
3
2
1
```



## Python之禅

 Beautiful is better than ugly. （优美比丑陋好） 

Explicit is better than implicit.（清晰比晦涩好） 

Simple is better than complex.（简单比复杂好）

Complex is better than complicated.（复杂比错综复杂好） 

Flat is better than nested.（扁平比嵌套好） 

Sparse is better than dense.（稀疏比密集好） 

Readability counts.（可读性很重要） 

Special cases aren't special enough to break the rules.（特殊情况也不应该违反这些规则） 

Although practicality beats purity.（但现实往往并不那么完美）

Errors should never pass silently.（异常不应该被静默处理）

Unless explicitly silenced.（除非你希望如此） 

In the face of ambiguity, refuse the temptation to guess.（遇到模棱两可的地方，不要胡乱猜测） 

There should be one-- and preferably only one --obvious way to do it.（肯定有一种通常也是唯一一种最佳的解决方案） 

Although that way may not be obvious at first unless you're Dutch.（虽然这种方案并不是显而易见的，因为你不是那个荷兰人^这里指的是Python之父Guido^） 

Now is better than never.（现在开始做比不做好） 

Although never is often better than *right* now.（不做比盲目去做好^极限编程中的YAGNI原则^） 

If the implementation is hard to explain, it's a bad idea.（如果一个实现方案难于理解，它就不是一个好的方案） 

If the implementation is easy to explain, it may be a good idea.（如果一个实现方案易于理解，它很有可能是一个好的方案） 

Namespaces are one honking great idea -- let's do more of those!（命名空间非常有用，我们应当多加利用） 