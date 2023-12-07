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

   - ``Path()``: 创建一个路径对象。
   - ``Path().cwd()``: 获取当前路径。
   - ``Path().home()``: 获取用户 home 路径。


2. 路径操作
^^^^^^^^^^^^^^^^^^^

我们以路径 *C:\Users\boyin\Downloads\demo.txt* 为例。

   - ``resolve()``: 返回规范化的绝对路径，*C:\Users\boyin\Downloads\demo.txt*。
   - ``parent``: 返回路径的父目录，*C:\Users\boyin\Downloads*。
   - ``parents``: 返回路径的所有父目录。
   - ``name``: 返回路径的基本名称（文件名或目录名），demo.txt。
   - ``suffix``: 返回路径的后缀，.txt。
   - ``stem``: 返回路径的文件名（不包含后缀），demo。
   - ``anchor``: 返回锚，C:\。

3. 文件/目录检查
^^^^^^^^^^^^^^^^^^^

   - ``exists()``: 判断路径是否存在。
   - ``is_file()``: 判断路径是否是文件。
   - ``is_dir()``: 判断路径是否是目录。

4. 文件/目录操作
^^^^^^^^^^^^^^^^^^^

以下方法中有参数 ``exist_ok`` 如果设置为 True 则表示文件存在不进行任何操作，否则报错。``parents`` 设置为 True 则表示可以创建多级目录，否则报错。

   - ``mkdir()``: 创建目录。
   - ``touch()``: 创建文件。
   - ``unlink()``: 删除文件（危险操作，慎重）。
   - ``rmdir()``: 删除目录（危险操作，慎重）。

5. 路径拼接
^^^^^^^^^^^^^^^^^^^

   - ``/``: 实例化的 ``Path`` 路径可以使用 ``/`` 操作符拼接路径。
   - ``joinpath()``: 实现路径拼接。

6. typing.Any
^^^^^^^^^^^^^^^^^^^

特殊类型，表示没有约束的类型。

7. typing.Optional
^^^^^^^^^^^^^^^^^^^

Optional[X] 等价于 X | None （或 Union[X, None] ） 。

8. typing.Union
^^^^^^^^^^^^^^^^^^^

联合类型； Union[X, Y] 等价于 X | Y ，意味着满足 X 或 Y 之一。

注意：

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
