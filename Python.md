# Python

## Python 缺点

### 效率低

- Python是解释性语言，运行时要一行行翻译成机器码

### 代码不能加密

- 解释性语言的程序发布的时候必须把源码发布

## 基础

- 注释`# explain ` 

- python语法采用缩进方式

  ```python
  if a >= 0:
      print a
  else:
      print -a
  ```

  - 每一行都是一个语句
  - 当语句以冒号“:”结尾时，缩进的语句视为代码块

### 输入输出

- 输出 `print 'hello world'`
- 输入` raw_input()`/`raw_input('输入前的提示信息')`

### 数据类型

- 整数

- 浮点数

- 字符串

  使用单引号或者双引号包围

- 布尔值

  可以用 and 、or 、not进行运算

- 空值

  用`None`表示

- 变量

  可以指向任意类型

### 字符串和编码

- 编码

  - ASCII：用一个字节来储存
  - Unicode：通常是两个字节来储存，非常偏僻的字是四个字节
  - UTF-8：根据不同的数字大小编码成1-6个字节，“可变长编码”，英文字母通常被编码成1个字节，汉子通常是3个字节

  当源代码中包含中文的时候，在保存源代码时，就需要务必指定保存为UTF-8编码。当Python解释器读取源代码时，为了让它按UTF-8编码读取，我们通常在文件开头写上这两行：

  ```
  #!/usr/bin/env python
  # -*- coding: utf-8 -*-

  ```

  第一行注释是为了告诉Linux/OS X系统，这是一个Python可执行程序，Windows系统会忽略这个注释；

  第二行注释是为了告诉Python解释器，按照UTF-8编码读取源代码，否则，你在源代码中写的中文输出可能会有乱码。

- 字符串

  - 格式化

    - 常见的占位符有：

      | %d   | 整数     |
      | ---- | ------ |
      | %f   | 浮点数    |
      | %s   | 字符串    |
      | %x   | 十六进制整数 |

      eg：

      ```python
      >>> 'Hello, %s' % 'world'
      'Hello, world'
      >>> 'Hi, %s, you have $%d.' % ('Michael', 1000000)
      'Hi, Michael, you have $1000000.'
      ```

    - 其中，格式化整数和浮点数还可以指定是否补0和整数与小数的位数：

      ```
      >>> '%2d-%02d' % (3, 1)
      ' 3-01'
      >>> '%.2f' % 3.1415926
      '3.14'
      ```

      如果你不太确定应该用什么，**`%s`永远起作用**，它会把任何数据类型转换为字符串：

      ```
      >>> 'Age: %s. Gender: %s' % (25, True)
      'Age: 25. Gender: True'

      ```

      对于Unicode字符串，用法完全一样，但最好确保替换的字符串也是Unicode字符串：

      ```
      >>> u'Hi, %s' % u'Michael'
      u'Hi, Michael'
      ```

### 使用list和tuple

#### list

- `classmates = ['first', 'second', 'third']`

- `len(classmates)` -> 3

- `classmate[0]` -> first  按照索引获得结合元素

- `classmate[-1]` -> third 按照索引反向获得集合中元素

- `classmates.append('four')` 添加集合中元素

- `classmates.insert(1, 'insert')` 插入指定位置

- `classmates.pop()` -> 最后一个被删除，并返回最后一个元素

- `classmates.pop(1)` -> 删除指定位置元素

- list里面的元素的数据类型也可以不同

  `L = ['Apple', 123, True]`

- 可以看作数组取值

  ```python
  p = ['asp', 'php']
  s = ['python', 'java', p, 'scheme']
  # 可以通过p[1]取得 'php'
  # 可以通过s[2][2]取得 'php'
  ```

#### tuple

另一种有序列表叫元组：tuple。tuple和list非常类似，但是tuple一旦初始化就不能修改

`classmates = ('first', 'second', 'third')`

> 现在，classmates这个tuple不能变了，它也没有append()，insert()这样的方法。其他获取元素的方法和list是一样的，你可以正常地使用`classmates[0]`，`classmates[-1]`，但不能赋值成另外的元素。
>
> **因为tuple不可变，所以更安全**

- 如果要定义一个空的tuple，可以写成`()`：`t = ()`

