---
title: 分分钟学会 Python3
date: 2017-12-24 14:13:44
tags: ['Python', '前端']
---
最近入门 Python，然后看到一篇英文资料在 [learnxinyminutes](https://learnxinyminutes.com/docs/python3/) 上，对内容进行了翻译并稍加修改，加入了自己的理解和补充，可能翻得不好，待日后我对 Python 有更深入理解了再进行修改和补充。

有不妥的地方也欢迎提出修改意见，[项目地址](https://github.com/linxiaoru/python-tutorial/blob/master/examples/basic/learnpython3.py)
<!--more-->

```python
# 单行注释用井号（#）开头。

""" 多行注释用三个双引号（""）包裹，
    通常，三个双引号中要换行可以直接换，
    不需要加换行符切割，可以用作多行字符串，
    并且可以在三个双引号中添加注释。
"""

####################################################
# 1. 基本数据类型和运算符
####################################################

# 数字
3  # => 3

# 数学运算
1 + 1   # => 2
8 - 1   # => 7
10 * 2  # => 20
35 / 5  # => 7.0

""" 整除法用（//），两个整型相除，得到的还是整型。
    但是,如果一个浮点数除以一个非浮点数得到的还会是一个浮点数，
    并且，计算的结果却是忽略小数部分，运算的结果类似两个整型相除，但是得到一个浮点数。
"""
5 // 3       # => 1
5.0 // 3.0   # => 1.0
-5 // 3      # => -2 同样适用于负数
-5.0 // 3.0  # => 1

# 单斜杠（/）除法即为 float 除法，得到的商会返回为浮点数形式
10.0 / 3    # => 3.3333333333333335

# 模除（求余）
7 % 3   # => 1

# 指数运算
2**3    # => 8

# 使用小括号提升运算的优先级
(1 + 3) * 2  # => 8

# 布尔值也是基本数据类型（注意大写）
True
False

# 加 not 取反
not True    # => False
not False   # => Ture

# 布尔运算符
# 注意（and）和（or）是大小写敏感的
True and False  # => False
False or True   # => True

# 注意使用布尔运算符时要用整型
# False 是 0，True 是 1。这里和 JS 很不一样，JS 里 0 是 false，其他的数字都是 true
# 不要混用布尔类型（整型）和位运算符 and/or（&，|）
0 and 2                 # => 0
-5 or 0                 # => -5
0 == False              # => True
2 == True               # => False
1 == True               # => True
-5 != False != True     # => True

# 判断是否相等用 ==
1 == 1  # => True
2 == 1  # => False

# 判断不等用 !=
1 != 1  # => False
2 != 1  # => True

# 比较判断
1 < 10  # => True
1 > 10  # => False
2 <= 2  # => True
2 >= 2  # => True

# 比较判断可以链式判断
1 < 2 < 3   # => True
2 < 3 < 2   # => False

# is 和 ==
# is 检查两个变量是否关联的是同一个对象，但是 == 检查的是对象是否指向同样的值
a = [1, 2, 3, 4]    #  a 指向一个新的列表，[1, 2, 3, 4]
b = a               #  b 指向的是 a 所指向的
b is a              # => True，a 和 b 所关联的是同一个对象
b == a              # => True，a 指向的的对象和 b 指向的对象是相等的
b = [1, 2, 3, 4]    #  b 指向一个新的列表，[1, 2, 3, 4]
b is a              # => False，a 和 b 关联的不是同一个对象
b == a              # => True，a 指向的对象和 b 指向的对象的值是相等的。

# 字符串用双引号（"）或者单引号（'）来创建
"This is a string."
'This is alse a string.'

# 字符串也能够相加，但是最好不要这样做
"Hello" + "world!"  # => "Hello world!"
# 字符串表达式（非变量）能够不使用加号（+）直接拼接
"Hello" "world"     # => "Hello world"

# 字符串能够被当作是一个列表的元素
"This is a string"[0]   # => 'T'

# 可以使用 len() 函数来获取字符串的长度
len("This is a string")     # => 16

# 可以使用 .format 来格式化字符串
"{} can be {}".format("Strings", "interpolated")    # => "Strings can be interpolated"

# 可以重复使用格式化参数来减少重复的输入
"{0} be nimble, {0} be quick, {0} jump over the {1}".format("Jack", "candle stick")
# => "Jack be nimble, Jack be quick, Jack jump over the candle stick"

# 如果不想去数参数，可以使用关键字来代替。就是说上例中的 {0} 代表的是 .format() 的第一个参数，{1} 是第二个参数
"{name} wants to eat {food}".format(name="Bob", food="lasagna")     # => "Bob wants to eat lasagna"

# 如果你的 Python 3 代码也需要运行在 Python 2.5 及以下版本中，可以使用旧的格式化形式
"%s can be %s the %s way" % ("Strings", "interpolated", "old")      # => "Strings can be interpolated the old way"

# None 是一个对象
None    # => None

# 不要使用相等判断号（==）来比较对象和 None
# 使用 is 来代替。这个能够检查对象的值是否相等
"etc" is None   # => False
None is None   # => True

# None，0，空字符串，空列表，空字典，空元组都判定为 False
# 其他的值都是 True
bool(0)   # => False
bool("")  # => False
bool([])  # => False
bool({})  # => False
bool(())  # => False

####################################################
## 2. 变量和集合
####################################################

# Python 有一个用于打印的 print 函数
print("I'm Python. Nice to meet you!")      # => I'm Python. Nice to meet you!

# 默认情况下，print 函数会在输出结束时换行（就是在结尾多输出了一个换行符）
# 可以使用可选参数 end 来改变字符串的结尾，意思就是之前结束的时候是在末尾加了个换行符，现在可以使用 end 参数来改变这个换行符
print("Hello, World", end="!")      # => Hello, World!

# 从控制台获取输入数据的一个简单方式
input_string_var = input("Enter some data: ")   # 将数据作为字符串返回
# 注意：在早期版本的 Python 中，input() 方法被命名为 raw_input()

# 不用声明就可以给变量赋值
# 按照惯例变量使用小写加下划线拼接的形式书写
some_var = 5
some_var    # => 5

# 访问未赋值的变量会发生异常
# 参见下一节《控制流》来学习更多关于异常的处理
some_unknown_var    # 抛出命名错误异常

# if 可以用作表达式来使用
# 相当于 C 语言的三元运算符 "?:"
"yahoo!" if 3 > 2 else 2    # => "yahoo!", 类似于 3 > 2 ? "yahoo!" : 2

# 列表用于存储序列
li = []
# 也可以使用一个预先填充好的列表
other_li = [4, 5, 6]

# 可以在列表末尾添加元素
li.append(1)    # li 变为 [1] 
li.append(2)    # li 变为 [1, 2] 
li.append(4)    # li 变为 [1, 2, 4] 
li.append(3)    # li 变为 [1, 2, 4, 3]
# 使用 pop() 方法可以将列表最末位元素移除,并返回被移除的元素
li.pop()        # => 3, 此时 li 变为 [1, 2, 4]
# 将 3 放回列表
li.append(3)    #  li 又变回 [1, 2, 4, 3]

# 可以像其他语言访问数组一样访问一个列表
li[0]   # => 1
# 获取最末位元素
li[-1]  # => 3

# 越界查询会发生索引错误
li[4]   # => 抛出索引错误

# 可以使用切片语法来查询列表的范围
# 起始索引是包含起始位置的元素的，结束索引没有包含结束位置的元素
# 像数学中的开闭区间一样
# li[start:stop:step], start 是起始索引；stop 是结束索引；step 是要间隔的位数，1 就表示正向隔一个数取一，-2 就表示从列表最末位开始反向隔两位取一
li[1:3]     # => [2, 4]
# 省略结尾,就从起始索引位一直到列表末位
li[2:]      # => [4, 3]
# 省略开头，就从列表首位开始到结束索引前一位元素
li[:3]      # => [1, 2, 4]
# 每隔两位取一个
li[::2]     # => [1, 4]
# 将列表从末尾开始反向隔一位取一
li[::-1]    # => [3, 4, 2, 1]

# 使用切片可以进行深拷贝
li2 = li[:] # => li2 = [1, 2, 4, 3]，但是（li2 is li）返回 False，（li2 == li）返回的是 True

# 使用 del 可以移除列表的任意元素
del li[2]   # li 变为 [1, 2, 3]

# 移除第一个出现的与参数一样的值
li.remove(2)    # li 变为 [1, 3]
li.remove(2)    # 抛出 2 不在列表中的 ValueError 错误

# 在指定索引位置插入元素
li.insert(1, 2) # => li 又变回 [1, 2, 3]

# 获取第一个与指定参数匹配的元素的索引
li.index(2)     # => 1
li.index(4)     # 抛出 4 不在列表中的 ValueError 错误 

# 列表可以相加
# 注意：li 和 other_li 不会被修改
li + other_li   # => [1, 2, 3, 4, 5, 6]

# 使用 extend() 扩展合并列表
li.extend(other_li)     # li 现在是 [1, 2, 3, 4, 5, 6]

# 使用 in 来检查一个元素是否存在于一个列表中
1 in li     # => True

# 使用 len() 来检测列表的长度
len(li)     # => 6

# 元组和列表很相似，但是是不可变的
tup = (1, 2, 3)
tup[0]          # => 1
tup[0] = 3      # 抛出 TypeError（类型错误） 异常

# 注意：长度为 1 的元组应该在元组的最后一位元素后加上一个逗号，
# 但是长度不为 1 的元组不应该在末位元素后面加逗号，即使是长度为 0 的也不要加
type((1))   # => <class 'int'>
type((1,))  # => <class 'tuple'>
type(())    # => <class 'tuple'>

# 使用在列表上的大多数操作也都是可以使用在元组上的
len(tup)            # => 3
tup + (4, 5, 6)     # => (1, 2, 3, 4, 5, 6)
tup[:2]             # => (1, 2)
2 in tup            # => True

# 可以把元组（或列表）中的元素取出来赋值给多个变量
a, b, c = (1, 2, 3) # a 现在是 1，b 是 2，c 是 3
# 也可以进行序列解包，就是把首个或前几个元素与后面几个元素分别提取出来
a, *b, c = (1, 2, 3, 4) # a 现在是 1，b 现在是 [2, 3]，c 是 4
# 如果省略了括号，那么元组会被自动创建
d, e, f = 4, 5, 6
# 来看看交换两个值有多么容易
e, d = d, e     # d 现在是 5，e 是 4


# 字典用于存储键值对的映射关系
empty_dict = {}
# 预先填充的字典
filled_dict = {"one": 1, "two": 2, "three": 3}

# 注意：dict 的 key 必须是不可变对象。这是因为 dict 根据 key来计算 value 的存储位置，
# 如果每次计算相同的 key 得出的结果不同，那dict内部就完全混乱了。
# 这个通过 key 计算位置的算法称为哈希算法（Hash），使得 dict 可以进行高速查找。
# 要保证 hash 的正确性，作为 key 的对象就不能变。
# 在 Python 中，字符串、整数、浮点数、元组等都是不可变的，因此，可以放心地作为 key。而 list 是可变的，就不能作为 key。
invalid_dict = {[1, 2, 3]: "123"}       # 抛出一个类型错误：不可进行哈希计算的类型：'list'
valid_dict = {(1, 2, 3): [1, 2, 3]}     # 值可以是任何类型的

# 使用 [] 来查找值
filled_dict["one"]  # => 1

# 使用 keys() 方法将所有的 key 作为一个可遍历的对象来获取。我们需要用 list() 来包裹，将其转化为列表。
# 注意：字典的键的顺序是不能保证的，你得到的结果可能和这个示例不一致
list(filled_dict.keys())    # => ["three", "two", "one"] 

# 使用 values() 方法将所有的 value 作为一个可遍历的对象来获取。同样，我们将其用 list() 来包裹，将其转化为列表。
# 注意：关于他们的顺序问题和上面 key 顺序问题一样，都是随机的。
list(filled_dict.values())  # => [3, 2, 1]

# 检查字典中键的存在性问题使用 in
"one" in filled_dict        # => True
1 in filled_dict            # => False

# 查找一个不存在的 key 会产生键名错误
filled_dict["four"]         # keyError,键名错误

# 使用 get() 方法来避免键名错误
# filled_dict.get("one")    # => 1
# filled_dict.get("four")   # => None
# get() 方法支持传入一个默认参数，当 value 不存在时返回这个默认参数
filled_dict.get("one", 4)   # => 1
filled_dict.get("four", 4)  # => 4

# setdefault() 方法可以将一个键值对插入字典，只有当这个键在原先的字典中不存在时成立
filled_dict.setdefault("five", 5)   # filled_dict["five"] 被设置成 5 
filled_dict.setdefault("five", 6)   # filled_dict["five"] 仍旧是 5

# 添加到字典
filled_dict.update({"four":4})      # => {"one": 1, "two": 2, "three": 3, "five": 5, "four": 4}
filled_dict["four"] = 4             # 另一种添加到字典的方法

# 使用 del 来移除字典的键
del filled_dict["one"]      # 从填充好的字典中移除 "one" 键

# Python 3.5 还可以使用附加的解包选项
{'a': 1, **{'b': 2}}    # => {'a': 1, 'b': 2}
{'a': 1, **{'a': 2}}    # => {'a': 2}

# set 用于保存集合
empty_set = set()
# 使用一连串的值来初始化一个 set 集合。是的，看起来和一个字典很像。
# 然而，set 中的键是不能重复的
some_set = {1, 1, 2, 2, 3, 4}      # some_set 现在是 {1, 2, 3, 4}

# 和字典的键累死，集合的元素也是不可变对象
invalid_set = {[1], 1}      # => 抛出类型错误：不可进行哈希计算的类型：'list'
valid_set = {(1,), 1}

# 添加一个项到集合
filled_set = some_set
filled_set.add(5)       # filled_set 现在是 {1, 2, 3, 4, 5}

# 使用 & 计算交集
other_set = {3, 4, 5, 6}
filled_set & other_set  # => {3, 4, 5}

# 使用 | 计算并集
filled_set | other_set  # => {1, 2, 3, 4, 5, 6}

# 使用 - 计算补集
{1, 2, 3, 4} - {2, 3, 5}    # => {1, 4}

# 使用 ^ 计算对称差集
{1, 2, 3, 4} ^ {2, 3, 5}    # => {1, 4, 5}

# 检查左边的集合是否是右边集合的超集
{1, 2} >= {1, 2, 3}         # => False

# 检查左边的集合是否是右边集合的子集
{1, 2} <= {1, 2, 3}         # => True

# 使用 in 来检查一个元素是否存在于集合中
2 in filled_set             # => True
10 in filled_set            # => False



####################################################
## 3. 控制流和可迭代对象
####################################################

# 创建一个变量
some_var = 5

# 这有一个条件语句。缩进在 Python 中很重要！
# 惯例上使用四个空格来锁进，而不是 tab 缩进
# 这会打印出 "some_var is smaller than 10"
if some_var > 10:
    print("some_var is totally bigger than 10.")
elif some_var < 10:     # 这里 elif 分句是可选的
    print("some_var is smaller than 10.")
else:
    print("some_var is indeed 10.")

"""
for 循环打印列表：
打印:
    dog is a mammal
    cat is a mammal
    mouse is a mammal
"""
for animal in ["dog", "cat", "mouse"]:
    # 可以使用 format() 来格式化字符串
    print("{} is a mammal".format(animal))

"""
使用 "range(number)" 遍历一个数字序列，范围从 0 到给定的数
打印：
    0
    1
    2
    3
"""
for i in range(4):
    print(i)

"""
使用 "range(lower, upper)" 遍历一个数字序列，范围从第一个小的参数到第二个大的参数（包括第一个参数，不包括第二个参数）
打印：
    4
    5
    6
    7
"""
for i in range(4, 8):
    print(i)

"""
使用 "range(lower, upper, step)" 遍历一个数字序列，范围从第一个小的参数到第二个大的参数,
间隔第三个参数的位数，例如 step = 2，则意味间隔 2 个位数，当没有指定 step 的值时，默认值为 1。
打印：
    4
    6
"""
for i in range(4, 8, 2):
    print(i)

"""
while 循环会一直持续循环直到条件不再满足为止。
打印：
    0
    1
    2
    3
"""
x = 0
while x < 4:
    print(x)
    x += 1      # x = x + 1 的简写方式

# 使用 try/except 块来处理异常
try:
    # 使用 raise 来抛出一个错误
    raise IndexError("This is an index error")
except IndexError as e:
    pass                # pass 只是一个空操作。通常你应该在这里做一些恢复工作。
except (TypeError, NameError):
    pass                # 如果需要的话，多个异常可以一起处理
else:                   # try/except 块的可选分句，必须遵循所有的 except 块
    print("All good!")  # 只有当代码在 try 中没有抛出异常时才运行这里
finally:                # 在所有情况下都会执行
    print("we can clean up resources here")

# 可以使用 with 语句替换 try/finally 来进行资源清理
with open("myfile.txt") as f:
    for line in f:
        print(line)

# Python 提供了一个基础的抽象概念叫作迭代器
# 可迭代对象可被当作一个序列来对待
# range 函数返回的对象就是一个可迭代对象

filled_dict = {"one": 1, "two": 2, "three": 3}
our_iterable = filled_dict.keys()
print(our_iterable)     # => dict_keys(['one', 'two', 'three']). 这就是一个实现了我们的可迭代对象接口的对象

# 我们能够遍历它
for i in our_iterable:
    print(i)        # 打印 one, two, three

# 然而我们不能通过元素索引来处理元素
our_iterable[1]     # 抛出类型异常错误

# 迭代器是一个知道如何创建可迭代对象的对象
our_iterable = iter(our_iterable)

# 迭代器时一个能够记住我们遍历它时的状态的对象
# 迭代器是通过next()来实现的，每调用一次他就会返回下一个元素，
# 当没有下一个元素的时候返回一个StopIteration异常，所以实际上定义了这个方法的都算是迭代器。
next(our_iterable)      # => "one"

# 当我们进行迭代时它能够维持状态
next(our_iterable)      # => "two"
next(our_iterable)      # => "three"

# 当没有下一个元素的时候返回一个 StopIteration 异常
next(our_iterable)      # 抛出 StopIteration 异常

# 可以使用 list() 来获取迭代器中所有的元素
list(filled_dict.keys())    # => 返回 ["one", "two", "three"]


####################################################
## 4. 函数
####################################################

# 使用 def 创建新的函数
def add(x, y):
    print("x is {} and y is {}".format(x, y))
    return x + y        # 使用 return 语句来返回值

# 调函函数并传入参数
add(5, 6)       # => 打印“x is 5 and y is 6“然后返回 11

# 另一种传入关键字参数的调用函数的方式。关键字参数传入的是一个 dict
add(y=6, x=5)   # => 关键字参数可以以任意顺序传入

# 你也可以定义一个函数，然后传入一个可变数量的位置参数。位置参数是可变参数，args 接收的是一个 tuple。
def varargs(*args):
    return args

varargs(1, 2, 3)    # => (1, 2, 3)

# 同样，你也可以定义一个传入可变数量的关键字参数的函数
def keywords_args(**kwargs):
    return kwargs

# 我们来调用看看，看它会发生什么
keywords_args(big="foot", loch="ness")      # => {"big": "foot", "loch": "ness"}

# 你也可以同时传入这两种类型的参数，如果你想这么做的话
def all_the_args(*args, **kwargs):
    print(args)
    print(kwargs)
"""
all_the_args(1, 2, a=3, b=4) prints:
    (1, 2)
    {"a": 3, "b": 4}
"""

# 调用函数时，关键字参数和定位参数还可以反过来用
# 使用 * 来解包元组，** 来解包关键字参数
args = (1, 2, 3, 4)
kwargs = {"a": 3, "b": 4}
all_the_args(*args)             # => 相当于 foo(1, 2, 3, 4)
all_the_args(**kwargs)          # => 相当于 foo(a=3, b=4)
all_the_args(*args, **kwargs)   # => 相当于 foo(1, 2, 3, 4, a=3, b=4)

# 返回多个值（包含元组参数）
def swap(x, y):
    return y, x         # 将多个值当作一个元组来返回，不用加圆括号
                        # 注意：圆括号被去掉了，但是也可加上

x = 1
y = 2
x, y = swap(x, y)       # => x = 2, y = 1
# (x, y) = swap(x, y)   # 又一次去掉了圆括号，但是也是可以加上的

# 函数作用域
x = 5

def set_x(num):
    # 局部声明一个变量 x 和全局声明的变量 x 是不一样的
    x = num     # => 43
    print(x)    # => 43

def set_global_x(num):
    global x
    print(x)    # => 5
    x = num     # 全局声明的参数 x 现在设置为 6
    print(x)    # => 6

set_x(43)
set_global_x(6)


# 函数在 Python 中是一等公民
def create_adder(x):
    def adder(y):
        return x + y
    return adder

add_10 = create_adder(10)
add_10(3)       # => 13

# 还有匿名函数
(lambda x: x >2)(3)                     # => True
(lambda x, y: x ** 2 + y ** 2)(2, 1)    # => 5

# 还有一些内建的高阶函数
list(map(add_10, [1, 2, 3]))            # => [11, 12, 13]
list(map(max, [1, 2, 3], [4, 2, 1]))    # => [4, 2, 3]
                                        # 1 和 4 比较，返回4；2 和 2 比较，返回 2；3 和 1 比较，返回 3
list(filter(lambda x: x > 5, [3, 4, 5, 6, 7]))      # => [6, 7]

# 我们可以使用递推式构造列表来构造更好的 map 和 filter 
# 递推式构造列表将输出存储为一个列表，而列表本身又可以套叠列表
[add_10(i) for i in [1, 2, 3]]          # => [11, 12, 13]
[x for x in [3, 4, 5, 6, 7] if x > 5]   # => [6, 7]

# 同样可以将集合和字典进行递推式构造
{x for x in 'abcddeef' if x not in 'abc'}   # => {'d', 'e', 'f'}
{x: x**2 for x in range(5)}                 # => {1: 1, 2: 4, 3: 9, 4: 16}


####################################################
## 5. 模块
####################################################

# 你可以引入模块
import math
print(math.sqrt(16))    # => 4.0

# 你也可以从模块中获取指定的函数
from math import ceil, floor
print(ceil(3.7))        # => 4.0
print(ceil(3.7))        # => 3.0

# 你也可以引入模块中的所有函数
# 警告：不推荐这么做
from math import *

# 也能够缩写模块的名字
import math as m
math.sqrt(16) == m.sqrt(16)     # => True

# Python 模块实际上就是普通的 Python 文件。
# 你可以自己写你自己的模块，也可以引入它们。
# 模块的名称和文件同名

# 你可以使用 dir 查明哪些函数和属性是已经在模块中定义好的
import math
dir(math)

# 如果在你当前脚本所在的文件夹中有一个 Python 脚本命名为 math.py，
# 那么这个 math.py 文件将会替换内置的 Python 模块而被加载 
# 这是因为本地文件夹的优先级高于 Python 的内置库


####################################################
## 6. 类
####################################################

#我们可以使用 class 语句来创建一个类
class Human:
    # 类的属性可以被类的所有实例所共享
    species = "H. sapiens"

    # 基本的初始化（构造函数），在类被实例化时调用
    # 注意：双下划线开头并且双下划线结尾的对象或者属性是被 Python 自己使用的，但是是存在于用户命名空间的
    # 方法（或者对象或者属性）像这样形式的：__init__, __str__, __repr__ 等叫做特殊方法（或者魔法方法）
    # 不要使用这样的形式来命名你自己的变量
    def __init__(self, name):
        # 将参数赋值给实例的 name 属性
        self.name = name

        # 初始化属性
        self._age = 0

    # 一个实例方法。所有的方法使用 self 作为第一个参数
    def say(self, msg):
        print("{name}: {message}".format(name=self.name, message=msg))

    # 另一个实例方法。
    def sing(self):
        return 'yo... yo... microphone check... one two... one two...'

    # 类方法会被所有实例共享。
    # 类方法在调用时，会将类本身作为第一个函数传入。
    @classmethod
    def get_species(cls):
        return cls.species

    # 静态方法被调用时不传入类或者实例的引用
    @staticmethod
    def grunt():
        return "*grunt*"

    # 属性函数(property)类似 getter
    # 它能将 age() 方法变成一个只读属性
    # 在 Python 中，没必要写 getter 和 setter 
    @property
    def age(self):
        return self._age

    # 属性函数(property)可以被设置
    @age.setter
    def age(self, age):
        self._age = age
    
    # 属性函数(property)也可以被删除
    @age.deleter
    def age(self):
        del self._age

# 当 Python 监视器读取源文件时会执行所有的代码
# __name__ 检测会确保这个代码块只在主程序模块中执行
if __name__ == '__main__':
    # 实例化一个类
    i = Human(name="Ian")
    i.say("hi")             # => "Ian: hi"
    j = Human(name="Joel")
    j.say("hello")          # => "Joel: hello"
    # i 和 j 是 Human 的实例，或者换句话说，它们是 Human 对象

    # 调用类方法
    i.say(i.get_species())  # => "Ian: H. sapiens"
    # 修改共享属性
    Human.species = "H. neanderthalensis"
    i.say(i.get_species())  # => "Ian: H. neanderthalensis"
    j.say(j.get_species())  # => "Joel: H. neanderthalensis"

    # 调用静态方法
    print(Human.grunt())    # => "*grunt*"

    # 不能使用对象的实例来调用静态方法
    # 因为 i.grunt() 将会自动地将 self（对象 i） 作为第一个参数传入
    print(i.grunt())                # => TypeError: grunt() takes 0 positional arguments but 1 was given
    
    # 更新实例的属性
    i.age = 42
    # 获取属性
    i.say(i.age)        # => "Ian: 42"
    j.say(j.age)        # => "Joel: 0"
    # 删除属性
    del i._age
    # i.age             # => 这会抛出一个 AttributeError 

####################################################
## 6.1 继承
####################################################

# 继承允许新的子类在定义时从父类继承方法和变量

# 将我们之前已经定义好的 Human 类作为基类或者父类，我们就可以定义一个子类，Superhero，它将继承父类的变量 "species",
# "name", 和 "age",同样还会继承方法，像是 "sing" 和 "grunt"方法，但是它也能拥有自己的特有的属性。

# 利用文件模块化这一优势，你可以将上面提到的类放到文件里，如 say.py，human.py

# 将函数从其他文件中导入可以使用以下的导入格式
# from "不带扩展名的文件名" import "函数或类"
from human import Human

# 指定父类（们）作为定义一个类时的参数
class Superhero(Human):

    # 如果子类要继承父类的所有定义并且不进行更改，你可以使用 pass 关键字（并且没有别的了）
    # 但是在这个例子中，pass 被注释掉了，是为了能有一个独特的子类
    # pass

    # 子类能够重写父类的属性
    species = 'Superhuman'

    # 子类能够自动地继承父类的构造器包括它的参数，但是也能够额外地定义附加的参数或者重写它的方法，比如说类的构造函数
    # 这个构造函数从 Human 类继承了 name 参数并且加了 superpowers 和 movie 参数：
    def __init__(self, name, movie=False,
                 superpowers=["super strength", "bulletproofing"]):

        # 添加附加的类属性：
        self.fictional = True
        self.movie = movie
        self.superpowers = superpowers

        # super 函数使你能够获取被子类重写了的父类的方法，在这个例子中，就是 __init__ 方法。
        # 调用父类的构造函数：
        super().__init__(name)

    # 重写 sing 方法
    def sing(self):
        return 'Dun, dun, DUN!'

    # 再添加一个额外的类方法
    def boast(self)
        for power in self.superpowers:
            print("I wield the power of {pow}!".format(pow=power))

if __name__ == '__main__':
    sup = Superhero(name="Tick")

    # 实例类型检查
    if isinstance(sup, Human):
        print('I am human')
    if type(sup) is Superhero:
        print('I am a superhero')

    # 使用 getattr() 和 super() 来获取 MRO(从左至右的深度优先遍历)
    # 属性是动态的而且还能够更新
    print(Superhero.__mro__)        # => (<class '__main__.Superhero'>,
                                    # => <class 'human.Human'>, <class 'object'>)

    # 使用自己的类属性来调用父类的方法
    print(sup.get_species())        # => Superhuman

    # 调用重载方法
    print(sup.sing())               # => Dun, dun, DUN!

    # 调用来自 Human 的方法
    say.say('Spoon')                # => Tick: Spoon

    # 调用只存在在 Superhero 中的方法
    sup.boast()                     # => I wield the power of super strength!
                                    # => I wield the power of bulletproffing!

    # 继承类的属性
    sup.age = 31
    print(sup.age)                  # => 31

    # 只存在在 Superhero 中的属性
    print('Am I Oscar eligible?' + str(sup.movie))

####################################################
## 6.2 多重继承
####################################################

# 另一个类的定义
# bat.py
class Bat:

    species = 'Baty'

    def __init__(self, can_fly=True):
        self.fly = can_fly

    # 这个类也有一个 say 方法
    def say(self, msg):
        msg = '... ... ...'
        return msg

    # 同样，它也有自己的方法
    def sonar(self):
        return '))) ... ((('

if __name__ == '__main__':
    b = Bat()
    print(b.say('hello'))
    print(b.fly)


# 还有一个从 Superhero 和 Bat 继承来的类的定义
#  superhero.py
from superhero import Superhero
from bat import Bat

# 定义从 Superhero 和 Bat 继承来的子类的 Batman
class Batman(Superhero, Bat):

    def __init__(self, *args, **kwargs):
        # 通常需要调用 super 来继承属性
        # super(Batman, self).__init__(*args, **kwargs)
        # 然而我们这里是要处理多态，super 不是简单地调用所谓基类的方法，而是调用 MRO 中的下一个类的方法，也就是类似于 next 的方法。
        # 因此用另一种方法替代，我们为所有的祖先调用 __init__ 
        # 使用 *args 和 **kwargs 是一中整洁的方式来传递参数，每个父类就像剥洋葱一样一层一层进行
        Superhero.__init__(self, 'anonymous', movie=True,
                            superpowers=['Wealthy'], *args, **kwargs)
        Bat.__init__(self, *args, can_fly=False, **kwargs)
        # 重写 name 属性的值
        self.name = 'Sad Affleck'

    def sing(self):
        return 'nan nan nan nan nan batman!'

if __name__ == '__main__':
    sup = Batman()

    # 使用 getattr() 和 super() 来获取 MRO(从左至右的深度优先遍历)
    # 这个属性是动态的并且可被更新
    print(Batman.__mro__)       # => (<class '__main__.Batman'>, 
                                # => <class 'superhero.Superhero'>, 
                                # => <class 'human.Human'>, 
                                # => <class 'bat.Bat'>, <class 'object'>)
    
    # 用自己的类属性来调用父类的方法
    print(sup.get_species())    # => Superman

    # 调用重载了的方法
    print(sup.sing())           # => nan nan nan nan nan batman!

    # 调用方法来自 Human，因为继承的顺序很重要
    sup.say('I agree')         # => Sad Affleck: I agree

    # 调用只存在于第二个祖先的方法
    print(sup.sonar())         # => ))) ... (((
    
    # 继承类的属性
    sup.age = 100
    print(sup.age)             # => 100

    # 从二级祖先处继承来的属性默认值是被重写了的
    print('Can I fly? ' + str(sup.fly)) # => Can I fly? False


####################################################
## 7. 进阶
####################################################

# 生成器能够帮你简化代码
def double_numbers(iterable):
    for i in iterable:
        yield i + +

# 生成器能节省内存因为它们只在加载迭代对象迭代过程中下一个进程中的数据。
# 这反而能使它们执行操作更大范围的值
# 注意：在 Python 中，rang 替代了 xrange
for i in double_numbers(range(1, 900000000)):  # range 是一个生成器
    print(i)
    if i >= 30:
        break

# 正如你能够创建一个递推式构造列表，同样，你也能创建一个递推式构造生成器
values = (-x for x in [1,2,3,4,5])
for x in values:
    print(x)  # 控制台打印 -1 -2 -3 -4 -5 

# 你也能直接在列表中列出所有的递推式构造生成器
values = (-x for x in [1,2,3,4,5])
gen_to_list = list(values)
print(gen_to_list)  # => [-1, -2, -3, -4, -5]


# 装饰器
# 在这个例子中，beg 包裹着 say。 如果 say_please 是真值，则它将会改变返回的信息
from functools import wraps


def beg(target_function):
    @wraps(target_function)
    def wrapper(*args, **kwargs):
        msg, say_please = target_function(*args, **kwargs)
        if say_please:
            return "{} {}".format(msg, "Please! I am poor :(")
        return msg

    return wrapper


@beg
def say(say_please=False):
    msg = "Can you buy me a beer?"
    return msg, say_please


print(say())                 # Can you buy me a beer?
print(say(say_please=True))  # Can you buy me a beer? Please! I am poor :(



```