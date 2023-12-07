.. _typing:

======================
typing 库讲解
======================

.. contents:: :local:

.. _introduction:

简介
-----------------------

``typing`` 是 Python 标准库中的一个模块，用于支持类型提示和类型检查。它提供了一组工具，用于在代码中添加类型注解，以提高代码的可读性和可维护性，并且可以被一些工具用于静态类型检查。要求 **Python 3.5** 及以上。

.. note::

   根据 `PEP 585 <https://peps.python.org/pep-0585/>`_ ，推荐使用 Python 内置的类型以及 ``collections`` 模块中的类型进行注解，往后 ``typing`` 中的类型可能被弃用。Python 3.9 以后版本支持泛化的内置类型，例如 ``list``，``tuple`` 等。

.. _example:

简单例子
---------------

.. code-block:: python

   from typing import Any

   name: str = "Borg"
   age: int = 26
   money: Any = None

   def add_numbers(a: int, b: int) -> int:
       return a + b

   def process_data(data: list[tuple[str, int]]) -> None:
       for item in data:
           print(f"Name: {item[0]}, Age: {item[1]}")


.. _typing_use:

注解简单分类与使用
----------------------------

1. 创建路径对象
^^^^^^^^^^^^^^^^^^^


2. 路径操作
^^^^^^^^^^^^^^^^^^^


3. 文件/目录检查
^^^^^^^^^^^^^^^^^^^


4. 文件/目录操作
^^^^^^^^^^^^^^^^^^^


5. 路径拼接
^^^^^^^^^^^^^^^^^^^


6. typing.Any
^^^^^^^^^^^^^^^^^^^

特殊类型，表示没有约束的类型。与所有类型兼容，**但不支持下标用法（[]）**。

7. typing.Optional
^^^^^^^^^^^^^^^^^^^

Optional[X] 等价于 X | None （或 Union[X, None] ） 。

8. typing.Union
^^^^^^^^^^^^^^^^^^^

联合类型； Union[X, Y] 等价于 X | Y ，意味着满足 X 或 Y 之一。


- Union[Union[int, str], float] == Union[int, str, float]

- Union[int] == int

- Union[int, str, int] == Union[int, str] == int | str

- Union[int, str] == Union[str, int]

9. typing.ClassVar
^^^^^^^^^^^^^^^^^^^

特殊类型注解构造，用于标注类变量。

.. _conclusion:

总结
------

总而言之，``typing`` 模块是 Python 中支持类型提示和类型检查的重要工具之一。通过在代码中添加类型注解，可以提高代码的可读性，帮助开发者更好地理解和维护代码。它支持复杂的类型表示，包括联合类型、可选类型和泛型。在类型检查工具的支持下，可以在开发阶段捕获一些潜在的类型错误，提高代码的质量和可靠性。个人建议尽可能对自己的代码进行注解，提高可读性以及便于检查。

.. _reference:

参考
---------

- `Python 官方文档 - typing <https://docs.python.org/3/library/typing.html>`_
