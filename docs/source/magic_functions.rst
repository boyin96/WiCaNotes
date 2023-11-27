.. _magic_functions:

========
魔法函数
========

.. contents:: :local:


.. _introduction:

Introduction
============

You need to know some basic calculus in order to understand how functions change over time (derivatives), and to calculate the total amount of a quantity that accumulates over a time period (integrals). The language of calculus will allow you to speak precisely about the properties of functions and better understand their behaviour.

Normally taking a calculus course involves doing lots of tedious calculations by hand, but having the power of computers on your side can make the process much more fun. This section describes the key ideas of calculus which you'll need to know to understand machine learning concepts.


.. _derivative:

Derivatives
===========

A derivative can be defined in two ways:

  #. Instantaneous rate of change (Physics)
  #. Slope of a line at a specific point (Geometry)

Both represent the same principle, but for our purposes it’s easier to explain using the geometric definition.

Introduction
------------

欢迎来到 Python 常见魔法函数文档。本文档将介绍 Python 中一些被称为“魔法函数”的特殊方法，这些方法在类中有特殊的用途。

Common Magic Functions
----------------------

本节将介绍一些常见的魔法函数及其用途。

:class:`~object.__init__`
~~~~~~~~~~~~~~~~~~~~~~~~~

`__init__` 方法是类的构造函数，用于在创建对象时进行初始化操作。

:class:`~object.__str__`
~~~~~~~~~~~~~~~~~~~~~~~~

`__str__` 方法定义了对象的字符串表示形式，通常在使用 `str()` 函数时调用。

:class:`~object.__len__`
~~~~~~~~~~~~~~~~~~~~~~~~

`__len__` 方法返回对象的长度，通常在使用 `len()` 函数时调用。

Usage
-----

本节将演示如何使用这些魔法函数。

:class:`~object.__init__` 的使用
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

为类定义 `__init__` 方法，并在创建对象时执行必要的初始化操作。

:class:`~object.__str__` 的使用
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

为类定义 `__str__` 方法，以便更友好地打印对象的字符串表示形式。

:class:`~object.__len__` 的使用
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

为类定义 `__len__` 方法，并在需要获取对象长度时使用 `len()` 函数。

References
----------

以下是一些参考文章，可以深入学习 Python 魔法函数的更多内容：

- `Python 官方文档 - Data Model <https://docs.python.org/3/reference/datamodel.html>`_

- `Understanding Python Metaclasses <https://realpython.com/python-metaclasses/>`_

- `Python Magic Methods by Example <https://rszalski.github.io/magicmethods/>`_
