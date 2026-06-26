---
title: 语法基础
status: todo
icon: material/stairs-up
---

本文介绍 Python 的 [语法基础](https://docs.python.org/zh-cn/3.14/reference/index.html)，即不 `import` 任何包的情况下涉及的内容。

## 数据类型

Python 是动态类型语言，定义变量时不需要声明类型，赋值时会自动确定。但也支持在定义变量时写上预期的数据类型，不过解释器不会主动检查数据类型不匹配的问题，可以借助第三方 [类型检查](./index.md#类型检查) 工具来避免此类错误。

### 不可变数据类型

`bool`:

```python
is_active: bool = True
```

`int`:

```python
x: int = 10
```

`float`:

```python
y: float = 3.14
```

`str`:

```python
# 基本用法
info: str = "Alice"

# 字符转义
info = "hello\tworld"  # hello    world
# 取消字符转义，输出原始内容（r 即 raw）
info = r"hello\tworld"  # hello\tworld

# 字符模板 (f-string)
age = 18.88
info = (f"My age is {age:.1f}, "
        f"and you?")  # My age is 18.9, and you?
# 跨行字符串可以使用小括号包裹
# :.1f 表示给浮点数四舍五入保留 1 位小数

# 字符串拼接
# 使用 str 的 join 方法，将迭代器中的 str 拼接为一个完整版的字符串
# 相较于使用 += 省去了存储中间对象的内存开销，提升了程序的性能
num = [1314, 520, 601]
splice_str = "-".join(str(x) for x in num)  # 1314-520-601
```

`tuple`: [顺序表](../../../ds-and-algo/topic/ds.md#顺序表) 数据结构，数据定义好后无法修改。

```python
coordinates: tuple = (10, 20)
print(coordinates[0])  # 访问元组的第一个元素
```

### 可变数据类型

`dict`: [哈希表](../../../ds-and-algo/topic/ds.md#哈希表) 数据结构。

```python
# 字典创建（可哈希对象才能作为键，不可变数据类型都可以作为字典的键）
person: dict[str, str | int] = {"name": "Alice", "age": 25}

# 值修改
person["age"] = 26

# 创建新的键值对
person["city"] = "New York"

# 根据键访问值
print(person["name"])
print(person.get("name", "")) 更安全的访问方法，当字典不存在时，返回第二个参数，这里是 ""

# 查询是否存在某个键 O(1)
if "bob" in person:
    pass
# 等价于（基本不这么写）
if "bob" in person.keys():
    pass

# 查询是否存在某个值 O(n)
if "bob" in person.values():
    pass

# 查询是否存在某个键值对 O(1)
if "bob" in person.items():
    pass
```

`list`: 同样是 [顺序表](../../../ds-and-algo/topic/ds.md#顺序表) 数据结构，但是可以修改以及增删内容。

```python
# 初始化
fruits: list[str] = ["apple", "banana", "cherry"]

# 初始化（列表推导式）
squares = [x**2 for x in range(5)]  # 生成 0 到 4 的平方

# 尾插入 O(1)
fruits.append("orange")

# 尾删除 O(1)
fruits.pop()

# 按值删除 O(n)，删除第一个匹配到的元素
fruits.remove("banana")

# 按索引删除 O(n)
del fruits[1]

# 索引（超出最大索引值会报错 list index out of range）
print(fruits[0])

# 切片（超出最大索引不会报错）
print(fruits[1:3])
```

`set`: 同样是 [哈希表](../../../ds-and-algo/topic/ds.md#哈希表) 数据结构，可以理解为只保留 key 的 `dict`。

```python
colors: set[str] = {"red", "green", "blue"}
colors.add("yellow")  # 添加元素
colors.remove("green")  # 删除元素
```

## 运算符

Python 有以下 [运算符](https://docs.python.org/zh-cn/3.13/reference/lexical_analysis.html#operators)：

```bash
+       -       *       **      /       //      %      @
<<      >>      &       |       ^       ~       :=
<       >       <=      >=      ==      !=
```

Python 的 [运算符优先级](https://docs.python.org/zh-cn/3.13/reference/expressions.html#operator-precedence)（越往下等级越低）：

|                            运算符                            |                描述                |
| :----------------------------------------------------------: | :--------------------------------: |
|                      `(expressions...)`                      |       绑定或加圆括号的表达式       |
|                      `[expressions...]`                      |              列表显示              |
|                      `{key: value...}`                       |              字典显示              |
|                      `{expressions...}`                      |              集合显示              |
|                          `x[index]`                          |                抽取                |
|                       `x[index:index]`                       |                切片                |
|                      `x(arguments...)`                       |                调用                |
|                        `x.attribute`                         |              属性引用              |
|                          `await x`                           |            await 表达式            |
|                             `**`                             |                乘方                |
|                       `+x`, `-x`, `~x`                       |         正，负，按位非 NOT         |
|                   `*`, `@`, `/`, `//`, `%`                   |     乘，矩阵乘，除，整除，取余     |
|                           `+`, `-`                           |               加和减               |
|                          `<<`, `>>`                          |                移位                |
|                             `&`                              |             按位与 AND             |
|                             `^`                              |            按位异或 XOR            |
|                             `|`                              |             按位或 OR              |
| `in`, `not in`, `is`, `is not`, `<`, `<=`, `>`, `>=`, `!=`, `==` | 比较运算，包括成员检测和标识号检测 |
|                           `not x`                            |           布尔逻辑非 NOT           |
|                            `and`                             |           布尔逻辑与 AND           |
|                             `or`                             |           布尔逻辑或 OR            |
|                         `if -- else`                         |             条件表达式             |
|                           `lambda`                           |           lambda 表达式            |
|                             `:=`                             |             赋值表达式             |

## 流程控制

条件语句 `if`、`elif`、`else` 用于分支控制。

```python
age = 20

if age >= 18:
    print("成人")
else:
    print("未成年")
```

循环语句 `for` 和 `while` 用于重复控制。

```python
# for 循环
for i in range(5):  # 输出 0 到 4
    print(i)

# while 循环
count = 0
while count < 3:
    print("count:", count)
    count += 1  # 增加 count 的值
```

## 函数

Python 使用 `def` 关键字定义函数。

```python
def greet(name):
    return "Hello, " + name

message = greet("Alice")
print(message)  # Hello, Alice
```

### 解包机制

Python 中的解包机制让参数传递变得更灵活。解包分成两类：

- 函数定义处，收集参数。`*args` 用来接收多余的「位置参数」，而 `**kwargs` 用来接收多余的「关键字参数」；
- 函数调用处，展开参数。`*` 用来展开序列（可迭代对象），`**` 用来展开字典。

解包机制的本质，是让 Python 的参数系统脱离固定形态，转而以更抽象的方式处理数据结构，使得语言表达能力在工程层面保持灵活却不混乱。

位置参数的收集与展开：

- 函数定义处，用于接收任意数量的「位置参数」并将它们收集为一个元组：

    ```python
    def add_all(x, *args):
        print(type(args))  # <class 'tuple'>
        total = 0
        for value in args:
            total += value
        return total
    
    print(add_all(1, 2, 3))  # 5
    print(add_all(1, 2, 3, 4))  # 9
    ```

- 函数调用处，把一个序列展开为多个位置参数：

    ```python
    def add_all(*args):
        total = 0
        for value in args:
            total += value
        return total
    
    nums = [1, 2, 3]  # 或 set、tuple 等可迭代对象
    print(add_all(*nums))  # 等价于 add_all(1, 2, 3)
    ```

关键字参数的收集与展开：

- 函数定义处，用于接收任意数量的「关键字参数」并将它们组织为一个字典：

    ```python
    def describe(**kwargs):
        print(type(kwargs))  # <class 'dict'>
        print(kwargs)  # {'name': 'Alice', 'age': 18}
    
    describe(name="Alice", age=18)
    ```

- 函数调用处，把一个字典展开为多个关键字参数：

    ```python
    def f(**kwargs):
        print(kwargs)  # {'name': 'Bob', 'city': 'Singapore'}
    
    info = {"name": "Bob", "city": "Singapore"}
    f(**info)  # 等价于 f(name="Bob", city="Singapore")
    ```

    > [!note]
    >
    > 字典解包是一种清晰、稳定、可扩展的方式，在 [Pydantic](./network-lib.md#pydantic)、[FastAPI](./network-lib.md#fastapi) 等框架中极为常见。例如，在构造 Pydantic 模型时，通常会将 JSON 反序列化得到的字典直接展开，模型内部再对字段进行验证：
    >
    > ```python
    > class User(BaseModel):
    >     name: str
    >     age: int
    > 
    > data = {"name": "Tom", "age": 20}
    > user = User(**data)
    > ```

### `lambda`

`lambda` 函数是一种简洁的匿名函数，适合编写非常简单、一次性的函数逻辑：

```python
square = lambda x: x**2
print(square(5))  # 25
```

不过，根据 [PEP 8](https://peps.python.org/pep-0008/#programming-recommendations) 的建议，在真实项目中并不推荐将 `lambda` 赋值给变量，应当使用 `def` 定义函数：

```diff
- square = lambda x: x**2
+ def square(x):
+     return x**2
```

原因包括：

- 使用 `def` 会让函数名出现在 traceback 中，更有利于调试。
- `lambda` 的核心价值在于内联使用，一旦赋值给变量就失去了匿名函数的意义。

因此，`lambda` 更适合作为参数直接传递给高阶函数：

```python
# 示例一
nums = [1, 2, 3]
result = map(lambda x: x * 2, nums)

# 示例二
students.sort(key=lambda s: s.score)
```

## 类

Python 是面向对象的语言，使用 `class` 关键字定义类。

```python
class Dog:
    def __init__(self, name, age):
        self.name = name
        self.age = age
    
    # 强私有方法（前置双下划线）
    def __calculate_dog_years(self):
        """将狗的年龄转换为人类年龄"""
        return self.age * 7
    
    # 公有方法
    def get_human_age(self):
        # 在类内部可以正常调用
        human_age = self.__calculate_dog_years()
        return f"{self.name} is {human_age} years old in human years."


dog = Dog("Buddy", 3)

# 调用公有方法
print(dog.get_human_age())  # Buddy is 21 years old in human years

# 调用私有方法（报错）
print(dog.__calculate_dog_years())  # AttributeError

# 强制调用私有方法（不推荐）
print(dog._Dog__calculate_dog_years())  # 21
```

## 模块

模块，即一个以 `.py` 为后缀的代码文件，其中可以包括类、函数、变量等任意 Python Object。

### `__all__`

模块内的 `__all__: list[str]` 变量用来约束当前模块暴露出去的对象。有助于明确一个模块中哪些是可以公开调用的，哪些仅仅是内部使用的。

=== "使用 `__all__`"

    ```python title="示例模块 cls_all.py" hl_lines="3"
    import math
    
    __all__ = ["Demo", "GLOBAL_VAR"]
    
    GLOBAL_VAR = "hello"
    
    class Demo:
        def __init__(self):
            print(f"cos(1) = {math.cos(1)}")
    
    def fun():
        print("this is function")
    ```
    
    ```python title="示例调用 main.py"
    from cls_all import *
    
    print(globals())
    ```
    
    ```text title="程序输出（调整格式后）" hl_lines="11-12"
    {
        '__name__': '__main__',
        '__doc__': None,
        '__package__': None,
        '__loader__': <_frozen_importlib_external.SourceFileLoader object at 0x000001EA08FAC380>,
        '__spec__': None,
        '__annotations__': {},
        '__builtins__': <module 'builtins' (built-in)>,
        '__file__': 'e:\\python\\demos\\fastapi-demo\\src\\cls_all_main.py',
        '__cached__': None,
        'Demo': <class 'cls_all.Demo'>,
        'GLOBAL_VAR': 'hello'
    }
    ```

=== "不用 `__all__`"

    ```python title="示例模块 cls_all.py" hl_lines="3"
    import math
    
    # __all__ = ["Demo", "GLOBAL_VAR"]
    
    GLOBAL_VAR = "hello"
    
    class Demo:
        def __init__(self):
            print(f"cos(1) = {math.cos(1)}")
    
    def fun():
        print("this is function")
    ```
    
    ```python title="示例调用 main.py"
    from cls_all import *
    
    print(globals())
    ```
    
    ```text title="程序输出（调整格式后）" hl_lines="11-14"
    {
        '__name__': '__main__',
        '__doc__': None,
        '__package__': None,
        '__loader__': <_frozen_importlib_external.SourceFileLoader object at 0x0000028A84E6C380>,
        '__spec__': None,
        '__annotations__': {},
        '__builtins__': <module 'builtins' (built-in)>,
        '__file__': 'e:\\python\\demos\\fastapi-demo\\src\\cls_all_main.py',
        '__cached__': None,
        'math': <module 'math' (built-in)>,
        'GLOBAL_VAR': 'hello',
        'Demo': <class 'cls_all.Demo'>,
        'fun': <function fun at 0x0000028A84EB0900>
    }
    ```

## 异常

现实场景下，程序的输入或运行几乎不可能始终正确，为了避免程序在出现异常时直接宕机，开发人员需要主动编写代码，来应对可能的异常，以确保系统的鲁棒性。

基本异常处理逻辑主要分两步：

1. 抛出异常：Python 使用 `raise` 关键字来抛出异常。
2. 捕获和处理异常：Python 使用 `try`、`except` 和 `finally` 关键字捕获和处理异常。

在发生 `raise ErrorType()` 后，Python 会做三件事：

1. 构造异常对象；
2. 停止当前函数的执行；
3. 基于函数调用栈「逐层向上」查找异常处理器 `try`。

### 抛出异常

基本语法：

```python
raise ExceptionObject()
```

常见异常：

```python
# 模块没找到
raise ModuleNotFoundError("module not found")

# 文件没找到
raise FileNotFoundError("file not found")

# 除零
raise ZeroDivisionError("division by zero")
```

### 捕获和处理异常

> [!note] 原则
>
> 能在当前层级处理的异常就立刻处理，无法处理的异常就抛出让上层处理。

实际编码过程中，往往需要配合标准库中的 [logging](./std-lib.md#logging) 模块，达到最佳实践：

```python
import logging

logger = logging.getLogger(__name__)

try:
    # 正常业务逻辑
    ...
except (
    TimeoutError,
    ConnectionError,
) as e:
    # 可预见的、可解决的异常，直接重试
    retry(e)
except ModuleNotFoundError as e:
    # 可预见的、无法解决的异常，向上抛出
    return error_response(e)
except Exception:
    # 不可预见的异常，先记录，再抛出
    logger.exception("unexpected error")  # 记录异常
    raise  # 向上抛出
finally:
    # 无论上述结果如何，都会执行这段逻辑，例如释放资源等
    ...
```

## 断言

在代码编写的过程中，为了快速验证逻辑，我们一般会使用断言语句，即 assert。其基本用法是：

```python
assert statement, assertion
```

这里 statement 即为需要验证的逻辑，当 statement 为 True 时程序才会继续执行下面的语句；assertion 即为 statement 为 False 时抛出的 AssertionError 的内容。本质上相当于：

```python
if not x:
    raise AssertionError("something error")
```

不过在生产环境下建议使用：

```python
if not x:
    raise ValueError("something error")
```

因为 Python 在 `-O` 模式下会移除所有 `assert`。

## 迭代器与生成器

迭代器是可以遍历的对象，生成器则是使用 `yield` 来定义的惰性迭代器。

```python
# 迭代器
numbers = iter([1, 2, 3])
print(next(numbers))  # 1
print(next(numbers))  # 2

# 生成器
def count_up_to(limit):
    count = 1
    while count <= limit:
        yield count
        count += 1

gen = count_up_to(3)
print(next(gen))  # 1
print(next(gen))  # 2
```
