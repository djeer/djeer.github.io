---
layout: post
title: Object vs Type vs Class vs Instance
---

Описание связи между объектом, типом, классом и экземпляром в Python.

## Объекты

Объекты - способ представления любых сущностей в Python. Все данные в Python представлены объектами и связями объектов. 
У каждого объекта есть идентификатор, тип и значение. Идентификатор (identity) никогда не меняется с момента создания объекта. 
Оператор `is` сравнивает идентификтаторы двух объектов; функция `id()` возвращает целочисленный идентификатор объекта.

*Для CPython, id(x) - адрес памяти, где хранится x.*

```
>>> a = 127
>>> b = 128
>>> id(a)
1465020576
>>> id(b)
1465020608
>>> a is b
False
```

В Python все - объект. Списки - это объекты. Строки, модули, функции - это объекты. Байткод программы хранится как объект. У всех объектов есть типы и идентификаторы:

```
>>> def dummy(): pass
...
>>> type(dummy), id(dummy)
(<class 'function'>, 2122090425880)
>>> type(dummy.__code__), id(dummy.__code__)
(<class 'code'>, 2122094942368)
>>>
```

Модель "все - объект" прослеживается и в самой реализации CPython. В коде CPyton к любой сущности, упомянутой выше, можно обратиться через указатель на стркутуру PyObject.

## Типы
У каждого объекта Python есть тип. Тип можно выяснить, вызвав "встроенную функцию" `type()`. Тип - это тоже объект, поэтому у него есть собственный тип, который называется `type`. Этот факт не слишком полезен для написания простого кода на Python, однако, он важен для понимания внутренностей CPython:

```
>>> type(42)
<class 'int'>
>>> type(type(42))
<class 'type'>
>>> type(type(type(42)))
<class 'type'>
```
Все типы сходятся к type.

## Классы
Давным-давно была разница между встроенными классами и пользовательскими классами, но с версии 2.2 (классы "нового стиля", унаследованные от object в 2.x, и все классы в 3.x) значимых отличий нет. В сущности, класс это механизм Python, который позволяет создавать пользовательские типы в коде Python.
```
>>> class Snake: pass
...
>>> s = Snake()
>>> type(s)
<class '__main__.Snake'>
>>>
```
Используя этот механизм, мы создали Snake - пользовательский тип. s - экземпляр класса Snake. Или, другими словами, s - это объект, и его тип - Snake. Термины "class" и "type" оба ссылаются на одну концепцию.

## Экземпляры (instance)
В отличие от неоднозначности терминов "класс" и "тип", экземпляр - это синоним термина "объект".
Еще раз: объекты - это экземпляры типов. Так, "42 это экземпляр типа int" равносильно "42 - объект int". 
Термин "экземпляр" используется во встроенной функции `isinstance()`.

В Python все - объект, поэтому любой пользовательский класс - это тоже объект, и его тип *обычно* "type". 
Повторим еще раз: все пользовательские типы *обычно* являются экземплярами класса type. 

На примере класса Snake:
```
>>> type(Snake)
<class 'type'>
```

Введем новый термин: Метакласс - это специальный класс, конструктор которого возвращает класс. То есть, связь метакласс->класс аналогична связи класс->объект. Тот факт, что классы являются экземплярами класса "type", позволяет нам создавать свои метаклассы (классы, которые наследуются от класса "type" и конструктор которых возвращает класс). 

```
>>> class TestMeta(type): pass  # создаем простейший метакласс
...
>>> MyClass = TestMeta("myclass",tuple(),{})  # создаем экземпляр метакласса
>>> MyClass
<class '__main__.myclass'>
>>> type(MyClass)  # получили пользовательский класс c типом, отличным от type
<class '__main__.TestMeta'>
>>> type(type(MyClass))
<class 'type'>
```

Зачем? Как правило, метаклассы используют при написании сложных фреймворков уровня Django ORM, они позволяют вынести часть внутренней магии выше пользовательского кода, чтобы "пользователь" не вдавался в подробности реализации. Как метко заметил гуру питона Тим Питерс, *Если вы сомневаетесь, надо ли вам их использовать — не надо.*

## Итог
Напрашивается наивный вопрос: так что же главнее - type или object?
```
>>> type.__bases__
(<class 'object'>,)
>>> object.__bases__
()
```
type наследуется от object, но при этом же:
```
>>> type(object)
<class 'type'>
```
object имеет тип type. По сути, object - корень иерархии объектов, а type - корень иерархии типов. Каждый тип это объект, а у каждого объекта есть тип, этим и вызвана такая двойственность их определений. Среди них нет первоисточника, это единственное подобное исключение, заложенное в основу языка.

Рассмотрим на примере стандартных классов. Класс bool наследуется от класса int. На картинке наследование объектов показано зеленым, наследование типов - красным.


![_config.yml]({{ site.baseurl }}/images/Diagram1.png)


И добавим сюда классы из примеров выше:


![_config.yml]({{ site.baseurl }}/images/Diagram2.png)


Таким образом, метаклассы позволяют реализовать иерархию типов, подобную наследованию объектов. Практический смысл метаклассов - при создании класса мы можем выполнить произвольный код, который модифицирует создаваемый класс, без необходимости вносить изменения в сам класс *(но в 99% случаев подобное лучше делать через декораторы классов/наследование)*.


## Примечание
Следует отличать встроенную функцию `isinstance()` и оператор `is`:

`isinstance(object, class)` возвращает True, если `object` является экземпляром `class`, либо экземпляром класса, унаследованного от `class`.

Оператор `is`, в свою очередь, возвращает True, если две переменные ссылаются на один объект, т.е. сверяет идентификаторы объектов.

## Динамическое создание классов через type()
Вместо вызова с одиним аргументом, `type()` можно вызвать с тремя:

`type(classname, superclasses, attributes_dict)`

Если `type` вызван с тремя аргументами, он вернет новый объект `type`. Это позволяет динамически формировать классы (как это сделано в примере с метаклассом):

* "classname" - строка, которая определяет имя класса и становится его атрибутом `__name__`;
* "superclasses" - это кортеж родительских классов, становится атрибутом `__bases__`;
* "attributes_dict" - это словарь, представляющий пространство имен нашего класса. Он содержит определенние тела класса и становится его атрибутом `__dict__`. Атрибутами класса могут быть любые объекты:

```
>>> m = type("mytype", (int,), {"test": "field", "greet": lambda: print("hello word")})
>>> m.test
'field'
>>> m.greet()
hello word
```

Иногда type() называют встроенной функцией, но надо понимать, что type(1), хоть и выглядит как print(1), на самом деле является вызовом констркутора типа type с одиним аргументом. Вызов конструктора с одним аргументом возвращает тип переданного объекта, с тремя - создает новый тип. Вот так выглядит сигнатура `type.__init__()`:
```
def __init__(cls, what, bases=None, dict=None):
        """
        type(object_or_name, bases, dict)
        type(object) -> the object's type
        type(name, bases, dict) -> a new type
        """
        ...
```

P.S. еще одна неплохая статья про метаклассы https://habrahabr.ru/post/145835/