- 要定义一个只有1个元素的tuple

  要定义一个只有1个元素的tuple，如果你这么定义：

  ```
  >>> t = (1)
  >>> t
  1

  ```

  定义的不是tuple，是`1`这个数！这是因为括号`()`既可以表示tuple，又可以表示数学公式中的小括号，这就产生了歧义，因此，Python规定，这种情况下，按小括号进行计算，计算结果自然是`1`。

  所以，只有1个元素的tuple定义时必须加一个逗号`,`，来消除歧义：

  ```
  >>> t = (1,)
  >>> t
  (1,)

  ```

  Python在显示只有1个元素的tuple时，也会加一个逗号`,`，以免你误解成数学计算意义上的括号。

### 使用dict和set

#### dict

`d = {'Michael': 95, 'Bob': 75, 'Tracy': 85}`

- 添加元素：`d['Adam'] = 67`

- 判断key是否存在：`'Thomas' in d`

- 根据key取出元素：

  dict提供的get方法，如果key不存在，可以返回None，或者自己指定的value

  返回None：`d.get('Thomas')`

  返回自己指定的值-1：`d.get('Thomas', -1)`

- 删除key：`pop(key)`

- key：不可以是不可变对象，比如list

#### set

`s = set([1, 2, 3])`

- 添加元素：`add(key)`

- 删除元素：`remove(key)`

- set可以看成数学意义上的无序和无重复元素的集合，因此，两个set可以做数学意义上的交集、并集等操作：

  ```python
  >>> s1 = set([1, 2, 3])
  >>> s2 = set([2, 3, 4])
  >>> s1 & s2
  set([2, 3])
  >>> s1 | s2
  set([1, 2, 3, 4])
  ```

- 元素不可以是不可变对象，比如list

### 条件判断和循环

#### 条件判断

```python
if <条件判断1>:
    <执行1>
elif <条件判断2>:
    <执行2>
elif <条件判断3>:
    <执行3>
else:
    <执行4>
```

- `if`判断条件还可以简写，比如写：

  ```
  if x:
      print 'True'
  ```

  只要`x`是非零数值、非空字符串、非空list等，就判断为`True`，否则为`False`。

#### 循环

- `for x in list/tuple`

  ```python
  names = ['Michael', 'Bob', 'Tracy']
  for name in names:
      print name
  ```

  > range(101)就可以生成0-100的整数序列


- `while`

  ```python
  sum = 0
  n = 99
  while n > 0:
      sum = sum + n
      n = n - 2
  print sum
  ```

## 函数

#### 调用函数

- abs(n)：绝对值
- cmp(a, b)：比较函数

#### 数据类型转换

Python内置的常用函数还包括数据类型转换函数，比如`int()`函数可以把其他数据类型转换为整数：

```
>>> int('123')
123
>>> int(12.34)
12
>>> float('12.34')
12.34
>>> str(1.23)
'1.23'
>>> unicode(100)
u'100'
>>> bool(1)
True
>>> bool('')
False
```

函数名其实就是指向一个函数对象的引用，完全可以把函数名赋给一个变量，相当于给这个函数起了一个“别名”：

```
>>> a = abs # 变量a指向abs函数
>>> a(-1) # 所以也可以通过a调用abs函数
1
```

#### 定义函数

**def定义函数，函数返回值是tuple**

```python
def my_abs(x):
    if x >= 0:
        return x
    else:
        return -x
```

- 空函数

  如果想定义一个什么事也不做的空函数，可以用`pass`语句：

  ```
  def nop():
      pass

  ```

  `pass`语句什么都不做，那有什么用？实际上`pass`可以用来作为占位符，比如现在还没想好怎么写函数的代码，就可以先放一个`pass`，让代码能运行起来。

  `pass`还可以用在其他语句里，比如：

  ```
  if age >= 18:
      pass
  ```

- 返回多个值

  ```python
  def move(x, y, step, angle=0):
      nx = x + step * math.cos(angle)
      ny = y - step * math.sin(angle)
      return nx, ny
  ```

  ```python
  x, y = move(100, 100, 60, math.pi / 6)
  ```

  但其实这只是一种假象，Python函数返回的仍然是单一值：

  ```python
  >>> r = move(100, 100, 60, math.pi / 6)
  >>> print r
  (151.96152422706632, 70.0)
  ```

  > 在语法上，返回一个tuple可以省略括号，而多个变量可以同时接收一个tuple，按位置赋给对应的值

