.. _magic_functions:

================
魔法方法与特殊变量
================

.. contents:: :local:


.. _introduction:

简介
------------

在 Python 中，**魔法方法（Magic Methods）** 和 **特殊变量（Special Variables）** 都是通过特定的命名规则来定义的。魔法方法是一种在类中定义的特殊方法，它们以双下划线开头和结尾。Python 对象可以说天生就具有这些魔法方法，我们也可以通过对类添加或者重载这些特殊方法来实现自己想要的行为。所谓的魔法方法就是通过在类中定义该方法从而为对象赋予特殊的效果。

特殊变量是一些具有特殊含义的变量，它们也以双下划线开头和结尾。


.. _magic_funcs:

常见魔法函数与使用
----------------------

构造方法
^^^^^^^^^^^^^^^^
``__init__(self, ...)``: 类的初始化方法。这个方法在对象被实例化时自动调用（对象被创建之后再调用，即对象已经存在），允许你执行一些初始化操作，为对象设置初始状态。该函数一般需要手动实现，主要用于对象属性的初始化。

``__new__(cls, ...)``: 对象实例化时第一个调用的方法（对象被创建之前再调用），它只取下 cls 参数，并把其他参数传给 __init__。该方法的返回值通常是类的新实例，它决定了实际创建的对象，可以使用该方法来构建单例模式（一个类只能有一个实例），但一般不常用。

``__del__(self)``: 对象的销毁器。销毁对象，在对象被删除时调用。方法可以包含一些在对象销毁时执行的清理操作，例如关闭文件、释放资源。由于依赖于垃圾回收机制，避免使用该方法来进行关键的清理操作，一般不常用。

类的表示
^^^^^^^^^^^^^^^^
``__str__(self)``: 返回对象的友好字符串表示形式，通过 str() 函数调用。将实例对象按照自己的定义的格式用字符串的形式表示出来。使用 str() 和 print() 会直接调用 __str__ 方法。

``__repr__(self)``: 返回对象的官方字符串表示形式，通过 repr() 函数调用。它更倾向于提供更详细和精确的信息，通常用于开发和调试阶段。使用 repr() 会直接调用 __repr__ 方法。

``__hash__(self)``: 用于定义对象的哈希值。哈希值是一个整数，用于在字典等数据结构中快速查找对象。当你将对象用作字典的键时，或者在集合中进行成员检查时，__hash__ 方法会被调用。在定义了 __hash__ 的情况下，对象的哈希值应该是固定的，即在对象的生命周期中保持不变。

比较操作
^^^^^^^^^^^^^^^^
``__eq__(self, other)``: 定义相等比较，通过 == 运算符调用。

``__ne__(self, other)``: 定义不相等比较，通过 != 运算符调用。

``__lt__(self, other)``: 定义小于比较，通过 < 运算符调用。

``__le__(self, other)``: 定义小于等于比较，通过 <= 运算符调用。

``__gt__(self, other)``: 定义大于比较，通过 > 运算符调用。

``__ge__(self, other)``: 定义大于等于比较，通过 >= 运算符调用。

算术运算
^^^^^^^^^^^^^^^^
``__add__(self, other)``: 定义加法操作，通过 + 运算符调用。

``__sub__(self, other)``: 定义减法操作，通过 - 运算符调用。

``__mul__(self, other)``: 定义乘法操作，通过 * 运算符调用。

``__truediv__(self, other)``: 定义真除法操作，通过 / 运算符调用。

.. tip::
   `Python operator <https://docs.python.org/zh-cn/3/library/operator.html>`_ 模块提供了全部比较操作以及数值操作。该模块主要用于：**函数化操作符**，将一些常见的操作符封装成函数，以便在函数式编程中使用。这样可以避免使用 lambda 函数或编写额外的函数来执行这些操作；**用于高阶函数**，在某些高阶函数（例如 map、filter、functools.reduce 等）中，通过使用 operator 模块的函数，可以提高代码的简洁性和可读性。

访问控制
^^^^^^^^^^^^^^^^
``__setattr__(self, name, value)``: 设置属性值时调用，例如 obj.attr = value。注意防止无限递归的问题。``setattr()`` 调用该方法。

``__getattr__(self, name)``: 获取不存在的属性时调用，例如 obj.nonexistent_attr。方法只有在尝试访问不存在的属性时才会被调用，如果存在该属性则不会调用该方法。``getattr()`` 调用该方法。

``__delattr__(self, name)``: 删除属性时调用，例如 del obj.attr。

``__getattribute__(self, name)``: 获取任意属性时都会调用，不推荐直接使用，以防止无限递归。

.. tip::
   为了防止无限递归调用上述方法，一般在该方法中调用基类的方法，比如 super().__getattribute__(name)，即通过调用基类的方法来避免。

