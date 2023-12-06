.. _design_patterns:

======================
设计模式
======================

.. contents:: :local:

.. _introduction:

简介
==============

该文档介绍了 Python 编程中的一些常见的设计模式与编程思想。设计模式有助于提高代码的可维护性、可读性和可扩展性，一般适合中大型项目。

设计模式通常按照其目的和用途被分为三个主要类别：

- 创建型模式（**Creational Patterns**）：工厂方法模式、抽象工厂模式、创建者模式、（原型模式）、单例模式等。
- 结构型模式（**Structural Patterns**）：适配器模式、桥模式、组合模式、装饰模式、外观模式、享元模式、代理模式等。
- 行为型模式（**Behavioral Patterns**）：解释器模式、责任链模式、命令模式、迭代器模式、中介者模式、备忘录模式、观察者模式、状态模式、策略模式、访问者模式、模版方法模式等。

.. _Creational Patterns:

创建型模式
============

该模式关注对象的创建机制，旨在解决对象的实例化过程。创建型模式的主要目标是使系统更加独立，减少系统对类的直接依赖。

.. _singleton:

1. 单例模式（Singleton Pattern）
----------------------------------

**保证一个类只有一个实例，并提供一个全局访问点。**

使用元类来设计单例模式。

.. code-block:: python

   class Singleton(type):

    __instance = None

    def __call__(cls, *args, **kwargs):

        if cls.__instance is None:
            cls.__instance = type.__call__(cls, *args, **kwargs)

        return cls.__instance
