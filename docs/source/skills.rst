.. _skills:

======================
Python 编程技巧
======================

该文档介绍了 Python 编程中的一些技巧以便提高代码的可读性、可维护性、效率和功能性。

.. contents:: :local:

.. _1:

函数参数传递
----------------------------------

1. 当你想限制函数的参数只能通过关键字传递时，可以使用 * 符号。这个技巧可以帮助提高函数调用的清晰度和可读性。

.. code-block:: python

    def example_function(arg1, *, kwarg1):
            """
            限制只能通过关键字传递参数的函数。

            Parameters:
                arg1: 位置传递的参数
                kwarg1: 只能通过关键字传递的参数
            """
            print(arg1, kwarg1)

        # 正确的方式：使用关键字传递
        example_function(1, kwarg1=2)

        # 错误的方式：使用位置传递
        # example_function(1, 2)  # 这会引发 SyntaxError

