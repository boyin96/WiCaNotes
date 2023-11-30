.. _questions:

======================
Q & A
======================

该文档收集了在 Python 编程中的一些疑惑与解答。

.. contents:: :local:


.. _abstract_con:

1. 构建抽象类的两种方式有何不同？
------------------------------

使用 ``ABC`` 作为基类

.. code-block:: python

    from abc import ABC, abstractmethod

    class MyAbstractClass(ABC):
        @abstractmethod
        def my_abstract_method(self):
            pass

        @abstractmethod
        def another_abstract_method(self):
            pass

使用 ``ABCMeta``

.. code-block:: python

    from abc import ABCMeta, abstractmethod

    class MyAbstractClass(metaclass=ABCMeta):
        @abstractmethod
        def my_abstract_method(self):
            pass

        @abstractmethod
        def another_abstract_method(self):
            pass

两种方式是一样的。根据 `Python 源码 <https://github.com/python/cpython/blob/main/Lib/abc.py>`_：

.. code-block:: python

    class ABC(metaclass=ABCMeta):
        """Helper class that provides a standard way to create an ABC using
        inheritance.
        """
        __slots__ = ()


.. _copy:

2. Python 深浅拷贝有何不同？
-------------------------

在 Python 中，深拷贝（deep copy）和浅拷贝（shallow copy）是两种拷贝数据的方式，它们在处理嵌套对象（如列表中包含列表）时有不同的行为。

浅拷贝（Shallow Copy）
^^^^^^^^^^^^^^^^^^^^^^^

浅拷贝创建了一个新的对象，但是它只复制了原对象的最外层元素，而不会递归地复制内部的嵌套对象。

使用 ``copy`` 模块的 ``copy`` 函数来进行浅拷贝：

.. code-block:: python

   import copy

   original_list = [1, [2, 3], [4, 5]]
   shallow_copied_list = copy.copy(original_list)

   # 修改嵌套对象
   shallow_copied_list[1][0] = 999

   print(original_list)  # [1, [999, 3], [4, 5]]
   print(shallow_copied_list)  # [1, [999, 3], [4, 5]]

深拷贝（Deep Copy）
^^^^^^^^^^^^^^^^^^^^^^

深拷贝创建了一个新的对象，并且递归地复制了原对象及其所有嵌套对象。

使用 ``copy`` 模块的 ``deepcopy`` 函数来进行深拷贝：

.. code-block:: python

   import copy

   original_list = [1, [2, 3], [4, 5]]
   deep_copied_list = copy.deepcopy(original_list)

   # 修改嵌套对象
   deep_copied_list[1][0] = 999

   print(original_list)  # [1, [2, 3], [4, 5]]
   print(deep_copied_list)  # [1, [999, 3], [4, 5]]


3. Python 中的闭包有何作用？
----------------------------

闭包概念
^^^^^^^^^^^^^^^^^^^^^^^^^

闭包是一种在函数嵌套的情况下，内部函数引用了外部函数中变量的函数。这样的函数被称为闭包，因为它“封装”了外部函数的作用域。

在 Python 中，闭包通常是指包含对外部作用域中变量引用的函数。闭包可以访问包含它的外部函数的变量，即使这些变量在外部函数执行完毕后仍然存在。

.. code-block:: python

   def outer_function(x):
       def inner_function(y):
           return x + y
       return inner_function

在这个例子中，``inner_function`` 是一个闭包，因为它引用了外部函数 ``outer_function`` 的变量 ``x``。

闭包使用场景
^^^^^^^^^^^^^^^^^^^^^^^^^

**1. 保持状态**

闭包可以用于保持某个状态。例如，一个计数器函数可以通过闭包保持计数状态：

.. code-block:: python

   def counter():
       count = 0

       def increment():
           nonlocal count
           count += 1
           return count

       return increment

   counter_func = counter()
   print(counter_func())  # 输出: 1
   print(counter_func())  # 输出: 2

**2. 实现装饰器**

闭包在实现装饰器时非常有用。装饰器本质上是一个闭包，它可以包装其他函数，并在执行前后进行额外的操作。

.. code-block:: python

   def my_decorator(func):
       def wrapper():
           print("Something is happening before the function is called.")
           func()
           print("Something is happening after the function is called.")
       return wrapper

   @my_decorator
   def say_hello():
       print("Hello!")

   say_hello()

闭包注意事项
^^^^^^^^^^^^^^^^^^^^^^^^^

**1. 修改外部变量**

在闭包中修改外部函数中的变量时，需要使用 ``nonlocal`` 关键字，否则 Python 会将其视为局部变量。

.. code-block:: python

   def outer_function():
       x = 10

       def inner_function():
           nonlocal x
           x = 20

       inner_function()
       print("After inner_function:", x)

   outer_function()

**2. 循环中的闭包**

在循环中创建闭包时，要注意循环变量的作用域问题，可以通过默认参数或使用函数工厂解决。

.. code-block:: python

   def create_closure(value):
       def closure():
           print(value)
       return closure

   closures = [create_closure(i) for i in range(5)]

   for closure in closures:
       closure()

