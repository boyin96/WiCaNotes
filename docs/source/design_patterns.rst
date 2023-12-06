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

.. _Creational_Patterns:

创建型模式
============

该模式关注对象的创建机制，旨在解决对象的实例化过程。创建型模式的主要目标是使系统更加独立，减少系统对类的直接依赖。

.. _singleton:

1. 单例模式（Singleton Pattern）
----------------------------------

保证一个类只有一个实例，并提供一个全局访问点。

.. code-block:: python

   class Singleton(type):

    __instance = None

    def __call__(cls, *args, **kwargs):

        if cls.__instance is None:
            cls.__instance = type.__call__(cls, *args, **kwargs)

        return cls.__instance

.. _factory:

2. 工厂方法模式（Factory Method Pattern）和抽象工厂模式（Abstract Factory Pattern）
---------------------------------------------------------------------------------

在工厂方法模式中，定义一个创建对象的接口，但将实际的创建工作推迟到子类中。这样每个子类都可以通过实现工厂方法来创建具体的对象。工厂方法模式通过将对象的实例化延迟到子类来实现解耦。

抽象工厂模式提供一个接口，用于创建一系列相关或相互依赖的对象，而无需指定它们的具体类。它是工厂方法模式的推广，适用于需要创建一组相关或相互依赖的对象的情况。

.. code-block:: python

    from abc import abstractmethod, ABCMeta

    class Factory(object, metaclass=ABCMeta):

        @abstractmethod
        def create(self, **kwargs):

            pass

    class AbstractFactory(Factory, metaclass=ABCMeta):

        def __init__(self):

            self._factories = dict()

        @abstractmethod
        def create(self, **kwargs):

            pass

        def _register(self, key, factory):

            self._factories[str(key)] = factory


.. _prototype:

3. 原型模式（Prototype Pattern）
----------------------------------

通过复制现有对象来创建新对象，而不是通过实例化新的对象。这种模式适用于创建成本较高或复杂对象的场景，以提高性能和减少资源消耗。

.. code-block:: python

    from copy import deepcopy
    from types import MethodType


    class Prototype(object):

        def prototype(self, **attributes):

            obj = deepcopy(self)
            for attribute in attributes:
                if callable(attributes[attribute]):
                    setattr(obj, attribute, MethodType(attributes[attribute], obj))
                else:
                    setattr(obj, attribute, attributes[attribute])

            return obj

.. _Structural_Patterns:

结构型模式
============

该模式关注类和对象的组合，以形成更大的结构。结构型模式的目标是使系统更加灵活，更容易组合和扩展。

.. _decorator:

1. 装饰模式（Decorator Pattern）
----------------------------------

它允许通过将对象放入包装器类中来动态地改变对象的行为。这种模式在不改变原始类接口的情况下，通过添加新功能来扩展对象的功能。

.. code-block:: python

    from functools import partial
    from abc import ABCMeta, abstractmethod

    class Decorator(object, metaclass=ABCMeta):

        def __get__(self, instance, owner):

            return partial(self.__call__, instance)

        @abstractmethod
        def __call__(self, *args, **kwargs):

            pass

    class DecoratorSimple(Decorator, metaclass=ABCMeta):

        def __init__(self, func):

            self.func = func

    class DecoratorComplex(Decorator, metaclass=ABCMeta):

        @abstractmethod
        def __init__(self, *args, **kwargs):

            pass

        @abstractmethod
        def __call__(self, func, *args, **kwargs):

            pass

    class CallWrapper(DecoratorSimple):

        def __call__(self, instance, func):

            def wrapped(*args, **kwargs):
                return self.func(instance, func, *args, **kwargs)

            return wrapped

.. _Behavioral_Patterns:

行为型模式
============

该模式关注对象之间的通信、责任分配和算法的抽象。行为型模式的目标是提供一种对象之间的协作方式，以便它们能够更灵活地互相合作。

.. _reference:

参考
============

- `python-patterns <https://github.com/faif/python-patterns>`_

- `PyPattyrn <https://github.com/tylerlaberge/PyPattyrn>`_
