# python-learn

Python提供了完善的基础代码库，覆盖网络、文件、GUI、数据库、文本等。

## Python 运行
```
// 指令运行
python test.py

// 直接运行
// 在 .py 文本第一行加上 `#!/usr/bin/env python`
// 然后通过指令 `chmod a+x test.py`
./test.py

```

## 输入输出
```
// 输出
// 多个字符串拼接，用逗号“,”隔开
print "hello, ", "world"
// 输入
// raw_input，让用户输入字符串，并存放到一个变量里。
name = raw_input('please enter you name: ')
// print "hello", name
```

## 函数
```
// 定义一个函数要使用def语句
def my_abs():
	if x >= 0:
		return x
	else:
		return -x

// 空函数
// 定义一个什么事也不做的空函数，可以用pass语句
def nop():
	pass
```

### 函数参数
```
// --- 默认参数 --- //
//---------------//

def power(x, n = 2):
	s = 1
	while n > 0:
		n = n - 1
		s = s * x
	return s

// 定义默认参数要牢记一点：默认参数必须指向不变对象
def add_end(L=None):
    if L is None:
        L = []
    L.append('END')
    return L

// --- 可变参数 --- //
//---------------//

// 由于参数个数不确定，作为一个list或tuple传进来 
def calc(numbers):
    sum = 0
    for n in numbers:
        sum = sum + n * n
    return sum
// 调用
calc([1, 2, 3])
calc((1, 3, 5, 7))

// 定义可变参数和定义list或tuple参数相比，仅仅在参数前面加了一个*号 
def calc(*numbers):
    sum = 0
    for n in numbers:
        sum = sum + n * n
    return sum
// 调用    
calc(1, 2, 3)
nums = [1, 2, 3]
calc(*nums) 

// --- 关键字参数 --- //
//-----------------//

// 可变参数允许你传入0个或任意个参数，这些可变参数在函数调用时自动组装为一个tuple。
// 而关键字参数允许你传入0个或任意个含参数名的参数，这些关键字参数在函数内部自动组装为一个dict。
def person(name, age, **kw):
	print 'name:', name, 'age:', age, 'other:', kw

person('Adam', 45, gender='M', job='Engineer')

// name: Adam age: 45 other: {'gender': 'M', 'job': 'Engineer'}

// 可以先组装出一个dict，然后，把该dict转换为关键字参数传进去：
kw = {'city': 'Beijing', 'job': 'Engineer'}
person('Jack', 24, **kw)
// name: Jack age: 24 other: {'city': 'Beijing', 'job': 'Engineer'}
```

## 尾递归
// 在函数返回的时候，调用自身本身，并且，return语句不能包含表达式。
// 这样，编译器或者解释器就可以把尾递归做优化，使递归本身无论调用多少次，都只占用一个栈帧，不会出现栈溢出的情况。
```
def fact(n):
    return fact_iter(n, 1)

def fact_iter(num, product):
    if num == 1:
        return product
    return fact_iter(num - 1, num * product)
// Python标准的解释器没有针对尾递归做优化，任何递归函数都存在栈溢出的问题。    
```

## 高级特征
### 切片
取一个list或tuple的部分元素是非常常见的操作。比如，一个list如下：
```
L = ['Michael', 'Sarah', 'Tracy', 'Bob', 'Jack']
// 取前面三个
L[0:3]
L[:3]

L = range(100)
// [0, 1, 2, 3, ..., 99]
// 前10个数
L[:10]
// output => [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
// 后十个
L[:-10]
// output => [90, 91, 92, 93, 94, 95, 96, 97, 98, 99]
// 前10个数，每两个取一个
L[:10:2]
// output => [0, 2, 4, 6, 8]

// tuple也是一种list，唯一区别是tuple不可变。
// 因此，tuple也可以用切片操作，只是操作的结果仍是tuple
(0, 1, 2, 3, 4, 5)[:3]
// output => (0, 1, 2)
```
### 迭代
通过for ... in来完成的
```
d = {'a': 1, 'b': 2, 'c': 3}
for key in d:
	print key

// 默认情况下，dict迭代的是key。如果要迭代value，可以用for value in d.itervalues()
// 如果要同时迭代key和value，可以用for k, v in d.iteritems()。

// 字符串也是可迭代对象	
```

判断一个对象是可迭代对象呢？方法是通过collections模块的Iterable类型判断：
```
from collections import Iterable
isinstance('abc', Iterable) # str是否可迭代
// True
isinstance([1,2,3], Iterable) # list是否可迭代
// True
isinstance(123, Iterable) # 整数是否可迭代
False
```

对list实现类似Java那样的下标循环怎么办？Python内置的enumerate函数可以把一个list变成索引-元素对
```
for i, value in enumerate(['A', 'B', 'C']):
	print i, value
```

### 列表生成式
```
[x * x for x in range(1, 11)]
// [1, 4, 9, 16, 25, 36, 49, 64, 81, 100]

[x * x for x in range(1, 11) if x % 2 == 0]
// [4, 16, 36, 64, 100]

// 两层循环
[m + n for m in 'ABC' for n in 'XYZ']
// ['AX', 'AY', 'AZ', 'BX', 'BY', 'BZ', 'CX', 'CY', 'CZ']

// 列出当前目录下的所有文件和目录名，可以通过一行代码实现
// import os # 导入os模块，模块的概念后面讲到
[d for d in os.listdir('.')] 
// # os.listdir可以列出文件和目录
// ['.emacs.d', '.ssh', '.Trash', 'Adlm', 'Applications', 'Desktop', 'Documents', 'Downloads', 'Library', 'Movies', 'Music', 'Pictures', 'Public', 'VirtualBox VMs', 'Workspace', 'XCode']
```

### 生成器
在循环的过程中不断推算出后续的元素呢？这样就不必创建完整的list，从而节省大量的空间。在Python中，这种一边循环一边计算的机制，称为生成器（Generator）。

要创建一个generator，有很多种方法。第一种方法很简单，只要把一个列表生成式的[]改成()，就创建了一个generator：
```
L = [x * x for x in range(10)]
// [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
g = (x * x for x in range(10))
// <generator object <genexpr> at 0x104feab40>
// 创建L和g的区别仅在于最外层的[]和()
// L是一个list，而g是一个generator。

// yield
// 把fib函数变成generator，只需要把print b改为yield b就可以了
def fib(max):
    n, a, b = 0, 0, 1
    while n < max:
        yield b
        a, b = b, a + b
        n = n + 1
// 最难理解的就是generator和函数的执行流程不一样。函数是顺序执行，遇到return语句或者最后一行函数语句就返回。而变成generator的函数，在每次调用next()的时候执行，遇到yield语句返回，再次执行时从上次返回的yield语句处继续执行。        
```
