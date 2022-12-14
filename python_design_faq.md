## Частые вопросы по дизайну и истории *Python* (перевод [статьи](https://docs.python.org/3/faq/design.html))

В ответах, если не указано иное, подразумевается стандартная реализация языка *Python* -- *CPython*.

Список вопросов актуален на 20 октября 2022 года.

## Содержание

[Почему в *Python* для группировки операторов используются отступы?](#почему-в-python-для-группировки-операторов-используются-отступы)

[Почему при выполнении простых арифметических операций я получаю странные результаты?](#почему-при-выполнении-простых-арифметических-операций-я-получаю-странные-результаты)

[Почему вычисления чисел с плавающей запятой такие неточные?](#почему-вычисления-чисел-с-плавающей-запятой-такие-неточные)

[Почему в *Python* строки неизменяемы?](#почему-в-python-строки-неизменяемы)

[Почему при определении методов и их вызове нужно явно использовать `self`](#почему-при-определении-методов-и-их-вызове-нужно-явно-использовать-self)

[Почему я не могу использовать присваивание прямо в выражении?](#почему-я-не-могу-использовать-присваивание-прямо-в-выражении)

[Когда *Python* использует функции, а когда методы?](#когда-python-использует-функции-а-когда-методы)

[Почему `join()` является методом строки, а не списка или кортежа?](#почему-join-является-методом-строки-а-не-списка-или-кортежа)

[Насколько дорога обработка исключений?](#насколько-дорога-обработка-исключений)

[Почему в *Python* нет операторов `switch` или `case`?](#почему-в-python-нет-операторов-switch-или-case)

[Разве вы не можете эмулировать потоки вместо того, чтоб полагаться на специфическую для каждой ОС реализацию потока?](#разве-вы-не-можете-эмулировать-потоки-вместо-того-чтоб-полагаться-на-специфическую-для-каждой-ос-реализацию-потока)

[Почему анонимные функции не могут содержать операторов?](#почему-анонимные-функции-не-могут-содержать-операторов)

[Можно ли скопмилировать *Python* в машинный код, *С* или другой язык?](#можно-ли-скопмилировать-python-в-машинный-код-с-или-другой-язык)

[Как *Python* управляет памятью?](#как-python-управляет-памятью)

[Почему *CPython* не использует более традиционную схему сбора мусора?](#почему-cpython-не-использует-более-традиционную-схему-сбора-мусора)

[Почему при выходе из *CPython* освобождается не вся память?](#почему-при-выходе-из-cpython-освобождается-не-вся-память)

[В чем разница между списками и кортежами?](#в-чем-разница-между-списками-и-кортежами)

[Как в *CPython* реализованы списки?](#как-в-cpython-реализованы-списки)

[Как в *CPython* реализованы словари?](#как-в-cpython-реализованы-словари)

[Почему ключи в словаре должны быть неизменяемыми?](#почему-ключи-в-словаре-должны-быть-неизменяемыми)

[Почему `list.sort()` не возвращает сортированный список](#почему-listsort-не-возвращает-сортированный-список)

[Как указать и обеспечить соблюдение спецификации интерфейса в *Python*?](#как-указать-и-обеспечить-соблюдение-спецификации-интерфейса-в-python)

[Почему в *Python* нет оператора `goto`?](#почему-в-python-нет-оператора-goto)

[Почему сырые строки (r-строки) не могут заканчиваться обратной косой чертой?](#почему-сырые-строки-r-строки-не-могут-заканчиваться-обратной-косой-чертой)

[Почему в *Python* нет оператора `with` для присвоения атрибутов?](#почему-в-python-нет-оператора-with-для-присвоения-атрибутов)

[Почему генераторы не поддерживают оператор `with`?](#почему-генераторы-не-поддерживают-оператор-with)

[Почему для операторов `if`/ `while`/ `def`/ `class` требуются двоеточия?](#почему-для-операторов-if-while-def-class-требуются-двоеточия)

[Почему *Python* допускает запятые в конце списков и кортежей?](#почему-python-допускает-запятые-в-конце-списков-и-кортежей)


## Почему в *Python* для группировки операторов используются отступы?

Гвидо ван Россум верит, что использование отступов для группировки операторов 
чертовски элегантно и вносит ясность в среднестатистическую программу, 
написанную на *Python*. И спустя некоторое время большинство пользователей 
влюбляются в эту особенность.

Поскольку отсутствуют начальные и конечные скобки, то нет разногласий между 
группировкой, вопринимаемой машиной и человеком. Время от времени программисты 
на *C* встречаются с фрагментами кода, похожими на этот:

```c
if (x <= y)
        x++;
        y--;
z++;
```

В случае истинности условия `x <= y` выполняется только оператор `x++`, однако 
отступ может внести сомнения в этом. Даже опытные программисты иногда 
задумываются, почему `y` уменьшается независимо от выполнения условия.

*Python* гораздо меньше подвержен конфликтам, связанным со стилем написания, 
так как в нем нет начальных и конечных скобок. В языке *C* есть разные способы 
расположения скобок. После того, как пользователь привыкнет к чтению кода, 
написанном в каком-то определенном стиле, вполне нормально, что при чтении 
кода, написанном в ином стиле, могут возникнуть сложности.

Многие стили кодирования подразумевают расположение скобок на отдельных 
строках, что делает программы длиннее, тратится ценное место на экране, 
а чтение и анализ кода становятся сложнее. В идеале функция должна занимать 
один экран (скажем, 20 -- 30 строк). Код на *Python* длинной в 20 строк может 
сделать гораздо больше кода на *C* аналогичной длины. Причина этого не только 
в отсутствии скобок, но и, например, в остуствии объявления переменных или 
использовании высокоуровневых типов данных. Ну и, конечно, помогает 
синтаксис с использованием отступов.

[К содержанию](#содержание)


## Почему при выполнении простых арифметических операций я получаю странные 
результаты?

Смотри следующий вопрос.

[К содержанию](#содержание)


## Почему вычисления чисел с плавающей запятой такие неточные?

Пользователи очень часто удивляются результатам подобным этому: 

```
>>>
>>> 1.2 - 1.0
0.19999999999999996
```

и думают, что это баг языка *Python*. Это не так. Дело здесь не в языке, 
а в платформе, на которой он реализован.

В *CPython* для хранения и обработки чисел с плавающей запятой используются 
числа типа `double` языка *C*. Значение объекта с плавающей запятой 
хранится в двоичном формате с фиксированной точностью (обычно 53 бита) 
и *Python* использует операции языка *C*, которые, в свою очередь, полагаются
 на аппаратную реализацию операций с плавающей запятой. Это означает, что при
  операциях с числами с плавающей запятой, *Python* ведет себя так же как другие
   популярные языки, например, *C* или *Java*.

Многие числа, которые легко записать в десятичной системе, невозможно записать
 в двоичном формате с плавающей запятой. Например:

```
>>>
>>> x = 1.2
```

Значение, сохраненное для `x`, является приближенным (с хорошей точностью) 
к 1,2, но не равняется ему. Фактически значение в двоичном формате равно:

```python
1.0011001100110011001100110011001100110011001100110011
```

что в десятичной системе равняется

```python
1.1999999999999999555910790149937383830547332763671875
```

Обычная точность в 53 бита позволяет языку *Python* хранить числа с точностью 
до 15 -- 16 знака после запятой.

[К содержанию](#содержание)


## Почему в *Python* строки неизменяемы?

У этого решения есть несколько преимуществ.

Первое -- производительность. Знание того, что строка неизменяема позволяет 
сразу выделить необходимое количество памяти во время создания объекта, 
в дальнейшем оно не изменится. Это так же является одним из различий между
 списками и кортежами.

Второе -- строки в *Python* являются такими же "элементарными", как числа. 
Ничто не может изменить значение числа 8, также ничто не может изменить 
значение строки "восемь".

[К содержанию](#содержание)


## Почему при определении методов и их вызове нужно явно использовать `self`?

Эта идея позаимствована из языка *Modula-3*. И, по некоторым причинам, это 
оказалось полезным.

Во-первых, более очевидно, что вы используете метод или атрибут экземпляра 
вместо локальной переменной. Запись `self.x` или `self.meth()` ясно 
показывает, что используется атрибут экземпляра или метод, даже если вы 
не знаете определение класса наизусть. В *C++* это можно определить 
по отсутствию объявления локальной переменной (при условии, что глобальные 
переменные редки или легко распознаваемы), но в Python нет объявлений 
локальных переменных, поэтому вам придется искать определение класса, чтобы 
быть уверенным. Некоторые стандарты кодирования *C++* и *Java* требуют, чтобы 
атрибуты экземпляра имели префикс `m_`, так что эта явность по-прежнему 
полезна и в этих языках.

Во-вторых, нет необходимости в специальном синтаксисе, если вы хотите явно 
сослаться на метод или вызвать его из определенного класса. В *C++*, если вы 
хотите использовать метод из родительского класса, который переопределен 
в дочернем классе, вы должны использовать оператор `::` -- в *Python* вы 
можете написать `baseclass.methodname(self, <argument list>)`. Это особенно 
полезно для методов `__init__()` и вообще в тех случаях, когда метод дочернего 
класса хочет расширить одноименный метод родительского класса и, 
таким образом, должен каким-то образом вызывать метод базового класса.

Наконец, это решает синтаксическую проблему с присваиванием: поскольку 
локальные переменные в Python -- это (по определению!) те переменные, которым 
присваивается значение в теле функции (и которые явно не объявлены 
глобальными), то мы должны быть каким-то способом сообщить интерпретатору, что 
присваивание предназначено для переменной экземпляра, а не локальной 
переменной, и желательно, чтобы оно было синтаксически понятным (по 
соображениям эффективности). *C++* делает это с помощью объявлений, но 
в *Python* нет объявлений, и было бы жаль вводить их только для этой цели. 
Использование явного `self.var` прекрасно решает эту проблему. Точно так же 
для использования переменных экземпляра -- необходимость писать `self.var` 
означает, что ссылки на неполные имена внутри метода (без `self`) не должны 
искать переменные внутри каталогов экземпляра. Иными словами, локальные 
переменные и переменные экземпляра находятся в двух разных пространствах имен, 
и вам нужно указать, какое пространство имен использовать.

[К содержанию](#содержание)


## Почему я не могу использовать присваивание прямо в выражении?

Начиная с *Python 3.8* можешь!

Моржовый оператор `:=` позволяет присваивать значение прямо в выражении:

```python
while chunk := fp.read(200):
   print(chunk)
```

[К содержанию](#содержание)


## Когда *Python* использует функции, а когда методы?

Слово Гвидо ван Россуму:

1. у некоторых операций префиксная нотация читается лучше, чем постфиксная (и 
инфиксная!) -- эта традиция пришла из математики, где сама запись помогает 
визуализировать проблему и обдумать ее. Посмотрите, как легко мы можем 
записать формулу `x * (a + b)` как `x * a + x * b`, и с какой сложностью мы 
сталкиваемся, когда пытаемся сделать то же самое, используя 
объектно-ориентированный подход 
(`x.multiply(a.add(b))` и `(x.multiply(a)).add(x.multiply(b))`)!
2. когда я читаю код вроде `len(x)`, я понимаю, что он запрашивает длину 
какого-то объекта `x`. Так же я понимаю две вещи: ответом будет целое число, 
а аргумент -- какой-то контейнер. В случае с `x.len()` я *должен знать*, что 
`x` -- контейнер, реализующий интерфейс, или наследующийся от класса, который 
имеет стандартную `len()`. Обратите внимание на путаницу, когда какой-то класс 
не реализует мэппинг, но, при этом, имеет методы `get()` или `keys()`, или 
что-то, что не является файлом, имеет метод `write()`.

[К содержанию](#содержание)


## Почему `join()` является методом строки, а не списка или кортежа?

Строки стали более похожи на другие стандартные типы, начиная с *Python 1.6*, 
когда к ним были добавлены методы, предоставляющие ту же функциональность, что 
и функции модуля строк. Большинство из этих методов получили широкое 
признание, но один из них, судя по всему, заставляет пользователей чувствовать 
себя неуютно:

```python
", ".join(['1', '2', '4', '8', '16'])
```

В результате имеем:

```python
"1, 2, 4, 8, 16"
```

Существует два распространенных аргумента против его использования.

Первый звучит так: «Использование метода строкового литерала (строковая 
константа) выглядит очень уродливо», на что мы отвечаем: может быть, но 
строковый литерал -- это просто фиксированное значение. Если методы разрешены 
для имен, связанных со строками, нет логической причины делать их недоступными 
для литералов.

Второе возражение обычно формулируется так: «Я говорю *последовательности*, 
чтобы она соединяла свои элементы с помощью *строковой* константы». 
К сожалению, это не так. По какой-то причине кажется, что использование 
`split()` в качестве строкового метода представляет гораздо меньшую сложность, 
но, например, в этом случае легко увидеть, что

```python
"1, 2, 4, 8, 16".split(", ")
```

-- это инструкция строковому литералу возвращать подстроки с заданным 
разделителем (по умолчанию, пробелами).

[К содержанию](#содержание)


## Насколько дорога обработка исключений?

Если исключений не возникает, то конструкция `try/except` чрезвычайно 
эффективна. Но на самом деле обработка исключений -- дорогое удовольствие. 
До *Python 2.0* использовалась такая конструкция:

```python
try:
    value = mydict[key]
except KeyError:
    mydict[key] = getvalue(key)
    value = mydict[key]
```

Это имеет смысл только в том случае, если вы всегда (или пости всегда) 
ожидаете ключ `key` в словаре `mydict`. В ином случае нужно было использовать

```python
if key in mydict:
    value = mydict[key]
else:
    value = mydict[key] = getvalue(key)
```

В этом конкретном случае вы также можете использовать 
`value = dict.setdefault(key, getvalue(key))`, но только в том случае, если 
вызов `getvalue()` достаточно дешев, потому что он вычисляется во всех случаях.

[К содержанию](#содержание)


## Почему в *Python* нет операторов `switch` или `case`?

Вы можете легко реализовать аналоги этих операторов используя 
`if... elif... else...`. Для литератов или констант в пространстве имен вы 
также можете использовать `match... case...`.

В случаях, когда нужно выбрать из большого количества вариантов, вы можете 
пользоваться словарем:

```python
functions = {'a': function_1,
             'b': function_2,
             'c': self.method_1}

func = functions[value]
func()
```

Вызов методов или объектов можно упростить с помощью встроенной функции 
`getattr()`:

```python
class MyVisitor:
    def visit_a(self):
        ...

    def dispatch(self, value):
        method_name = 'visit_' + str(value)
        method = getattr(self, method_name)
        method()
```

Для защиты от действий злоумышленников в именах методов рекомендуется 
использовать префикс (в примере -- `visit_`).

[К содержанию](#содержание)


## Разве вы не можете эмулировать потоки вместо того, чтоб полагаться на специфическую для каждой ОС реализацию потока?

1. К сожалению, интерпретатор помещает по крайней мере один фрейм стека *C* 
для каждого фрейма стека *Python*. Кроме того, расширения могут вызывать 
*Python* почти в случайные моменты. Поэтому для полной реализации потоков 
требуется поддержка потоков для *C*.

2. К счастью, существует Stackless Python с полностью переработанным циклом 
интерпретатора, который позволяет избежать стека *C*.

[К содержанию](#содержание)


## Почему анонимные функции не могут содержать операторов?

Анонимные функции в *Python* не могут содержать операторов, поскольку 
синтаксис языка не посзволяет обрабатывать операторы, вложенные в выражения. 
Однако это не является серьезной проблемой. В отличие от ананонимных функций в 
других языках, где они добавляют функциональность, в *Python* 
лямбда-выражения -- всего лишь синтаксический сахар, нужный когда вам лень 
определять функцию.

Функции в *Python* уже являются объектами первого класса и могут быть 
объявлены в локальной области видимости. Следовательно, единственное 
преимущество использования лямбда-выражений вместо локально определенных 
функций состоит в том, что вам не нужно придумывать для них имя!

[К содержанию](#содержание)


## Можно ли скопмилировать *Python* в машинный код, *С* или другой язык?

*Cython* компилирует модифицированную версию *Python* с необязательными 
аннотациями в расширения *C*. *Nuitka* -- многообещающий компилятор *Python* 
в код *C++*, целью которого является поддержка всего языка *Python*.

[К содержанию](#содержание)


## Как *Python* управляет памятью?

Детали управления памятью зависят от конкретной реализации *Python*.  
Стандартная реализация -- *CPython* -- для обнаружения неиспользуемых объектов 
использует счетчик ссылок и механизм поиска неиспользуемых циклов, который 
удаляет задействованные в них объекты. Модуль `gc` предоставляет функции для 
сбора мусора, получения статистики отладки и настройки параметров сборщика.

Другие реализации (такие как *Jython* и *PyPy*) могут полагаться на другой 
механизм -- полноценный сборщик мусора. Использование в разных реализациях 
разных механизмов сбора мусора может вызвать некоторые проблемы при переносе 
код, если он (код) зависит от реализации механизма подсчета ссылок.

В некоторых реализациях *Python* следующий код (который отлично подходит для 
*CPython*), вероятно, исчерпает файловые дескрипторы:

```python
for file in very_long_list_of_files:
    f = open(file)
    c = f.read(1)
```

Действительно, используя схему подсчета ссылок *CPython*, каждое новое 
присваивание `f` закрывает предыдущий файл. Однако с традиционным сборщиком 
мусора эти файловые объекты будут собираться (и закрываться) только через 
различные и, возможно, длительные интервалы времени.

Если вы хотите написать код, который будет работать независимо от конкретной 
реализации *Python*, вы должны явно закрыть файл или использовать 
оператор `with`:

```python
for file in very_long_list_of_files:
    with open(file) as f:
        c = f.read(1)
```

[К содержанию](#содержание)


## Почему *CPython* не использует более традиционную схему сбора мусора?

Во-первых, это не стандартная функция *C* и, следовательно, она не переносима. 
Да, мы знаем о библиотеке `Boehm GC` (также известной как `bdwgc`). В ней есть 
фрагменты кода на Ассемблере для большинства распространенных платформ, 
но не для всех; для работы с ней *Python* требуются патчи.

Традиционный сборщик мусора также становится проблемой, когда *Python* 
встраивается в другие приложения. В то время как в автономном *Python* 
нормально заменить стандартные `malloc()` и `free()` версиями, 
предоставляемыми библиотекой `gc`, приложение, встраивающее *Python*, может 
иметь свою собственную замену для `malloc()` и `free()`, отличную от *Python*. 
Прямо сейчас *CPython* работает со всем, что правильно реализует 
`malloc()` и `free()`.

[К содержанию](#содержание)


## Почему при выходе из *CPython* освобождается не вся память?

При выходе из *Python* освобождаются не все объекты, на которые ссылаются 
глобальные пространства имен. Это происходит при наличии циклических ссылок. 
Также существуют определенные биты памяти, выделенные библиотекой *C*, которые 
невозможно освободить (например, такой инструмент, как *Purify*, будет 
жаловаться на это). Однако, *Python* агрессивно относится к очистке памяти при 
выходе и пытается уничтожить каждый отдельный объект.

Если вы хотите, чтобы *Python* удалял определенные вещи при освобождении, 
используйте модуль `atexit`.

[К содержанию](#содержание)


## В чем разница между списками и кортежами?

Списки и кортежи во многом схожи, однако используются принципиально разными 
способами.

Кортежи можно рассматривать как аналогичные записям *Pascal* или структурам 
*C*; это небольшие наборы связанных данных, которые могут относиться к разным 
типам и обрабатываются как группа. Например, декартовы координатыцелесообразно 
представить в виде кортежей из двух или трех чисел.

Списки же больше похожи на массивы в других языках. Они, как правило, содержат 
различное количество объектов одного типа, которые обрабатываются один 
за другим. Например, `os.listdir('.')` возвращает список строк, представляющих 
файлы в текущем каталоге. Функции, работающие с этим выводом, обычно 
не прерываются, если вы добавите в каталог еще один или два файла.

Кортежи неизменяемы, а это означает, что после создания кортежа вы не можете 
заменить ни один из его элементов новым значением. Списки изменяемы, то есть 
что вы всегда можете изменить элементы списка. В качестве ключей словаря можно 
использовать только неизменяемые элементы, следовательно, на эту роль подходят 
кортежи, а не списки.

[К содержанию](#содержание)


## Как в *CPython* реализованы списки?

Списки в *CPython* на самом деле представляют собой массивы переменной длины, 
а не связанные списки в стиле *Lisp*. Реализация использует непрерывный массив 
ссылок на другие объекты и хранит указатель на этот массив и длину массива 
в структуре заголовка списка.

Это делает индексирование списка `a[i]` операцией, стоимость которой 
не зависит от размера списка или значения индекса.

Когда в список добавляются новые элементы, размер массива ссылок изменяется. 
Некоторая хитрость применяется для повышения производительности многократного 
добавления элементов; когда массив должен быть увеличен, выделяется 
дополнительное пространство, поэтому в следующие несколько раз не требуется 
фактическое изменение размера.

[К содержанию](#содержание)


## Как в *CPython* реализованы словари?

Словари в *CPython* реализованы как хэш-таблицы с изменяемым размером. 
По сравнению с бинарными деревьями хэш-таблицы имеют более простую реализацию, 
а так же быстрее выполняют операцию поиска (самая распространенная операция).

Словари работают, вычисляя хэш-сумму для каждого ключа, хранящегося в словаре, 
с помощью встроенной функции `hash()`. Хэш-сумма сильно различается 
в зависимости от ключа и начального символа; например, «Python» может иметь 
значение -539294296, тогда как «python» -- строка, отличающаяся на один 
бит -- может иметь значение 1142331976. Хэш-сумма используется для вычисления 
местоположения во внутреннем массиве, где будет храниться значение. Так как вы 
храните ключи с разными хэш-суммами, то для извлечения ключа из словаря 
требуется постоянное время -- O(1).

[К содержанию](#содержание)


## Почему ключи в словаре должны быть неизменяемыми?

Реализация словарей использует значение хеш-суммы, вычисленное из значения 
ключа, чтобы найти местоположение ключа в памяти. Если бы ключ был изменяемым 
объектом, его значение, и, следовательно, его хэш-сумма, могли бы измениться.
Если вы попытаетесь найти тот же объект в словаре (после изменения ключа), то 
он не будет найден, потому что его хеш-сумма изменится. Если вы попытаетесь 
найти старое значение, оно также не будет найдено, потому что значение 
объекта, найденного в этом хеш-бункере, будет другим.

Если вы хотите, чтобы ключ словаря совпадал со списком, сначала преобразуйте 
список в кортеж; функция `tuple(L)` создает кортеж с теми же элементами, 
что и список `L`.
Кортежи неизменяемы и поэтому могут использоваться в качестве ключей словаря.

Некоторые примеры, которые не будут работать:

- список хэш-сумм по их адресу (ID объекта). Не работает, потому что в случае 
создания списка с такими же значениями, он не будет найден. 
Например, при вызове:

```python
mydict = {[1, 2]: '12'}
print(mydict[[1, 2]])
```

будет получено исключение `KeyError`, поскольку идентификатор [1, 2], 
используемый во второй строке, отличается от идентификатора в первой строке. 
Другими словами, словарные ключи следует сравнивать с использованием `==`, а 
не `is`.

- копия списка при использовании его в качестве ключа не сработает, потому что 
список, будучи изменяемым объектом, может содержать ссылку на себя, и тогда 
  копирующий код зациклится.

- разрешить использовать списки в качстве ключей, но запретить их (списки) 
изменять. Это может привести к трудноотслеживаемым ошибкам в коде, когда вы 
забыли об этом и изменили список. К тому же это делает недействительным 
важный инвариант словарей: каждое значение в `d.keys()` можно использовать 
в качестве ключа словаря.

- отметить списки как доступные только для чтения. В этом случае изменить свое 
значение может не только объект верхнего уровня;  в качестве ключа вы можете 
использовать кортеж, содержащий список. Ввод чего-либо в качестве ключа 
в словарь потребовал бы пометить все объекты, достижимые оттуда, как доступные 
только для чтения, и опять же, объекты, ссылающиеся на самих себя, могут 
вызвать бесконечный цикл.

Можно использовать трюк, чтобы обойти этот ограничение, но использовать его 
нужно осторожно. Вы можете обернуть изменяемую структуру внутри экземпляра 
класса, который имеет методы `__eq__()` и `__hash__()`. Вы должны быть 
уверены, что хэш-суммы всех таких объектов-оболочек, которые находятся в 
словаре (или другой структуре на основе хэша), остаются фиксированными, пока 
объекты находятся в словаре (или другой структуре).

```python
class ListWrapper:
    def __init__(self, the_list):
        self.the_list = the_list

    def __eq__(self, other):
        return self.the_list == other.the_list

    def __hash__(self):
        l = self.the_list
        result = 98767 - len(l)*555
        for i, el in enumerate(l):
            try:
                result = result + (hash(el) % 9999999) * 1001 + i
            except Exception:
                result = (result % 7777777) + i * 333
        return result
```

Имейте в виду, что вычисление хеш-суммы осложняется тем, что некоторые члены 
списка могут быть не хэшируемыми, а также возможностью арифметического 
переполнения.

Кроме этого, всегда должно быть так, что если `o1 == o2` (т.е. `o1.__eq__(o2)` 
равно `True`), то `hash(o1) == hash(o2)` 
(т.е. `o1.__hash__() == o2.__hash__()`), независимо от того, находится объект 
в словаре или нет.

В случае использования `ListWrapper` всякий раз, когда объект-оболочка 
находится в словаре, обернутый список не должен изменяться, чтобы избежать 
аномалий. Не делайте этого, если вы не готовы серьезно подумать о требованиях 
и последствиях их неправильного выполнения. Считайте себя предупрежденным.

[К содержанию](#содержание)


## Почему `list.sort()` не возвращает сортированный список?

В ситуациях, когда важна производительность, создание копии списка только ради 
его сортировки -- расточительно. Поэтому `list.sort()` сортирует список на 
месте. Чтобы напомнить вам об этом, метод ничего не возвращает. Таким образом, 
вы не будете обмануты, случайно перезаписав список, когда вам нужна 
отсортированная копия, но при этом вам нужно сохранить несортированную версию.

Если вы хотите вернуть отсортированный список, используйте встроенный функцию 
`sorted()`. Функция принимает итерируемый объект, сортирует и возвращает его. 
Например, вот как перебирать ключи словаря в отсортированном порядке:

```python
for key in sorted(mydict):
    ...  # do whatever with mydict[key]...
```

[К содержанию](#содержание)


## Как указать и обеспечить соблюдение спецификации интерфейса в *Python*?

Спецификация интерфейса модуля, предоставляемая такими языками, как *C++* и 
*Java*, описывает прототипы методов и функций модуля. Многие считают, что 
соблюдение спецификаций интерфейса во время компиляции помогает при создании 
больших программ.

В *Python 2.6* был добавлен модуль `abc`, позволяющий вам определять 
абстрактные базовые классы (ABC, от Abstract Base Class). Далее вы можете 
использовать `isinstance()` и `issubclass()`, чтобы проверить, реализует ли 
экземпляр или класс коннкретный ABC. Модуль `collections.abc` определяет набор 
полезных ABC, таких как `Iterable`, `Container` и `MutableMapping`.

В *Python* многие преимущества спецификаций интерфейса могут быть получены при 
соблюдении дисциплины тестирования компонентов.

Хороший набор тестов для модуля может обеспечивать как регрессионный тест, 
так и спецификация интерфейса модуля и набор примеров. Многие модули *Python* 
можно запускать как сценарии для простой «самопроверки». Даже модули, 
использующие сложные внешние интерфейсы, часто можно тестировать изолированно, 
используя тривиальные «заглушки» для эмуляции внешнего интерфейса. Модули 
`doctest` и `unittest` или сторонние среды тестирования можно использовать для 
создания исчерпывающих наборов тестов, которые проверяют каждую строку кода 
в модуле.

Соблюдение дисциплины тестирования, а также наличие спецификаций интерфейса, 
может помочь в создании больших сложных приложений на *Python*. На самом деле 
использование тестов лучше, потому что спецификация интерфейса не может 
тестировать определенные свойства программы. Например, ожидается, что метод 
`append()` будет добавлять новые элементы в конец некоторого внутреннего 
списка. Спецификация интерфейса не может проверить, что ваша реализация 
`append()` действительно сделает это правильно, но проверить это свойство 
в наборе тестов несложно.

Написание наборов тестов очень полезно, и вы можете спроектировать свой код 
так, чтобы его было легко тестировать. Один из все более популярных методов, 
разработка через тестирование (TDD, от Test Driven Development), требует 
сначала написания набора тестов, а потом написание самого кода. Конечно, 
*Python* позволяет вам быть неряшливым и вообще не писать тестовые примеры.

[К содержанию](#содержание)


## Почему в *Python* нет оператора `goto`?

В 1970-х люди поняли, что неограниченное использование оператора `goto` 
может привести к «спагетти-коду», который будет трудно понять и изменить. 
В высокоуровневых языках в нем также нет необходимости, если есть способы 
ветвления (в *Python*  -- операторы `if` и `or`, `and` и `if-else`) и цикла 
(операторы `while` и `for`,  иногда содержащими `continue` и `break`).

Для предоставления «структурированного перехода», можно использовать
исключениия, которые работают даже между вызовами функций. Многие считают, что 
исключения могут удобно эмулировать все разумные варианты использования 
конструкций `go` или `goto` языков *C*, *Fortran* и других. Например:

```python
class label(Exception):
    pass  # declare a label

try:
    ...
    if condition: raise label()  # goto label
    ...
except label:  # where to goto
    pass
...
```

Это не позволяет вам прыгнуть в середину цикла, но обычно считается 
злоупотреблением использования `goto`. Используйте экономно.

[К содержанию](#содержание)


## Почему сырые строки (r-строки) не могут заканчиваться обратной косой чертой?

Говоря точнее, они не могут заканчиваться нечетным количеством обратных косых 
черт: непарная обратная косая черта в конце экранирует символ закрывающей 
кавычки, оставляя незавершенную строку.

Сырые строки были разработаны, чтобы облегчить создание входных данных для 
процессоров (главным образом, механизмов регулярных выражений), которые хотят 
выполнять свою собственную обработку экранирования обратной косой черты. Такие 
процессоры в любом случае считают непарную обратную косую черту ошибкой, 
поэтому сырые строки не допускают этого. В свою очередь, они позволяют вам 
передавать символ строковой кавычки, экранируя его обратной косой чертой. Эти 
правила хорошо работают, когда r-строки используются по назначению.

Если вы пытаетесь создать путь к файлу для *Windows*, обратите внимание, что 
системные вызовы также принимают косую черту:

```python
f = open("/mydir/file.txt")  # works fine!
```

Если вы пытаетесь создать путь для *DOS*, попробуйте, например, один 
из следующих вариантов:

```python
dir = r"\this\is\my\dos\dir" "\\"
dir = r"\this\is\my\dos\dir\ "[:-1]
dir = "\\this\\is\\my\\dos\\dir\\"
```

[К содержанию](#содержание)


## Почему в *Python* нет оператора `with` для присвоения атрибутов?

В Python есть оператор `with`, который оборачивает выполнение блока, вызывая 
код при входе и автоматически выходя из блока. Некоторые языки имеют 
конструкцию, которая выглядит следующим образом:

```python
with obj:
    a = 1               # equivalent to obj.a = 1
    total = total + 1   # obj.total = obj.total + 1
```

В *Python* такая конструкция была бы неоднозначной.

Другие языки, такие как *Object Pascal*, *Delphi* и *C++*, используют 
статическую типизацию, так что можно однозначно узнать, какой элемент 
присваивается. Это основной момент статической типизации -- компилятор всегда 
знает область видимости каждой переменной во время компиляции.

*Python* использует динамическую типизацию. Невозможно заранее знать, на какой 
атрибут будет ссылка во время выполнения программы. Атрибуты объектов могут 
быть добавлены или удалены из объектов на лету. Во время простого чтения 
нельзя узнать, на какой атрибут ссылается программа: на локальный, глобальный 
или атрибут объекта?

К примеру, рассмотрим незавершенный фрагмент кода:

```python
def foo(a):
    with a:
        print(x)
```

Фрагмент предполагает, что объект «a» должен иметь атрибут «x». Однако 
в *Python* нет ничего, что сообщало бы об этом интерпретатору. Что произойдет, 
если «а» — это, скажем, целое число? Если есть глобальная переменная 
с именем «x», будет ли она использоваться внутри блока `with`? Как видите, 
динамическая природа *Python* значительно усложняет выбор.

Однако основное преимущество оператора `with` и подобных -- уменьшение 
объема кода -- может быть легко достигнуто в *Python* с помощью присваивания. 
Вместо:

```python
function(args).mydict[index][index].a = 21
function(args).mydict[index][index].b = 42
function(args).mydict[index][index].c = 63
```

пишите:

```python
ref = function(args).mydict[index][index]
ref.a = 21
ref.b = 42
ref.c = 63
```

В этом случае также достигается побочный эффект в виде увеличения скорости 
выполнения, потому что привязка имен происходит прямо во время выполнения кода 
*Python*, во втором случае привязка происходит только один раз.

[К содержанию](#содержание)


## Почему генераторы не поддерживают оператор `with`?


По техническим причинам генератор, используемый в качестве менеджера 
контекста, не будет работать корректно. Когда, как это чаще всего бывает, 
в качестве итератора, работающего до завершения, используется генератор, 
закрытие не требуется. Когда это так, оберните его как 
`contextlib.closing (generator)` в операторе `with`.

[К содержанию](#содержание)


## Почему для операторов `if`/ `while`/ `def`/ `class` требуются двоеточия?

Двоеточие требуется в первую очередь для повышения читабельности (один из 
результатов экспериментального языка *ABC*). Сравни:

```python
if a == b
    print(a)
```

и

```python
if a == b:
    print(a)
```

Заметь, что второй текст читается немного легче. Обратите внимание, как 
двоеточие выделяет пример в этом ответе на часто задаваемые вопросы; это 
стандартное употребление в английском языке.

Другая второстепенная причина заключается в том, что двоеточие упрощает работу 
редакторов с подсветкой синтаксиса; они могут искать двоеточия, чтобы решить, 
когда нужно увеличить отступ, вместо того, чтобы выполнять более сложный 
анализ текста программы.

[К содержанию](#содержание)


## Почему *Python* допускает запятые в конце списков и кортежей?


*Python* позволяет добавлять запятую в конце списков, кортежей и словарей:

```python
[1, 2, 3,]
('a', 'b', 'c',)
d = {
    "A": [1, 5],
    "B": [6, 7],  # last trailing comma is optional but good style
}
```

Есть несколько причин, позволяющих это сделать.

Когда у вас есть значение списка, кортежа или словаря, распределенное 
по нескольким строкам, проще добавить дополнительные элементы, потому что вам 
не нужно помнить о добавлении запятой в предыдущую строку. Также строки могут 
быть переупорядочены без создания синтаксической ошибки.

Случайный пропуск запятой может привести к ошибкам, которые трудно 
диагностировать. Например:

```python
x = [
  "fee",
  "fie"
  "foo",
  "fum"
]
```

Этот список выглядит так, будто состоит из четырех элементов, но на самом деле 
их три: «fee», «fiefoo» и «fum». Всегда добавляя запятую, вы избегаете этого 
источника ошибки.

Разрешение завершающей запятой также может упростить программную генерацию 
кода.

[К содержанию](#содержание)