### 函数的参数

- 默认参数

  ```python
  def enroll(name, gender, age=6, city='Beijing'):
      print 'name:', name
      print 'gender:', gender
      print 'age:', age
      print 'city:', city
  ```

  只有与默认参数不符的调用才需要提供额外的信息

  ```python
  enroll('Bob', 'M', 7)
  enroll('Adam', 'M', city='Tianjin')
  ```

  > 默认参数的坑：
  >
  > ```python
  > def add_end(L=[]):
  >     L.append('END')
  >     return L
  > ```
  >
  > 当你正常调用时，结果似乎不错：
  >
  > ```python
  > >>> add_end([1, 2, 3])
  > [1, 2, 3, 'END']
  > >>> add_end(['x', 'y', 'z'])
  > ['x', 'y', 'z', 'END']
  > ```
  >
  > 当你使用默认参数调用时，一开始结果也是对的：
  >
  > ```python
  > >>> add_end()
  > ['END']
  > ```
  >
  > 但是，再次调用`add_end()`时，结果就不对了：
  >
  > ```python
  > >>> add_end()
  > ['END', 'END']
  > >>> add_end()
  > ['END', 'END', 'END']
  > ```

  > 原因解释如下：
  >
  > Python函数在定义的时候，默认参数`L`的值就被计算出来了，即`[]`，因为默认参数`L`也是一个变量，它指向对象`[]`，每次调用该函数，如果改变了`L`的内容，则下次调用时，默认参数的内容就变了，不再是函数定义时的`[]`了。
  >
  > 所以，定义默认参数要牢记一点：默认参数必须指向不变对象！
  >
  > 要修改上面的例子，我们可以用`None`这个不变对象来实现：
  >
  > ```python
  > def add_end(L=None):
  >     if L is None:
  >         L = []
  >     L.append('END')
  >     return L
  > ```

#### 可变参数

```python
def calc(*numbers):
    sum = 0
    for n in numbers:
        sum = sum + n * n
    return sum
```

调用该函数时，可以传入任意个参数，包括0个参数：

```python
>>> calc(1, 2)
5
>>> calc()
0
```

如果已经有一个list或者tuple，要调用一个可变参数怎么办？

```python
>>> nums = [1, 2, 3]
>>> calc(*nums)
14
```

#### 关键字参数

关键字参数允许你传入0个或任意个含参数名的参数，这些关键字参数在函数内部自动组装为一个dict

```python
def person(name, age, **kw):
    print 'name:', name, 'age:', age, 'other:', kw
```

```python
>>> person('Adam', 45, gender='M', job='Engineer')
name: Adam age: 45 other: {'gender': 'M', 'job': 'Engineer'}
```

也可以先组装出一个dict，然后，把该dict传进去

```python
>>> kw = {'city': 'Beijing', 'job': 'Engineer'}
>>> person('Jack', 24, **kw)
name: Jack age: 24 other: {'city': 'Beijing', 'job': 'Engineer'}
```

#### 参数组合

在Python中定义函数，可以用必选参数、默认参数、可变参数和关键字参数，这4种参数都可以一起使用，或者只用其中某些，但是请注意，参数定义的顺序必须是：必选参数、默认参数、可变参数和关键字参数。

```python
def func(a, b, c=0, *args, **kw):
    print 'a =', a, 'b =', b, 'c =', c, 'args =', args, 'kw =', kw
```

最神奇的是通过一个tuple和dict，你也可以调用该函数：

```python
>>> args = (1, 2, 3, 4)
>>> kw = {'x': 99}
>>> func(*args, **kw)
a = 1 b = 2 c = 3 args = (4,) kw = {'x': 99}
```

所以，对于任意函数，都可以通过类似`func(*args, **kw)`的形式调用它，无论它的参数是如何定义的。

### 递归函数

解决递归调用栈溢出的方法是通过**尾递归**优化，事实上**尾递归和循环的效果是一样的**，所以，把循环看成是一种特殊的尾递归函数也是可以的。

尾递归是指，在函数返回的时候，调用自身本身，并且，**return语句不能包含表达式**。这样，编译器或者解释器就可以把尾递归做优化，使递归本身无论调用多少次，都只占用一个栈帧，不会出现栈溢出的情况。

## 高级特性

