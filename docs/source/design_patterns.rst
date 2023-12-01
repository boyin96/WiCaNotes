.. _design_patterns:

======================
设计模式
======================

该文档介绍了 Python 编程中的一些常见的设计模式与编程思想。

.. contents:: :local:

.. _singleton:

1. 单例模式（Singleton Pattern）
----------------------------------
保证一个类只有一个实例，并提供一个全局访问点。

使用元类来设计单例模式。

1. 定义元类 ``SingletonMeta``:

.. code-block:: python

    class SingletonMeta(type):
      _instances = {}

      def __call__(cls, *args, **kwargs):
          if cls not in cls._instances:
              cls._instances[cls] = super().__call__(*args, **kwargs)
          return cls._instances[cls]

2. 使用 ``SingletonMeta`` 元类:

.. code-block:: python

    class MyClass(metaclass=SingletonMeta):
      def __init__(self, name):
          self.name = name

3. ``MyClass`` 只能有一个实例:

.. code-block:: python

    obj1 = MyClass("Instance 1")
    obj2 = MyClass("Instance 2")

    print(obj1.name)  # Output: Instance 1
    print(obj2.name)  # Output: Instance 1
    print(obj1 is obj2)  # Ture
