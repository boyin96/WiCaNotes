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