### 切片

取一个list或tuple的部分元素

- `L = ['Michael', 'Sarah', 'Tracy', 'Bob', 'Jack']`

```python
>>> L[0:3]
['Michael', 'Sarah', 'Tracy']
>>> L[:3]
['Michael', 'Sarah', 'Tracy']
>>> L[-2:]
['Bob', 'Jack']
>>> L[-2:-1]
['Bob']
```

- `L = range(100)`

前10个数，每两个取一个：

```python
>>> L[:10:2]
[0, 2, 4, 6, 8]
```

所有数，每5个取一个：

```python
>>> L[::5]
[0, 5, 10, 15, 20, 25, 30, 35, 40, 45, 50, 55, 60, 65, 70, 75, 80, 85, 90, 95]
```

甚至什么都不写，只写`[:]`就可以原样复制一个list：

```python
>>> L[:]
[0, 1, 2, 3, ..., 99]
```

- tuple也是一种list，唯一区别是tuple不可变。因此，tuple也可以用切片操作，只是操作的结果仍是tuple：

  ```python
  >>> (0, 1, 2, 3, 4, 5)[:3]
  (0, 1, 2)
  ```

- 字符串`'xxx'`或Unicode字符串`u'xxx'`也可以看成是一种list，每个元素就是一个字符。因此，字符串也可以用切片操作，只是操作结果仍是字符串：

  ```python
  >>> 'ABCDEFG'[:3]
  'ABC'
  >>> 'ABCDEFG'[::2]
  'ACEG'
  ```

### 迭代

list这种数据类型虽然有下标，但很多其他数据类型是没有下标的，但是，**只要是可迭代对象，无论有无下标，都可以迭代**，比如dict就可以迭代：

```python
>>> d = {'a': 1, 'b': 2, 'c': 3}
>>> for key in d:
...     print key
...
a
c
b
```

因为dict的存储不是按照list的方式顺序排列，所以，迭代出的结果顺序很可能不一样。

默认情况下，dict迭代的是key。如果要迭代value，可以用`for value in d.itervalues()`，如果要同时迭代key和value，可以用`for k, v in d.iteritems()`。

由于字符串也是可迭代对象，因此，也可以作用于`for`循环：

```python
>>> for ch in 'ABC':
...     print ch
...
A
B
C
```

所以，当我们使用`for`循环时，只要作用于一个可迭代对象，`for`循环就可以正常运行，而我们不太关心该对象究竟是list还是其他数据类型。

那么，如何判断一个对象是可迭代对象呢？方法是通过collections模块的Iterable类型判断：

```python
>>> from collections import Iterable
>>> isinstance('abc', Iterable) # str是否可迭代
True
>>> isinstance([1,2,3], Iterable) # list是否可迭代
True
>>> isinstance(123, Iterable) # 整数是否可迭代
False
```

最后一个小问题，如果要对list实现类似Java那样的下标循环怎么办？Python内置的`enumerate`函数可以把一个list变成索引-元素对，这样就可以在`for`循环中同时迭代索引和元素本身：

```python
>>> for i, value in enumerate(['A', 'B', 'C']):
...     print i, value
...
0 A
1 B
2 C
```

上面的`for`循环里，同时引用了两个变量，在Python里是很常见的，比如下面的代码：

```python
>>> for x, y in [(1, 1), (2, 4), (3, 9)]:
...     print x, y
...
1 1
2 4
3 9
```

### 列表生成式

列表生成式即List Comprehensions，是Python内置的非常简单却强大的可以用来创建list的生成式。

- `对x的骚操作 for x in range(a, b) if 条件`
  - 筛选出仅偶数的平方：

    ```python
    >>> [x * x for x in range(1, 11) if x % 2 == 0]
    [4, 16, 36, 64, 100]
    ```

- 使用两层循环

  ```python
  >>> [m + n for m in 'ABC' for n in 'XYZ']
  ['AX', 'AY', 'AZ', 'BX', 'BY', 'BZ', 'CX', 'CY', 'CZ'
  ```

- 列表生成式也可以使用两个变量来生成list：

  ```python
  >>> d = {'x': 'A', 'y': 'B', 'z': 'C' }
  >>> [k + '=' + v for k, v in d.iteritems()]
  ['y=B', 'x=A', 'z=C']
  ```

### 生成器

