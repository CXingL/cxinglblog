---
title: Python 装饰器
date: 2017-01-19 15:17:23
tags: [python, ]
---
装饰器可以让其他函数或类在不需要做任何代码修改的前提下增加额外功能，装饰器返回值是一个函数/类对象。
例如：

    import logging
    def use_logging(func):

        def wrapper():
            logging.warn("%s is running" % func.__name__)
            return func()
        return wrapper

    @use_logging
    def foo():
        print('i am foo')

    foo()
将会输出：

    WARNING:root:foo is running
    i am foo

<!-- more -->
## 关于 python 装饰器

**python 中的函数可以像普通变量一样当做参数传递给另一个函数，所以有了装饰器的用法。
装饰器的作用就是为已存在的对象添加额外功能。**

有一个函数：

    def foo():
        print('i am foo')

想要增加一个新的需求：记录下函数的执行日志，简单的方法：

    import logging
    def foo():
        print('i am foo')
        logging.info("foo is running")

当其他函数有类似的需求时，这种方法会产生大量雷同的代码，为了减少重复代码，可以重新定义一个新的函数：专门处理日志，日志处理后再执行真正的业务代码：

    import logging
    def use_logging(func):
        logging.warn(%s is running' % func.__name__)
        func()

    def foo():
        print('i am foo')
    
    use_logging(foo)

这种方法实现了我们的目的，但是每次调用的就是 use_logging 这个函数，而不是 foo 函数。如果使用装饰器，就可以避免这种情况

    import logging
    def use_logging(func):

        def wrapper():
            logging.warn("%s is running" % func.__name__)
            return func()
        return wrapper
    
    def foo():
        print('i am foo')

    foo = use_logging(foo)
    foo()

use_logging 就是一个装饰器，它是一个普通函数，吧真正业务逻辑函数 func 包裹在其中，返回一个叫 wrapper 函数。

#### 语法糖
@ 符号是 python 里装饰器的语法糖，它可以省略最后一次再次赋值的操作

    import logging
    def use_logging(func):

        def wrapper():
            logging.warn("%s is running" % func.__name__)
            return func()
        return wrapper

    @use_logging
    def foo():
        print("i am foo")

    foo()

如上，有了 @ ，就可以省去 foo = use_logging（foo） 这一句，只需在定义的地方加上装饰器。

** \*args、\*\*kwargs **
如果业务逻辑函数 foo 需要参数，例如：

    def foo(name):
        print ('i am %s' % name)

那就在定义 wrapper 函数时指定参数：

    def wrapper(name):
            logging.warn('%s is running' % func.__name__)
            return func(name)
        return wrapper

这样 foo 函数定义的函数参数就可以定义在 wrapper 函数中。如果是多个参数，或者不知道到底有几个参数时，可以用 \*args 来代替：

    def wrapper(*args):
            logging.warn("%s is running" % func.__name__)
            return func(*args)
        return wrapper

这样无论多少个参数，都可以完整的传递到 func 中去。如果 foo 函数定义了一些关键字参数，例如：

    def foo(name, age = None, height = None):
        print ('i am %s, age %s, height %s' % (name, age, height))

这种情况可以把 wrapper 函数指定关键字函数:

    def wrapper(*args, **kwargs):
            # args 是一个数组，kwargs 是一个字典
            logging.warn("%s is running" % func.__name__)
            return func(*args, **kwargs)
        return wrapper

#### 带参数的装饰器

装饰器的语法允许在调用时，提供其他参数，如 @decoration(a)。这样就可以更加灵活，例如在装饰器中指定日志的等级：

    def use_logging(level):
        def decorator(func):
            def wrapper(*args, **kwargs):
                if level == "warn":
                    logging.warn("%s is running" % func.__name__)
                elif level == "info"
                    logging.info("%s is running“) % func.__name__)
                return func(*args)
            return wrapper

        return decorator
    
    @use_logging(level = "warn")
    def foo(name = 'foo'):
        print("i am %s" % name)
    
    foo()

上面 use_logging 是允许带参数的装饰器。实际上是对原有装饰器的封装，并返回一个装饰器。
可以理解为一个含有参数的闭包，当 @use_logging(level='warn') 调用的时候，Python 能发现这一层的封装，并把参数传递到装饰器的环境中。
@use_logging(level='warn') 等价于 @decorator

#### 类装饰器
装饰器不仅可以是函数，还可以是类，类装饰器比函数装饰器有灵活度大、高内聚、封装性等优点。类装饰器使用 __call__ 方法, 只要使用 @ 将装饰器附加到函数上，就会调用此方法：

    class Foo(object):
        def __init__(self, func):
            self._func = func
        
        def __call__(self):
            print ('class decorator running')
            self._func()
            print ('class decorator ending')

    @Foo
    def bar():
        print ('bar')

    bar()

#### functools.wraps
使用装饰器有个缺点就是原函数的元信息不见了，比如函数的 docstring、__name__、参数列表：

    def logged(func):
        def with_logging(*args, **kwargs):
            print func.__name__   # 输出 'with_logging'
            print func.__doc__    # 输出 None
            renturn func(*args, **kwargs)
        return with_logging
    
    @logged
    def f(x):
        """does some math"""
        return x + x * x
    
    logged(f)

可以看出函数 f 被 with_logging 取代了，所以 docstring、\_\_name\_\_ 就变成了 with_logging 的信息。
使用 functools.wraps 能把原函数的元信息拷贝到装饰器里面的 func 函数中

    from functools import wraps
    def logged(func):
        @wraps(func)
        def with_logging(*args, **kwargs):
            print func.__name__
            print func.__doc__
            return func(*args, **kwargs)
        return with_logging
    
    @logged
    def f(x):
        """does some math"""
        return x + x * x

#### 装饰器顺序
一个函数可以同时定义多个装饰器：

    @a
    @b 
    @c 
    def f():
        pass

执行顺序是从里到外，最先调用里层的装饰器，最后调用外层的装饰器，等效于：

    f = a(b(c(f)))
