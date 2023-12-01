.. _concepts:

======================
高级概念
======================

本文收集了在 Python 编程中的一些高级概念。

.. contents:: :local:

.. _closure:

1. Python 中的闭包
----------------------------

闭包概念
^^^^^^^^^^^^^^^^^^^^^^^^^

闭包是一种在函数嵌套的情况下，内部函数引用了外部函数中变量的函数。这样的函数被称为闭包，因为它“封装”了外部函数的作用域。

在 Python 中，闭包通常是指包含对外部作用域中变量引用的函数。闭包可以访问包含它的外部函数的变量，即使这些变量在外部函数执行完毕后仍然存在。

.. code-block:: python

   def outer_function(x):
       def inner_function(y):
           return x + y
       return inner_function

在这个例子中，``inner_function`` 是一个闭包，因为它引用了外部函数 ``outer_function`` 的变量 ``x``。

闭包使用场景
^^^^^^^^^^^^^^^^^^^^^^^^^

**1. 保持状态**

闭包可以用于保持某个状态。例如，一个计数器函数可以通过闭包保持计数状态：

.. code-block:: python

   def counter():
       count = 0

       def increment():
           nonlocal count
           count += 1
           return count

       return increment

   counter_func = counter()
   print(counter_func())  # 输出: 1
   print(counter_func())  # 输出: 2

**2. 实现装饰器**

闭包在实现装饰器时非常有用。装饰器本质上是一个闭包，它可以包装其他函数，并在执行前后进行额外的操作。

.. code-block:: python

   def my_decorator(func):
       def wrapper():
           print("Something is happening before the function is called.")
           func()
           print("Something is happening after the function is called.")
       return wrapper

   @my_decorator
   def say_hello():
       print("Hello!")

   say_hello()

闭包注意事项
^^^^^^^^^^^^^^^^^^^^^^^^^

**1. 修改外部变量**

在闭包中修改外部函数中的变量时，需要使用 ``nonlocal`` 关键字，否则 Python 会将其视为局部变量。

.. code-block:: python

   def outer_function():
       x = 10

       def inner_function():
           nonlocal x
           x = 20

       inner_function()
       print("After inner_function:", x)

   outer_function()

**2. 循环中的闭包**

在循环中创建闭包时，要注意循环变量的作用域问题，可以通过默认参数或使用函数工厂解决。

.. code-block:: python

   def create_closure(value):
       def closure():
           print(value)
       return closure

   closures = [create_closure(i) for i in range(5)]

   for closure in closures:
       closure()


.. _decorator:

2. Python 装饰器详解
----------------------

装饰器概念
^^^^^^^^^^^

装饰器是一种用于修改函数或方法行为的机制，它允许你在不修改原始代码的情况下增强或改变函数的功能。**装饰器本质上是函数或类，可以接受一个函数作为输入，并返回一个新的函数或方法。**

装饰器是可调用的对象，其参数是另一个函数（被装饰的函数）。 装饰器可能会处理被装饰的函数，然后把它返回，或者将其替换成另一个函数或可调用对象。多数装饰器会修改被装饰的函数。通常，它们会定义一个内部函数，然后将其返回，替换被装饰的函数。


使用场景
^^^^^^^^^^^^^

**1. 函数修饰**

.. code-block:: python

   def my_decorator_1(func):
       def wrapper():
           print("Something is happening before the function is called.")
           func()
           print("Something is happening after the function is called.")
       return wrapper

   # 或者直接返回
   def my_decorator_2(func):
       # Do something...
       return func

   @my_decorator
   def say_hello():
       print("Hello!")

   # 调用被修饰后的函数
   say_hello()


**2. 类修饰**

.. code-block:: python

    class MyDecorator:
        def __init__(self, func):
            self.func = func

        def __call__(self):
            print("Something is happening before the function is called.")
            self.func()
            print("Something is happening after the function is called.")

    @MyDecorator
    def say_hello():
        print("Hello!")

    # 调用被修饰后的函数
    say_hello()

注意事项
^^^^^^^^^^^

**1. 语法糖 @ 的使用**

装饰器可以通过 ``@decorator`` 语法糖更方便地应用于函数或方法。这种语法糖等同于 ``func = decorator(func)``。

**2. 参数传递**

如果装饰器本身需要接受参数，可以在其内部再定义一层函数，并返回这个函数。

.. code-block:: python

    def my_decorator_with_args(arg):
        def decorator(func):
            def wrapper():
                print(f"Decorator argument: {arg}")
                func()
            return wrapper
        return decorator

    @my_decorator_with_args("example")
    def say_hello():
        print("Hello!")

    # 调用被修饰后的函数
    say_hello()

**3. 多个装饰器的顺序**

多个装饰器可以串联使用，但是它们的顺序很重要，因为它们按照从下到上的顺序执行。

.. code-block:: python

    @decorator1
    @decorator2
    def my_function():
        pass

**4. 保留原函数的签名和字符串文档**

如果想保留装饰器后函数的签名，可以使用 ``functools`` 模块中的 ``wraps`` 装饰器。``wraps`` 装饰器实际上是一个装饰器工厂函数，它用于包装一个装饰器，确保被装饰的函数保留原始函数的元信息，包括函数名、文档字符串、参数签名等。

.. code-block:: python

    from functools import wraps

    def my_decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            print("Something is happening before the function is called.")
            result = func(*args, **kwargs)
            print("Something is happening after the function is called.")
            return result
        return wrapper

    @my_decorator
    def say_hello(name):
        """A simple function that greets a person."""
        print(f"Hello, {name}!")

    # 使用装饰后的函数，保留了原函数的签名和文档字符串
    say_hello("John")

    # 输出函数签名
    print(say_hello.__name__)  # 输出: say_hello
    print(say_hello.__doc__)   # 输出: A simple function that greets a person.

**5. 装饰器执行时间**

装饰器的一个关键特性是，它们在被装饰的函数定义之后立即运行。这通常是在导入时（即 Python 加载模块时）。

装饰器是在定义函数时被调用的，而不是在函数被调用时。当 Python 解释器加载模块并遇到被装饰的函数定义时，装饰器会立即执行。这意味着装饰器的代码在程序运行过程中只执行一次。

.. code-block:: python

    def my_decorator(func):
        print("Decorator is executed when the function is defined.")
        def wrapper():
            print("Wrapper is executed when the decorated function is called.")
            func()
        return wrapper

    @my_decorator
    def say_hello():
        print("Hello!")

    # Output:
    # Decorator is executed when the function is defined.

*装饰器函数与被装饰的函数在同一个模块中定义。实际情况是，装饰器通常在一个模块中定义，然后应用到其他模块中的函数上。*