如果列表元素可以按照某种算法推算出来，就不必创建完整的list，从而节省大量的空间。

在Python中，这种一边循环一边计算的机制，称为生成器（Generator）。

- 这个生成器也是一个可迭代对戏那个


- 创建生成器

  - 方法一：只要把一个列表生成式的`[]`改成`()`，就创建了一个generator

    ```python
    >>> L = [x * x for x in range(10)]
    >>> L
    [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
    >>> g = (x * x for x in range(10))
    >>> g
    <generator object <genexpr> at 0x104feab40>
    ```

    - 调用`g.next()`，就计算出下一个元素的值，直到计算到最后一个元素，没有更多的元素时，抛出StopIteration的错误

  - 方法二：函数定义中包含`yield`关键字，那么这个函数就不再是一个普通函数，而是一个generator

    > generator的函数
    >
    > - 在每次调用`next()`的时候执行，
    > - 遇到`yield`语句返回
    > - 再次执行时从上次返回的`yield`语句处继续执行

    ```python
    >>> def odd():
    ...     print 'step 1'
    ...     yield 1
    ...     print 'step 2'
    ...     yield 3
    ...     print 'step 3'
    ...     yield 5
    ...
    >>> o = odd()
    >>> o.next()
    step 1
    1
    >>> o.next()
    step 2
    3
    >>> o.next()
    step 3
    5
    >>> o.next()
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    StopIteration
    ```

    - 对于函数改成的generator来说，遇到return语句或者执行到函数体最后一行语句，就是结束generator的指令，`for`循环随之结束

## 函数式编程

函数式编程的一个特点就是，允许把函数本身作为参数传入另一个函数，还允许返回一个函数

#### 高阶函数

编写高阶函数，就是让函数的参数能够接收别的函数。

一个最简单的高阶函数：

```python
def add(x, y, f):
    return f(x) + f(y)
```

## 模块



## 面向对象编程

- 所有的数据类型都可以视为对象

### 类和实例

```python
# 创建一个类
# 每一个方法都要有一个self参数，调用的时候会自动传进去
class Student(object):
    # 相当于java的构造方法->数据也是可以直接在这里封装
    def __init__(self, name, score):
        self.name = name
        self.score = score

    def print_message(self):
        print '%s:%s' % (self.name, self.score)

# 创建对象
stu1 = Student('huang', '100')

# 调用对象的方法
stu1.print_message()

# 直接操作对象内部的数据
print stu1.name + stu1.score
```

`class Student(Object)` :(Object)指定继承关系

- python允许对实例变量绑定任何数据

```python
# 心血来潮，本来是没有abcd这个域的
stu1.abcd = 123
```

### 访问权限

- 私有内部属性

把实例变量改成以`__`开头即可

```python
class Student(object):
    def __init__(self, name, score):
        self.__name = name
        self.__score = score

    def print_message(self):
        print '%s:%s' % (self.__name, self.__score)
```

- 被私有的内部变量仍有办法从外部访问

```python
# 创建对象
stu1 = Student('huang', '100')

# 直接操作对象内部的数据
print stu1._Student__name
```

### 继承和多态

- 继承

```python
class Animal(object):
    def run(self):
        print 'run animal'

# pass 表示不做任何事情
class Dog(Animal):
    pass

Dog().run()
```

- 其他的和JAVA差不多

### 获取对象信息

- 使用type()查看对象类型

`type(变量/类/方法/函数)`

判断对象类型：

```python
type(u'abc')==types.UnicodeType
```

- 使用instance()判断对象是否属于某个类型

- 使用dir(object)获得对象的所有属性和方法

  - 类似`__xxx__`的属性和方法在python中是由特殊用途的，例如\__len__方法返回长度

    ```python
    len('ABC') 
    'ABC'.__len__()
    # 两者是等价的
    ```

- 直接操作一个对象的状态

  - `hassttr(obj, 'x')`有属性x吗
  - `setattr(obj, 'y', 19) `设置一个属性y
  - `getattr(obj, 'y')`获取属性y
  - `getattr(obj, 'z', 404)` 获取属性y，默认值为404，如果属性值不存在，返回默认值

## 面向对象高级编程

### 使用\__slots__

