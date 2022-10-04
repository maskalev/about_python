## Частые вопросы по дизайну и истории *Python* (перевод 
[статьи](https://docs.python.org/3/faq/design.html))

В ответах, если не указано иное, подразумевается стандартная реализация языка *Python* -- *CPython*

### Содержание

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

[Как в *CPython* реализованы словари?](#как-в-CPython-реализованы-словари)

Why must dictionary keys be immutable?

Why doesn’t list.sort() return the sorted list?

How do you specify and enforce an interface spec in Python?

Why is there no goto?

Why can’t raw strings (r-strings) end with a backslash?

Why doesn’t Python have a “with” statement for attribute assignments?

Why don’t generators support the with statement?

Why are colons required for the if/while/def/class statements?

Why does Python allow commas at the end of lists and tuples?


### Почему в *Python* для группировки операторов используются отступы?

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


### Почему при выполнении простых арифметических операций я получаю странные 
результаты?

Смотри следующий вопрос.


### Почему вычисления чисел с плавающей запятой такие неточные?

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
  операциях с числами с плавающей запятой, Питон ведет себя так же как другие
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


### Почему в *Python* строки неизменяемы?

У этого решения есть несколько преимуществ.

Первое -- производительность. Знание того, что строка неизменяема позволяет 
сразу выделить необходимое количество памяти во время создания объекта, 
в дальнейшем оно не изменится. Это так же является одним из различий между
 списками и кортежами.

Второе -- строки в *Python* являются такими же "элементарными", как числа. 
Ничто не может изменить значение числа 8, также ничто не может изменить 
значение строки "восемь".


### Почему при определении методов и их вызове нужно явно использовать `self`?

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


### Почему я не могу использовать присваивание прямо в выражении?

Начиная с *Python 3.8* можешь!

Моржовый оператор `:=` позволяет присваивать значение прямо в выражении:

```python
while chunk := fp.read(200):
   print(chunk)
```


### Когда *Python* использует функции, а когда методы?

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


### Почему `join()` является методом строки, а не списка или кортежа?

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


### Насколько дорога обработка исключений?

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


### Почему в *Python* нет операторов `switch` или `case`?

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


### Разве вы не можете эмулировать потоки вместо того, чтоб полагаться на 
специфическую для каждой ОС реализацию потока?

1. К сожалению, интерпретатор помещает по крайней мере один фрейм стека *C* 
для каждого фрейма стека *Python*. Кроме того, расширения могут вызывать 
*Python* почти в случайные моменты. Поэтому для полной реализации потоков 
требуется поддержка потоков для *C*.

2. К счастью, существует Stackless Python с полностью переработанным циклом 
интерпретатора, который позволяет избежать стека *C*.


### Почему анонимные функции не могут содержать операторов?

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


### Можно ли скопмилировать *Python* в машинный код, *С* или другой язык?

*Cython* компилирует модифицированную версию *Python* с необязательными 
аннотациями в расширения *C*. *Nuitka* -- многообещающий компилятор *Python* 
в код *C++*, целью которого является поддержка всего языка *Python*.


### Как *Python* управляет памятью?

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


### Почему *CPython* не использует более традиционную схему сбора мусора?

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


### Почему при выходе из *CPython* освобождается не вся память?

При выходе из *Python* освобождаются не все объекты, на которые ссылаются 
глобальные пространства имен. Это происходит при наличии циклических ссылок. 
Также существуют определенные биты памяти, выделенные библиотекой *C*, которые 
невозможно освободить (например, такой инструмент, как *Purify*, будет 
жаловаться на это). Однако, *Python* агрессивно относится к очистке памяти при 
выходе и пытается уничтожить каждый отдельный объект.

Если вы хотите, чтобы *Python* удалял определенные вещи при освобождении, 
используйте модуль `atexit`.


### В чем разница между списками и кортежами?

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


### Как в *CPython* реализованы списки?

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


### Как в *CPython* реализованы словари?

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