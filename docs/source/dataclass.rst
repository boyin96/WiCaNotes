.. _dataclass:

======================
dataclasses 库讲解
======================

.. contents:: :local:

.. _introduction:

简介
-----------------------

``dataclasses`` 是 Python 3.7 以上标准库中的一个模块，旨在简化创建和管理数据类（data class）的过程。数据类是一种用于存储数据的特殊类，它通过自动生成一些常见的特殊方法，简化了类的定义，使得代码更加清晰和简洁。


与其他数据类型比较
------------------

1. **与字典（dict）的比较：**

   - 字典是一种无序的键值对集合，通常用于表示动态的、灵活的数据结构。然而，字典不提供类型检查和结构化数据的便利性。
   - 数据类提供了类型注解和自动生成的特殊方法，使得数据的结构更为明确，同时不失灵活性。

2. **与元组（tuple）和命名元组（namedtuple）的比较：**

   - 元组是不可变的序列，而数据类是可变的。
   - 命名元组提供了字段名称，但是不支持添加、删除、修改字段。
   - 数据类在功能上类似于命名元组，但提供了更多的灵活性和功能。

3. **与自定义类的比较：**

   - 自定义类需要手动编写大量的特殊方法，而数据类通过 ``@dataclass`` 装饰器自动生成这些方法。
   - 数据类提供了更简洁的语法，使得创建简单的数据容器更为便利。

.. _example:

简单例子
---------------------

.. code-block:: python

    from dataclasses import dataclass

    @dataclass
    class InventoryItem:

        name: str
        unit_price: float
        quantity_on_hand: int = 0

        def total_cost(self) -> float:
            return self.unit_price * self.quantity_on_hand

    # 等价于

    def __init__(self, name: str, unit_price: float, quantity_on_hand: int = 0):
        self.name = name
        self.unit_price = unit_price
        self.quantity_on_hand = quantity_on_hand

.. _dataclass_use:

常见方法与属性
----------------------------

1. dataclasses.dataclass
^^^^^^^^^^^^^^^^^^^

``dataclass(*, init=True, repr=True, eq=True, order=False, unsafe_hash=False, frozen=False, match_args=True, kw_only=False, slots=False, weakref_slot=False)``

以下等价：

.. code-block:: python

    @dataclass
    class C:
        ...

    @dataclass()
    class C:
        ...

    @dataclass(init=True, repr=True, eq=True, order=False, unsafe_hash=False, frozen=False,
               match_args=True, kw_only=False, slots=False, weakref_slot=False)
    class C:

2. dataclasses.field
^^^^^^^^^^^^^^^^^^^

``dataclasses.field(*, default=MISSING, default_factory=MISSING, init=True, repr=True, hash=None, compare=True, metadata=None, kw_only=MISSING)``

.. code-block:: python

    @dataclass
    class C:
        mylist: list[int] = field(default_factory=list)

    c = C()
    c.mylist += [1, 2, 3]


.. _conclusion:

总结
------

- ``dataclass`` 提供了一种简单而强大的方式来定义数据类，减少了样板代码，提高了代码的可读性和可维护性。

- 它适用于需要存储数据、进行比较和输出字符串表示的场景，尤其在数据处理、配置等方面有着广泛的应用。


.. _reference:

参考
---------

- `Python 官方文档 - dataclass <https://docs.python.org/3/library/dataclasses.html>`_
