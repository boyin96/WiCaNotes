.. _questions:

======================
Q & A
======================

该文档收集了在 Python 编程中的一些疑惑与解答。

Q. 构建抽象类的两种方式有何不同？
---------------------------

使用 `ABC` 作为基类
.. code-block:: python

    from abc import ABC, abstractmethod

    class MyAbstractClass(ABC):
        @abstractmethod
        def my_abstract_method(self):
            pass

        @abstractmethod
        def another_abstract_method(self):
            pass

使用 `ABCMeta`
.. code-block:: python

    from abc import ABCMeta, abstractmethod

    class MyAbstractClass(metaclass=ABCMeta):
        @abstractmethod
        def my_abstract_method(self):
            pass

        @abstractmethod
        def another_abstract_method(self):
            pass


A. 两种方式是一样的。根据 `Python 源码 <https://github.com/python/cpython/blob/main/Lib/abc.py>`_：
.. code-block:: python

    class ABC(metaclass=ABCMeta):
        """Helper class that provides a standard way to create an ABC using
        inheritance.
        """
        __slots__ = ()