自定义序列
^^^^^^^^^^^^^^^^
``__len__(self)``: 返回对象的长度，通过 len() 函数调用。

``__getitem__(self, key)``: 定义对象索引操作，通过 obj[key] 调用。这个方法允许对象像序列一样通过索引来获取元素。

``__setitem__(self, key, value)``: 定义赋值操作，通过 obj[key] = value 调用。这个方法允许对象像序列一样通过索引来设置元素的值。方法通常不要求返回值，但如果希望支持链式赋值，可以返回 self。

``__delitem__(self, key)``: 定义删除操作，通过 del obj[key] 调用。这个方法允许对象像序列一样通过索引来删除元素。

``__iter__()``: 方法是在一个对象被用于迭代时调用的。它应该返回一个迭代器对象。``iter()`` 调用该方法。

``__next__()``: 方法是迭代器对象的魔法方法，负责返回序列中的下一个元素。当序列中没有元素可迭代时，__next__() 应该抛出 StopIteration 异常。``next()`` 调用该方法。

.. code-block:: python

   class Reverse:
    """Iterator for looping over a sequence backwards."""
    def __init__(self, data):
        self.data = data
        self.index = len(data)

    def __iter__(self):
        return self

    def __next__(self):
        if self.index == 0:
            raise StopIteration
        self.index = self.index - 1
        return self.data[self.index]

可调用对象
^^^^^^^^^^^^^^^^
``__call__(self, ...)``: 允许类的一个实例像函数那样被调用。当一个对象被调用时，解释器会查找并调用该对象的 __call__ 方法。这个是我用到比较多的方法之一，非常方便实用。可以使用该方法来构建装饰器。

上下文管理
^^^^^^^^^^^^^^^^
``__enter__(self)``: 进入上下文时执行的操作，与 with 语句配合使用。__enter__ 方法应该返回一个对象，这个对象会被赋值给 as 子句中的变量，使得在 with 语句块内可以使用。

``__exit__(self, exc_type, exc_value, traceback)``: 退出上下文时执行的操作，与 with 语句配合使用。self: 表示对象本身，即离开上下文管理器的对象。
exc_type: 表示在 with 语句块内发生的异常的类型，如果没有异常则为 None。exc_value: 表示在 with 语句块内发生的异常的值，如果没有异常则为 None。traceback: 表示在 with 语句块内发生的异常的追踪对象，如果没有异常则为 None。


.. _magic_vars:

常见特殊变量与使用
----------------------

对象和类信息
^^^^^^^^^^^^^^^^
``__class__``: 对象所属的类。

``__name__``: 模块的名字，在主程序中为 "__main__"。

``__slots__``: 是用于定义类的固定属性集的机制。当你定义一个类时，如果你知道这个类将拥有固定的一组属性，并且你希望避免动态添加新属性，可以使用 __slots__。 用于限制类的属性集，防止动态添加新属性。

文档和注释
^^^^^^^^^^^^^^^^
``__doc__``: 对象的文档字符串。

``__annotations__``: 类型注解字典。

调试和诊断
^^^^^^^^^^^^^^^^
``__module__``: 定义对象的模块名。

``__dict__``: 包含对象命名空间的字典。一个包含对象属性的字典，其中键是属性名，值是相应的属性值。如果是类，则有自己的__dict__属性，用于存放类的静态函数、类函数、普通函数、全局变量以及一些内置的属性。

.. code-block:: python

   class Person:
       def __init__(self,_obj):
           self.name = _obj['name']
           self.age = _obj['age']
           self.energy = _obj['energy']
           self.gender = _obj['gender']
           self.email = _obj['email']
           self.phone = _obj['phone']
           self.country = _obj['country']

   # 可以简化为
   class Person:
       def __init__(self,_obj):
           self.__dict__.update(_obj)

.. note::

    根据 Python 官网关于`类 <https://docs.python.org/3/tutorial/classes.html>`_ 的说明：
    模块对象有一个秘密的只读属性 __dict__，它返回用于实现模块命名空间的字典；__dict__ 是属性但不是全局名称。 显然，使用这个将违反命名空间实现的抽象，应当仅被用于事后调试器之类的场合。

模块
^^^^^^^^^^^^^^^^
``__all__``: 是一个用于定义模块的公开接口的列表。当你使用 from module import * 语法时，__all__ 定义了哪些名称会被导入到当前命名空间中。这可以用于限制导入的符号，以避免导入过多的符号，或者用于指定哪些符号是模块的公共接口。

.. _reference:

参考
----------

- `Python 官方文档 - Data Model <https://docs.python.org/3/reference/datamodel.html>`_

- `Python Magic Methods by Example <https://rszalski.github.io/magicmethods/>`_
