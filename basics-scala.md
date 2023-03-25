# Основы

+ [Выражения](#Выражения)
+ [Значения](#Значения)
+ [Переменные](#Переменные)
+ [Блоки](#Блоки)
+ [Функции](#Функции)
+ [Методы](#Методы)
+ [Классы](#Классы)
+ [Классы-образцы (Case Class)](#Классы-образцы_(Case_Class))
+ [Объекты](#Объекты)
+ [Трейты](#Трейты)
+ [Главный метод](#Главный_метод)
+ [Единообразие типов](#Единообразие_типов)
+ [Приведение типа](#Приведение_типа)
+ [Nothing и Null](#Nothing_и_Null)

## Выражения

__Выражения__ - вычисляемые утверждения (напр., `1+1`).

Вывести выражения можно используя `println`.

```scala
println(1 + 1) // 2
println("Hello") // "Hello"
```

[К оглавлению](#Основы)

## Значения

Результат выражения можно присвоить имени с ключевым словом `val`.

```scala
val x = 1 + 1
print(x) // 2
```

`x` в примере - значение. Вызов значения не приводит к повторному вычислению. Значения __неизменяемы__ и не могут быть __переназначены__.

```scala
x = 3 // не компилируется
```

Типы значений могут выводиться автоматически, но можно указать и явно. Объявление типа происходит после `имени значения` и `:`

```scala
val x: Int = 1 + 1
```

[К оглавлению](#Основы)

## Переменные

Переменные похожи нв значения, но их можно присваивать заново. Объявляются с помощью ключевого слова `var`.

```scala
var x = 1 + 1
x = 3
println(x * x) // 9
```

Как и в значении можно явно указать тип.

```scala
var x: Int = 1 + 1
```

[К оглавлению](#Основы)

## Блоки

Можно комбинировать блоки, окружая их `{}` - __блок__. Результат последнего выражения в блоке - результат всего блока.

```scala
println({
  val x = 1 + 1
  x + 1
}) // 3
```

[К оглавлению](#Основы)

## Функции

__Функция__ - выражение, которое принимает параметры.

Напр., можно определить анонимную функцию (без имени), которая возвращает переданное число, прибавив единицу:

```scala
(x: Int) => x + 1
```

Слева от `=>` находится список параметров, справа - выражение, связанное с параметрами. Так же функциям можно давать имена:

```scala
val addOne = (x: Int) => x + 1
print(addOne(1)) // 2
```

Функция может принимать множество параметров:

```scala
val add = (x: Int, y: Int) => x + y
print(add(1, 2)) // 3
```

Функция может не принимать параметров:

```scala
val getTheAnswer = () => 42
println(getTheAnswer()) // 42
```

[К оглавлению](#Основы)

## Методы

Методы похожи на функции, но есть несколько различий.

Метод задается ключевым словом `def`, после чего следует имя, список параметров, возвращаемый тип и тело. Возвращаемый тип объявляется после `списка параметров` и `:`.

```scala
def add(x: Int, y: Int): Int = x + y
println(add(1, 2)) // 3
```

Методы могут принимать несколько списков параметров.

```scala
def addThenMultiply(x: Int, y: Int)(multiplier: Int) = (x + y) * multiplier
println(addThenMultiply(1, 2)(3)) // 9
```

Могут не принимать параметры вообще.

```scala
def name: String = System.getProperty("user.name")
println("Hello, " + name)
```

Методы могут иметь многострочные выражения. Последнее выражение становится возвращаемым значением метода.

```scala
def getSquareString(input: Double): String = {
  val square = input * input
  square.toString
}
println(getSquareString(2.5)) // 6.25
```

[К оглавлению](#Основы)

## Классы

Класс объявляется ключевым словом `class`, за которым следуют его имя и параметры конструктора. Классы могут содержать методы, константы, переменные, типы, объекты, трейты и классы (члены).

```scala
class Greeter(prefix: String, suffix: String) {
  def greet(name: String): Unit =
    println(prefix + name + suffix)
}
```

Возвращаемый тип метода `greet` - `Unit` используется, когда не имеет смысла что-либо возвращать (аналогичен `void`). Т.к. каждое выражение должно иметь какое-то значение, то при отсутствии возвращающегося значения возвращается экземпляр `Unit`. Явным образом можно задать `()` (не несет какой-либо информации).

Можно создать экземпляр класса используя `new`.

```scala
val greeter = new Greeter("Hello, ", "!")
greeter.greet("Scala developer") // Hello, Scala developer!
```

Обычно используется конструктор и тело класса.

```scala
class Point(var x: Int, var y: Int) {
  def move(dx: Int, dy: Int): Unit = {
    x = x + dx
    y = y + dy
  }

  override def toString: String =
    s"($x, $y)"
}

val point1 = new Point(2, 3)
point1.x // 2
println(point1) // prints (2, 3)
```

В примере выше у класса `Point` есть четыре члена: переменные: `x` и `y`, методы: `move` и `toString`. Основной конструктор находится в сигнатуре класса `(var x: Int, var y: Int)`. Метод `move` принимает два целочисленных аргумента и возвращает `Unit` - пустое множество без информации. `toString` не принимает аргументы, возвращает значение `String`. Т.к. `toString` переопределяет `toString` из `AnyRef`, он помечен ключевым словом `override`.

Конструктор может иметь необязательный параметр, если указать значение по умолчанию. Т.к. конструктор считывает аргументы слева направо, если нужно указать значение `y`, нужно указать задаваемый параметр.

```scala
class Point(var x: Int = 0, var y: Int = 0)

val origin = new Point() // x и y равны 0
val point1 = new Point(1)
println(point1.x) // 1
val point2 = new Point(y = 2)
println(point2.y) // 2
```

__Скрытые члены и синтаксис Геттер/Сеттер__

По умолчанию члены класса `public`. Чтобы скрыть, нужен модификатор `private`

```scala
class Point {
  private var _x = 0
  private var _y = 0
  private val bound = 100

  def x = _x

  def x_=(newValue: Int): Unit = {
    if (newValue < bound) _x = newValue else printWarning
  }

  def y = _y

  def y_=(newValue: Int): Unit = {
    if (newValue < bound) _y = newValue else printWarning
  }

  private def printWarning = println("WARNING")
}

val point1 = new Point
point1.x = 99
point1.y = 101 // WARNING
```

Здесь данные хранятся в скрытых переменных `_x` и `_y`. Методы `def x` и `def y` - методы для доступа к скрытым данным (геттеры). `def x_` и `def y_` - для проверки и установки значений `_x` и `_y` (сеттеры). 

[К оглавлению](#Основы)

## Классы-образцы (Case Class)

__Класс-образец__ - по умолчанию такие классы неизменны и сравниваются по значению из конструктора. Объявляются ключевыми словами `case class`. Ключевое слово `new` необязательно для создания экземпляра (т.к. по умолчанию есть объект компаньон с методом `apply` - создает экземпляр класса).

```scala
case class Point(x: Int, y: Int)

val point1 = Point(1, 2)

println(point1.x) // 1
point1.x = 2 // не компилируется
```

При создании класс-образца с параметрами - параметры публичные и неизменяемые.

Классы-образцы сравниваются по структуре, а не по ссылкам.

```scala
case class Message(sender: String, body: String)

val message1 = Message("Jack", "Hello")
val message2 = Message("Jack", "Hello")
val messagesAreTheSame = message1 == message2 // true
```

Можно создать копию класса-образца методом `copy`. По желанию можно изменить аргументы конструктора.

```scala
case class Message(sender: String, body: String)

val message1 = Message("Jack", "Hello")
val message2 = message1.copy(sender = "Piter")
message2.sender // Piter
message2.body // Hello
```

[К оглавлению](#Основы)

## Объекты

Объект - является значением, задается и существует в единственном экземпляре (Singleton Object). Объект задается при помощи ключевого слова `object`. Создается лениво, когда на него ссылаются (lazy val).

```scala
package logging

object Logger {
  def info(message: String): Unit = println(s"INFO: $message")
}
```

Метод `info` может быть импортирован в любом месте программы. Вызов `info` в другом пакете:

```scala
import logging.Logger.info

class Project(name: String)

class Test {
  val project1 = new Project("One")
  val project2 = new Project("Two")
  info("Created projects") // INFO: Created projects
}
```

Если `object` вложен в другой класс или объект, то он "зависим от пути".

[К оглавлению](#Основы)

## Трейты

__Трейты (Traits)__ - используются для обмена между классами информацией о структуре и полях. Похожи на интерфейсы (Java). Классы и объекты могут расширять трейты, но трейты не могут быть созданы и поэтому не имеют параметров.

Трейт объявляется при помощи ключевого слова `trait`.

```scala
trait HairColor
```

Трейты полезны в качестве обобщенного типа с абстрактными методами.

```scala
trait Iterator[A] {
  def hasNext: Boolean
  def next(): A
}
```

При наследовании от `Iterator[A]` требуется указать тип `A` и реализовать методы `hasNext` и `next`.

Чтобы использовать трейт, нужно наследовать класс от него, используя ключевое слово `extends`, затем реализовать все абстрактные члены трейты, используя ключевое слово `override`.

```scala
trait Iterator[A] {
  def hasNext: Boolean
  def next(): A
}

class IntIterator(to: Int) extends Iterator[Int] {
  private var current = 0
  override def hasNext: Boolean = current < to
  override def next(): Int = {
    if (hasNext) {
      val t = current
      current += 1
      t
    } else 0
  }
}

val iterator = new IntIterator(10)
iterator.next() // 0
iterator.next() // 1
```

Туда, где требуется определенный тип трейта, можно передать любой наследованный от требуемого трейта класс.

```scala
import scala.collection.mutable.ArrayBuffer

trait Pet {
  val name: String
}

class Cat(val name: String) extends Pet
class Dog(val name: String) extends Pet

val dog = new Dog("Harry")
val cat = new Cat("Sally")

val animals = ArrayBuffer.empty[Pet]
animals.append(dog)
animals.append(cat)
animals.foreach(pet => println(pet.name)) // Harry и Sally
```

У трейта `Pet` есть абстрактное поле `name`, которое реализовано в классах `Cat` и `Dog`.

[К оглавлению](#Основы)

## Главный метод

__Главный метод__ - отправная точка в программе. Для JVM требуется, чтобы он назывался `main` и принимал один аргумент, массив строк.

```scala
object Main {
  def main(args: Array[String]): Unit = {
    println("Hello, Scala developer")
  }
}
```

[К оглавлению](#Основы)

## Единообразие типов

В Scala все значения имеют тип, включая числовые значения и функции.

![img.png](img.png)

+ `Any` - супертип всех типов (верхний тип). Определяет универсальные методы: `equals`, `hashCode`, `toString`. Есть два прямых подкласса: `AnyVal` и `AnyRef`.
+ `AnyVal` - представляет числовые типы. Есть девять предварительно определенных числовых типов (не могут быть `null`): `Double`, `Float`, `Long`, `Int`, `Short`, `Byte`, `Char`, `Unit`, `Boolean`.
  + `Unit` - числовой тип, который не содержит значимой информации (пустое множество).
+ `AnyRef` - представляет ссылочные типы (все типы, не относящиеся к числовым). Каждый тип, объявляемый пользователем - подтип `AnyRef` (аналог `java.lang.Object`).

```scala
val list: List[Any] = List(
  "a string",
  732,
  'c',
  true,
  () => "анонимная функция возвращающая строку"
)

list.foreach(element => println(element))
```
Ответ:
```
a string 
732
c
true
<function>
```

Переменная `list` типа `List[Any]`. Список инициализируется элементами различных типов (они все экземпляры `scala.Any`).

[К оглавлению](#Основы)

## Приведение типа

Числовые типы приводятся следующим образом:

![img_1.png](img_1.png)

```scala
val x: Long = 987654321
val y: Float = x.toFloat // 9.8765434E8 (точность теряется)
val face: Char = '☺'
val number: Int = face // 9786
```

Приведение типа - однонаправленно.

```scala
val x: Long = 987654321
val y: Float = x.toFloat // 9.8765434E8
val z: Long = y // не компилируется
```

[К оглавлению](#Основы)

## Nothing и Null

`Nothing` - подтип всех типов (нижний тип). Нет значения, которое имеет тип `Nothing`. Используется для сигнала о не вычислимости (брошено исключение, выход из программы, бесконечное зацикливание), т.е. для выражений, которые не вычисляются.

`Null` - подтип всех ссылочных типов (подтип `AnyRef`). Имеет одно значение, определяемое ключевым словом литерала `null`. `Null` предоставляется для функциональной совместимости с другими языками JVM (почти никогда не используется в коде Scala).

[К оглавлению](#Основы)

[Learning Scala](README.md#scala)