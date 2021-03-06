

第二章: 语言基础
=====================


.. toctree::
    :maxdepth: 3

定义C变量和类型
---------------------

使用 `cdef` 来声明变量:

.. code-block:: python

    cdef int i, j, k
    cdef float f, g[42], *h
    
    cdef struct Grail:
         int age
         float volume

    cdef union Food:
         char *spam
         float *eggs

    cdef enum CheeseType:
         cheddar, edam,
         camembert

    cdef enum CheeseState:
         hard = 1
         soft = 2
         runny = 3


目前无法直接定义常量，可以通过枚举方式:

.. code-block:: python

    cdef enum:
       tons_of_spam = 3


需要注意， `struct,union,enum` 只能用于定义类型，不能用于定义引用:

.. code-block:: c

    cdef struct Grail *gp # WRONG

    cdef Grail *gp  #Right


可以通过 `ctypedef` 来重定义类型名:

.. code-block:: c

    ctypedef unsigned long ULong

    ctypedef int* IntPtr


类型
~~~~~~~~~~~~~~~~~~~~~

`Cython` 使用近似C的语法表示C类型(包括指针). 它支持所有的标准C类型char,short,int,long,long long及其unsigned版本，如 unsigned int. 其中特殊的类型，一个是bint类型用于表示C布尔类型(0表示False,非0表示True),另一个是 `Py_ssize_t类型` 表示Python容器的大小。

可以用C语言里面的方式在Cython里面构建指针类型，方法是在指针对应的类型后添加字符 `*`, 比如 int** 表示指向C int类型的指针的指针.数组构建方法是直接使用C语言的语法，如 int[10]. 注意，Cython使用数组的访问方式来访问指针引用内容，因此 `*x` 是错误的Python语法，应该是 x[0].

同时，Python类型list,dict,tuple，乃至任何用户自定义扩展类型都可以用作静态类型；但是，Python的int,long类型是不可以用于静态类型，需要替换为C的int,long
类型，因为Python整数类型用于静态类型没有任何优势。

分组的C声明
~~~~~~~~~~~~~~~~~~~~~