- 动态绑定方法

  - 给实例动态绑定属性
  - 给实例动态绑定方法
  - 给类动态绑定方法

  ```python
  # 给实例动态绑定属性
  stu1.age = 20

  from types import MethodType

  def set_age(self, age):
      self.age = age
      
  # 给实例动态绑定方法
  stu1.set_age = MethodType(set_age, stu1, Student)
  stu1.set_age(15)

  # 给类（所有实例）动态绑定方法
  Student.set_age = MethodType(set_age, None, Student)
  ```

### 使用\__slots__

限制class的属性，只允许添加name和age属性

```python
class Student(object):
	...     
	__slots__ = ('name', 'age') 
```

- `__slots__`定义的属性仅对当前类起作用，对子类无效

### 使用@property

```python
class Student(object):
	# 相当于getter方法
    @property
    def score(self):
        return self._score
	
    # 相当于setter方法
    @score.setter
    def score(self, value):
        if not isinstance(value, int):
            raise ValueError('score must be an integer!')
        if value < 0 or value > 100:
            raise ValueError('score must between 0 ~ 100!')
        self._score = value
```

调用的时候`s.score`就相当于调用了getter/setter

### 多继承

通过多继承，一个子类就可以同时获得多个父类的所有功能

`class Bat(Mammal, Flyable)`

- Mixin设计

  设计继承关系的时候，通常都是单一继承下来的，但是如果需要“混合”额外的功能，就需实现多继承

### 定制类

python的class中有许多特殊用途的函数，可以帮助我们定制类

- \__str__()

  相当于在类中重写java的toString()

  - \_\_repr\_\_() 是在交互模式下直接输入变量调用的toString()，可以让`__repr__ = __str__`

- __iter\_\_()

  实现了这个方法就可以被用于for...in循环，类似于list，该方法返回一个迭代对象，python的for循环会不断调用该迭代对象的next方法来循环拿到下一个值

  ```python
  class Fib(object):
      def __init__(self):
          self.a, self.b = 0, 1 # 初始化两个计数器a，b

      def __iter__(self):
          return self # 实例本身就是迭代对象，故返回自己

      def next(self):
          self.a, self.b = self.b, self.a + self.b # 计算下一个值
          if self.a > 100000: # 退出循环的条件
              raise StopIteration();
          return self.a # 返回下一个值
  ```

- __getitem\_\_

  为了可以像list[n]那样通过下标去元素

  ```
  class Fib(object):
      def __getitem__(self, n):
          a, b = 1, 1
          for x in range(n):
              a, b = b, a + b
          return a
  ```

**要真正模仿list的功能还要__getitem\_\_实现对切片的处理，step参数，负数处理等**

- __getattr\_\_

  调用某个属性时候如果没有，那么调用__getattr\_\_，在这个方法里，可以返回特定的属性值，也可以抛出异常

  - 这种完全动态的热醒可以针对完全动态的情况调用

    eg：REST API，调用API的URL类似，利用完全动态的__getattr\_\_，可以写出一个链式调用

    ```python
    class Chain(object):

        def __init__(self, path=''):
            self._path = path

        def __getattr__(self, path):
            return Chain('%s/%s' % (self._path, path))

        def __str__(self):
            return self._path
    ```

- __call\_\_

  ```python
  class Student(object):
      def __init__(self, name):
          self.name = name

      def __call__(self):
          print('My name is %s.' % self.name)
  ```

  调用方法如下：

  ```
  s = Student('huang')
  s() # 这里会执行__call__
  ```

  - __call\_\_还可以定义参数

### 使用元类

- 使用type()创建class对象

  ```python
  >>> def fn(self, name='world'): # 先定义函数
  ...     print('Hello, %s.' % name)
  ...
  >>> Hello = type('Hello', (object,), dict(hello=fn)) # 创建Hello class
  >>> h = Hello()
  >>> h.hello()
  Hello, world.
  >>> print(type(Hello))
  <type 'type'>
  >>> print(type(h))
  <class '__main__.Hello'>
  ```

  1. class的名称；
  2. 继承的父类集合，注意Python支持多重继承，如果只有一个父类，别忘了tuple的单元素写法；
  3. class的方法名称与函数绑定，这里我们把函数`fn`绑定到方法名`hello`上。

- metaclass（元类）

  除了使用`type()`动态创建类以外，要控制类的创建行为，还可以使用metaclass。

  **听说很难，，先跳了****