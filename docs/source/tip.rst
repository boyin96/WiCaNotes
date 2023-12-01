.. _python-programming-tips:

======================
编程注意事项
======================

该文档收集了在 Python 编程中的一些注意事项和最佳实践，以提高代码的可读性、可维护性和性能。

1. 避免使用可变的默认参数
----------------------------------

默认参数应该是不可变对象，避免使用可变对象（如列表或字典）作为函数参数的默认值。这可以防止不期望的副作用，因为可变默认参数在函数间共享同一个对象。

.. code-block:: python

    # 不推荐
    def process_data(data=[]):
        data.append(42)
        return data

    # 推荐
    def process_data(data=None):
        if data is None:
            data = []
        data.append(42)
        return data
