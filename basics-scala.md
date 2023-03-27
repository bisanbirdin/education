[Learning Scala](README.md#scala)

# Основы

+ [Выражения](#Выражения)
+ [Блоки](#Блоки)
+ [Функции](#Функции)
+ [Методы](#Методы)
+ [Классы](#Классы)
+ [Классы образцы (Case Class)](#Классы-образцы-case-class)
+ [Объекты](#Объекты)
+ [Трейты](#Трейты)
+ [Главный метод](#Главный-метод)
+ [Единообразие типов](#Единообразие-типов)
+ [Параметры по умолчанию](#Параметры-по-умолчанию)
+ [Кортежи](#Кортежи)
+ [Композиция классов с примесями](#Композиция-классов-с-примесями)
+ [Функции высшего порядка](#ункции-высшего-порядка)
+ [Множественные списки параметров (каррирование)](#Множественные-списки-параметров-каррирование)
+ [Сопоставление с примером](#Сопоставление-с-примером)
+ [Регулярные выражения](#Регулярные-выражения)
+ [Объект экстрактор](#Объект-экстрактор)
+ [Сложные for выражения](#Сложные-for-выражения)

## Выражения

__Выражения__ - вычисляемые утверждения (напр., `1+1`).

Вывести выражения можно используя `println`.

```scala
println(1 + 1) // 2
println("Hello") // "Hello"
```

_ЗНАЧЕНИЯ_

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

_ПЕРЕМЕННЫЕ_

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

_ВЛОЖЕННЫЕ МЕТОДЫ_

В Scala можно вложить метод в тело другого метода.

```scala
def factorial(x: Int): Int = {
  def fact(x: Int, accumulator: Int): Int = {
    if (x <= 1) accumulator
    else fact(x - 1, x * accumulator)
  }
  fact(x, 1)
}
println("Factorial of 2: " + factorial(2)) // 2
println("Factorial of 3: " + factorial(3)) // 6
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

_КОНСТРУКТОРЫ_

Конструктор может иметь необязательный параметр, если указать значение по умолчанию. Т.к. конструктор считывает аргументы слева направо, если нужно указать значение `y`, нужно указать задаваемый параметр.

```scala
class Point(var x: Int = 0, var y: Int = 0)

val origin = new Point() // x и y равны 0
val point1 = new Point(1)
println(point1.x) // 1
val point2 = new Point(y = 2)
println(point2.y) // 2
```

_СКРЫТЫЕ ЧЛЕНЫ И СИНТАКСИС Геттер/Сеттер_

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

## Классы образцы (Case Class)

__Класс-образец__ - по умолчанию такие классы неизменны и сравниваются по значению из конструктора. Объявляются ключевыми словами `case class`. Ключевое слово `new` необязательно для создания экземпляра (т.к. по умолчанию есть объект компаньон с методом `apply` - создает экземпляр класса).

```scala
case class Point(x: Int, y: Int)

val point1 = Point(1, 2)

println(point1.x) // 1
point1.x = 2 // не компилируется
```

При создании класс-образца с параметрами - параметры публичные и неизменяемые.

_СРАВНЕНИЕ_

Классы-образцы сравниваются по структуре, а не по ссылкам.

```scala
case class Message(sender: String, body: String)

val message1 = Message("Jack", "Hello")
val message2 = Message("Jack", "Hello")
val messagesAreTheSame = message1 == message2 // true
```

_КОПИРОВАНИЕ_

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

_ОБЪЕКТЫ КОМПАНЬОНЫ_

Объект с именем как у класса - __объект компаньон (companion object)__, а класс - __класс-компаньон объекта__. Такие объекты и классы могут получить доступ к приватным членам своего компаньона.

```scala
import scala.math._

case class Circle(radius: Double) {
  import Circle._
  def area: Double = calculateArea(radius)
}

object Circle {
  private def calculateArea(radius: Double): Double = Pi * pow(radius, 2.0)
}

val circle1 = Circle(5.0)

circle1.area
```

Класс `Circle` имеет член `area`, который специфичен для конкретного экземпляра, а метод `calculateArea` объекта `Circle`, доступен каждому экземпляру класса `Circle`.

Объект компаньон может содержать методы создающие конкретные экземпляры класса:

```scala
class Email(val username: String, val domainName: String)

object Email {
  def fromString(emailString: String): Option[Email] = {
    emailString.split('@') match {
      case Array(a, b) => Some(new Email(a, b))
      case _ => None
    }
  }
}

val scalaCenterEmail = Email.fromString("scala.center@epfl.ch")
scalaCenterEmail match {
  case Some(email) => println(
    s"""Registered an email
       |Username: ${email.username}
       |Domain name: ${email.domainName}
     """)
  case None => println("Error: could not parse email")
}
```

`object Email` содержит производящий метод `fromString`, который создает экземпляр `Email` из строки. Возвращается `Option[Email]` на случай возникномения ошибок парсинга.

_ПРИМЕЧАНИЕ ДЛЯ Java-ПРОГРАММИСТОВ_

`static` члены в Java смоделированы как обычные члены объекта компаньона в Scala. При использовании объекта компаньона из Java кода, члены будут определены в классе компаньоне с модификатором `static`. Это называется _пробрасывание статикки_. 

[К оглавлению](#Основы)

## Трейты

__Трейты (Traits)__ - используются для обмена между классами информацией о структуре и полях. Похожи на интерфейсы (Java). Классы и объекты могут расширять трейты, но трейты не могут быть созданы и поэтому не имеют параметров.

_ОБЪЯВЛЕНИЕ ТРЕЙТА_

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

_ИСПОЛЬЗОВАНИЕ ТРЕЙТОВ_

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

_ПОДТИПЫ_

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

![1.png](https://github.com/bisanbirdin/education/blob/157cd1742932f9c026c703abe51780e591ed83c3/pictures/1.png)

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

_ПРИВЕДЕНИЕ ТИПА_

Числовые типы приводятся следующим образом:

![2.png](https://github.com/bisanbirdin/education/blob/157cd1742932f9c026c703abe51780e591ed83c3/pictures/2.png)

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

_Nothing и Null_

`Nothing` - подтип всех типов (нижний тип). Нет значения, которое имеет тип `Nothing`. Используется для сигнала о не вычислимости (брошено исключение, выход из программы, бесконечное зацикливание), т.е. для выражений, которые не вычисляются.

`Null` - подтип всех ссылочных типов (подтип `AnyRef`). Имеет одно значение, определяемое ключевым словом литерала `null`. `Null` предоставляется для функциональной совместимости с другими языками JVM (почти никогда не используется в коде Scala).

[К оглавлению](#Основы)

## Параметры по умолчанию

В Scala можно задать значения параметров по умолчанию (не указывать параметры лишний раз).

```scala
def log(message: String, level: String = "INFO") = println(s"$level: $message")

log("System starting") // "INFO: System starting"
log("Usern not found", "WARNING") // "WARNING: User not found
```

У параметра `level` есть значение по умолчанию, поэтому он необязателен. Аргумент `WARNING` переназначает аргумент по умолчанию. Если при вызове пропущен хотя бы один аргумент, остальные нужно вызывать по имени конкретного аргумента.

```scala
class Point(val x: Double = 0, val y: Double = 0)

val point1 = new Point(y = 1)
```

Параметры по умолчанию в Scala, при вызове из Java кода - обязательны.

```scala
// Point.scala
class Point(val x: Double = 0, val y: Double = 0)
```

```java
// Main.java
public class Main {
  public static void main(String[] args) {
    Point point = new Point(1); // не скомпилируется
  }
}
```

[К оглавлению](#Основы)

## Кортежи

__Кортеж (Tuple)__ - класс контейнер содержащий упорядоченный набор элементов различного типа (неизменяем). Полезны, когда нужно вернуть сразу несколько значений из функции. Создание кортежа:

```scala
val ingredient = ("Sugar", 25): Tuple2[String, Int]
```

Данная запись создает кортеж размерности 2. Кортежи представлены серией классов от Tuple1 до Tuple22.

_ДОСТУП К ЭЛЕМЕНТАМ_

Доступ к элементам осуществляется синтаксисом подчеркивания `tuple._n`.

```scala
println(ingredient._1) // Sugar
println(ingredient._2) // 25
```

_РАСПАКОВКА ДАННЫХ КОРТЕЖА_

Кортежи поддерживают распаковку.

```scala
val (name, quantity) = ingredient
println(name) // Sugar
println(quantity) // 25
```

[К оглавлению](#Основы)

## Композиция классов с примесями

__Примеси (Mixin)__ - трейты, которые используются для создания класса.

```scala
abstract class A {
  val message: String
}
class B extends A {
  val message = "I'm an instance of class B"
}
trait C extends A {
  def loudMessage = message.toUpperCase()
}
class D extends B with C

val d = new D
println(d.message) // I'm an instance of class B
println(d.loudMessage) // I'M AN INSTANCE OF CLASS B
```

У класса `D` есть суперкласс `B` и примесь `C`. У класса может быть 1 суперкласс и много примесей.

Пример:

```scala
abstract class AbsIterator {
  type T
  def hasNext: Boolean
  def next(): T
}
```

Класс имеет абстрактный тип `T` и методы итератора. Создадим реализацию класса:

```scala
class StringIterator(s: String) extends AbsIterator {
  type T = Char
  private var i = 0
  def hasNext = i < s.length
  def next() = {
    val ch = s charAt (i)
    i += 1
    ch
  }
}
```

Создадим трейт, который тоже наследуется от AbsIterator.

```scala
trait RichIterator extends AbsIterator {
  def foreach(f: T => Unit): Unit = while (hasNext) f(next())
}
```

У трейта реализован метод `foreach`, который вызывает переданную функцию `f: T => Unit` на каждом элементе `next()` пока в итераторе есть элементы. Т.к. `RichIterator` трейт - ему не нужно реализовывать абстрактные члены.

Объединение функциональности `StringIterator` и `RichIterator` в один класс:

```scala
object StringIteratorTest extends App {
  class RichStringIter extends StringIterator("Scala") with RichIterator
  val richStringIter = new RichStringIter
  richStringIter foreach println
}
```

[К оглавлению](#Основы)

## Функции высшего порядка

__Функции высшего порядка (фвп)__ - могут принимать другие функции в виде параметров или возвращать в качестве результата функцию. Это возможно, т.к. функция - объект первого класса в Scala. Здесь "функция высшего порядка" используется для методов и функций, описанных выше.

Пример фвп - `map` из коллекции Scala.

```scala
val salaries = Seq(20000, 70000, 40000)
val doubleSalary = (x: Int) => x * 2
val newSalaries = salaries.map(doubleSalary) // List(40000, 140000, 80000)
```

`doubleSalary` - функция, которая принимает параметр `x` типа `Int` и возвращает `x * 2`. Кортеж (список имен в скобках) слева от стрелки `=>` - список параметров, справа - значение выражения. Для сокращения, можно сделать функцию анонимной и передать напрямую в `map`.

```scala
val salaries = Seq(20000, 70000, 40000)
val newSalaries = salaries.map(x => x * 2) // List(40000, 140000, 80000)
```
или
```scala
val salaries = Seq(20000, 70000, 40000)
val newSalaries = salaries.map(_ * 2)
```

_ПРЕОБРАЗОВАНИЕ МЕТОДОВ В ФУНКЦИИ_

Возможно передача методов в качестве аргументов функциям более высокого порядка, т.к. компилятор Scala может преобразовать метод в функцию.

```scala
case class WeeklyWeatherForecast(temperatures: Seq[Double]) {
  private def convertCtoF(temp: Double) = temp * 1.8 + 32
  def forecastInFahrenheit: Seq[Double] = temperatures.map(convertCtoF) // передача метода convertCtoF
}
```

Метод `convertCtoF` передается в `forecastInFahrenheit`, т.к. компилятор преобразовывает `convertCtoF` в функцию `x => ConvertCtoF(x)` (`x` - сгенерированное имя, уникальное в своей области видимости).

_ФУНКЦИИ, КОТОРЫЕ ПРИНИМАЮТ ФУНКЦИИ_

Причина использования фвп - сокращение избыточного (повторяющегося) кода. 

Пример: требуются методы для повышения зарплат исходя из разных условий. Без фвп:

```scala
object Salary {
  def smallPromotion(salaries: List[Double]): List[Double] =
    salaries.map(salary => salary * 1.1)
  def greatPromotion(salaries: List[Double]): List[Double] =
    salaries.map(salary => salary * math.log(salary))
  def hugePromotion(salaries: List[Double]): List[Double] =
    salaries.map(salary => salary * salary)
}
```

Методы отличаются только коэффициентом умножения. Можно вынести повторяющийся код в фвп:

```scala
object Salary {
  private def promotion(salaries: List[Double], promotionFunction: Double => Double): List[Double] =
    salaries.map(promotionFunction)
  def smallPromotion(salaries: List[Double]): List[Double] =
    promotion(salaries, salary => salary * 1.1)
  def greatPromotion(salaries: List[Double]): List[Double] =
    promotion(salaries, salary => salary * math.log(salary))
  def hygePromotion(salaries: List[Double]): List[Double] =
    promotion(salaries, salary => salary * salary)
  
}
```

Метод `promotion` берет зарплату и функцию типа `Double => Double` (берет и возвращает Double) и возвращает их произведение.

_ФУНКЦИИ, ВОЗВРАЩАЮЩИЕ ФУНКЦИИ_

```scala
def URLBuilder(ssl: Boolean, domainName: String): (String, String) => String = {
  val schema = if (ssl) "https://" else "http://"
  (endpoint: String, query: String) => s"$schema$domainName/$endpoint?$query"
}

val domainName = "www.example.com"
def getURL = urlBuilder(ssl = true, domainName)
val endpoint = "users"
val query = "id=1"
val url = getURL(endpoint, query) // "https://www.example.com/users?id=1": String
```

Возвращаемый тип `uriBuilder` `(String, String) = > String` - это значит, что возвращаемая анонимная функция принимает две строки и возвращает одну.

[К оглавлению](#Основы)

## Множественные списки параметров (каррирование)

Методы могут объявляться с несколькими списками параметров. Если метод вызывается с меньшим количеством списка параметров, это приводит к созданию новой функции (частичное применение).

```scala
val numbers = List(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
val numberFunc = numbers.foldLeft(List[Int]()) _

val squares = numberFunc((xs, x) => xs :+ x * x)
print(squares) // List(1,4,9,16,25,36,49,64,81,100)

val cubes = numberFunc((xs, x) => xs :+ x * x * x)
print(cubes) // List(1,8,27,64,125,216,343,512,729,1000)
```

`foldLeft` применяет бинарный оператор `op` к начальному значению `z` и ко всем остальным элементам коллекции слева направо.

```scala
def foldLeft[B](z: B)(op: (B, A) = B): B
```

Пример. Начиная с начального значения 0, `foldLeft` применяет функцию `(m, n) => m + n` к каждому элементу списка и предыдущему накопленному значению:

```scala
val numbers = List(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
val res = numbers.foldLeft(0)((m, n) => m + n)
print(res) // 55
```

_ОТДЕЛЬНЫЙ ФУНКЦИОНАЛЬНЫЙ ПАРАМЕТР_

Функцию `op` можно выделить в отдельный функциональный параметр `foldLeft`. Это позволяет использовать автоматическое выведение типа.

```scala
numbers.foldLeft(0)(_ + _)
```

[К оглавлению](#Основы)

## Сопоставление с примером

__Сопоставление с примером (Pattern matching)__ - механизм сравнения значений с определенным примером. При успешном совпадении значение может быть разложено на составные части (более мощная версия `switch` из Java).

_СИНТАКСИС_

Синтаксис состоит из ключевого слова `match` (сопоставить) и хотя бы одного пункта `case`, с которым мы хотим сопоставить решение.

```scala
import scala.util.Random

val x: Int = Random.nextInt(10)

x match {
  case 0 => "zero"
  case 1 => "one"
  case _ => "many"
}
```

Значение `x` - случайное число от 0 до 10. `x` левый операнд оператора `match`, справа - выражение с 3 примерами (вариантами). Последний вариант `_` - "ловит" оставшиеся варианты (числа больше 1).

Сопоставление с примером возвращает значение.

```scala
def matchTest(x: Int): String = x match {
  case 1 => "one"
  case 2 => "two"
  case _ => "many"
}
matchTest(1) // one
matchTest(3) // many
```

_СОПОСТАВЛЕНИЕ С КЛАССАМИ ОБРАЗЦАМИ_

Классы образцы особенно полезны для сопоставления.

```scala
abstract class Notification

case class Email(sender: String, title: String, body: String) extends Notification
case class SMS(caller: String, message: String) extends Notification
case class VoiceRecording(contactName: String, link: String) extends Notification
```

`Notification` - абстрактный суперкласс, от которого наследуются три реализации `Email`, `SMS`, `VoiceRecording`. При сопоставлении с классом образцом можно автоматически извлекать параметры благодаря автоматическому использованию объекта экстрактора.

```scala
def showNotification(notification: Notification): String = {
  notification match {
    case Email(email, title, _) =>
      s"You got an email from $email with title: $title"
    case SMS(number, message) =>
      s"You got an SMS from $number! Message: $message"
    case VoiceRecording(name, link) =>
      s"You received a Voice Recording from $name! Click the link to hear it: $link"
  }
}
val someSms = SMS("12345", "Are you there?")
val someVoiceRecording = VoiceRecording("Tom", "voicerecording.org/id/123")

println(showNotification(someSms)) // You got an SMS from 12345! Message: Are you there?
println(showNotification(someVoiceRecording)) // You received a Voice Recording from Tom! Click the link to hear it: voicerecording.org/id/123
```

_ОГРАЖДЕНИЯ ПРИМЕРОВ_

__Ограждения примеров__ - логические выражения, используемые, чтобы сделать выбор более специфичным (убрать лишние варианты). Нужно добавить `if <логическое выражение>`.

```scala
def showImportantNotification(notification: Notification, importantPeopleInfo: Seq[String]): String = {
  notification match {
    case Email(email, _, _) if importantPeopleInfo.contains(email) =>
      "You got an email from special someone!"
    case SMS(number, _) if importantPeopleInfo.contains(number) =>
      "You got an SMS from special someone!"
    case other =>
      showNotification(other) // подойдут любые типы
  }
}

val importantPeopleInfo = Seq("867-5309", "jack@gmail.com")

val someSms = SMS("867-5309", "Are you there?")
val someVoiceRecording = VoiceRecording("Tom", "voice.org/id")
val importantEmail = Emeil("jack@gmail.com", "Drinks tonight?", "I'm free after 5!")
val importantSms = SMS("867-5309", "I'm here! Where are you?")

println(showImportantNotification(someSms,importantPeopleInfo))
println(showImportantNotification(someVoiceRecording,importantPeopleInfo))
println(showImportantNotification(importantEmail,importantPeopleInfo))
println(showImportantNotification(importantSms,importantPeopleInfo))
```

В варианте `case Email(email, _, _) if importantPeopleInfo.contains(email)` пример сравнивается если только `email` находится в списке `importantPeopleInfo`.

_СОПОСТАВЛЕНИЕ ТОЛЬКО С ТИПОМ_

```scala
abstract class Device
case class Phone(model: String) extends Device {
  def screenOff = "Turning screen off"
}
case class Computer(model: String) extends Device {
  def screenSaverOn = "Turning screen saver on..."
}
def goIdle(device: Device) = device match {
  case p: Phone => p.screenOff
  case c: Computer => c.screenSaverOn
}
```

Метод `goIdle` реализует изменение поведения в зависимости от типа`Device`. По соглашению используется первая буква типа.

_ЗАПЕЧАТАННЫЕ КЛАССЫ_

Трейты и классы могут быть помечены как `sealed` - значит, что подтипы должны объявляться в одном файле.

```scala
sealed abstract class Furniture
case class Couch() extends Furniture
case class Chair() extends Furniture

def findPlateToSit(piece: Furniture):String = piece match {
  case a: Couch => "Lie on the couch"
  case b: Chair => "Sit on the chair"
}
```

[К оглавлению](#Основы)

## Регулярные выражения

__Регулярные выражения (Regular expression)__ - специальный шаблон для поиска данных, задаваемый в виде текстовой строки. Строка может быть преобразована в регулярное выражение методом `.r`.

```scala
import scala.util.matching.Regex

val numberPattern: Regex = "[0-9]".r

numberPattern.findFirstMatchIn("awesomepassword") match {
  case Some(_) => println("Password OK")
  case None => println("Password must containt a number")
}
```

`numberPattern` - это `Regex` для проверки того, есть ли число в пароле.

Используя `()` можно объединять несколько групп регулярных выражений.

```scala
import scala.util.matching.Regex

val keyValPattern: Regex = "([0-9a-zA-Z-#() ]+): ([0-9a-zA-Z-#() ]+)".r

val input: String =
  """background-color: #A03300;
    |background-image: url(img/header100.png);
    |background-position: top center
    |background-repeat: repeat-x;
    |background-size: 2160px 108px;
    |margin: 0;
    |height: 108px;
    |width: 100%;""".stripMargin

for (patternMatch <- keyValPattern.findAllMatchIn(input))
  println(s"key: ${patternMatch.group(1)} value: ${patternMatch.group(2)}")
```

Здесь подобраны ключи и значения. Результат:

```
key: background-color value: #A03300
key: background-image value: url(img
key: background-position value: top center
key: background-repeat value: repeat-x
key: background-size value: 2160px 108px
key: margin value: 0
key: height value: 108px
key: width value: 100
```

[К оглавлению](#Основы)

## Объект экстрактор

__Объект экстрактор/распаковщик (extractor object)__ - объект с методом `unapply`. В то время как метод `apply` действует как конструктор (принимает аргументы и создает объект), метод `unapply` действует обратным образом (принимает объект и пытается извлечь аргументы, из которых он создан).

```scala
import scala.util.Random

object CustomerID {
  def apply(name: String) = s"$name--${Random.nextLong()}"

  def unapply(customerID: String): Option[String] = {
    val stringArray: Array[String] = customerID.split("--")
    if (stringArray.tail.nonEmpty) Some(stringArray.head) else None
  }
}

val customer1ID = CustomerID("Sukyoung") // Sukyoung--23098234908
customer1ID match {
  case CustomerID(name) => println(name) // Sukyoung
  case _ => println("Could not extract a CustomerID")
}
```

Метод `apply` создает `CustomerID` из строки `name`. `unapply` возвращает `name` обратно. `CustomerID("Sukyoung)` - это сокращенный синтаксис вызова `CustomerID.apply("Sukyoung")`. При вызове `case CustomerID(name) => println(name)` вызывается метод `unaplly`.

При объявлении нового значения можно инициализировать через извлечение методом `unapply`.

```scala
val customer2ID = CustomerID("Nico")
val CustomerID(name) = customer2ID
println(name)
```

Это эквивалентно `val name = CustomerID.unapply(customer2ID).get`.

Возвращаемый тип `unapply` выбирается следующим образом:

+ если это тест, возвращается `Boolean` (напр., `case even()`);
+ если в результате найдено одно значение типа `T`, то возвращается `Option[T]`;
+ если нужно получить несколько значений `T1,..., Tn`, то ответ нужно группировать в дополнительный кортеж `Option[(T1,..., Tn)]`.

Если в зависимости от входа нужно вернуть произвольное количество значений, то экстрактор определяется методом `unapplySeq`, который возвращает `Option[Seq[T]]`.

[К оглавлению](#Основы)

## Сложные for выражения

Scala предлагает простую запись для выражения _последовательных преобразований_. Их можно упростить используя спец. синтаксис `for выражения (for comprehension)`, который записывается как `for (enumerators) yield e`, где `enumerators` - список перечислителей, разделенных `;`. Отдельный перечислитель - это генератор (вводит новые переменные) или фильтр.

```scala
case class User(name: String, age: Int)

val userBase = List(User("Travis", 28),
  User("Jack", 33) 
  User("Tom", 25))

val twentySomethings = for (user <- userBase if (user.age >= 20 && user.age < 30))
  yield user.name // добавить результат к списку

twentySomethings.foreach(name => println(name)) // Travis Tom
```

`for выражение` используется с оператором `yield` - создает `List`. Указано `yield user.name` (вывести имя пользователя), получается `List[String]`. `user <- userBase` - генератор, `if (...)` - фильтр.

Пример с двумя генераторами (вычисляет все пары чисел между `0` и `n-1`, сумма которых равна заданному `v`):

```scala
def foo(n: Int, V: Int) =
  for (i <- 0 until n;
       j <- i until n if i + j == v)
  yield (i, j)

foo(10, 10) foreach {
  case (i, j) =>
    println(s"($i, $j) ") // (1,9) (2, 8) (3, 7) (4, 6) (5, 5)
}
```

Здесь `n == 10` и `v == 10`. На первой итерации `i == 0` и `j == 0`, т.о. `i + j != v` - ничего не выдается. `j` увеличивается еще в 9 раз, прежде чем `i` увеличится до `1`. Без фильтра `if` было бы напечатано: `(0, 0) (0, 1) ...(0, 9) (1, 0) ...`.

`for выражение` не ограничивается работой со списками. Тип данных поддерживающий операции `withFilter`, `map` и `flatMap` может использоваться в `for выражении`.

Можно обойтись без `yield`, тогда результатом будет `Unit`. Программа эквивалентная предыдущей, но без `yield`:

```scala
def foo(n: Int, v: Int) =
  for (i <- 0 until n;
       j <- i until n if i + j == v)
  println(s"($i, $j")

foo(10, 10)
```

[К оглавлению](#Основы)



[Learning Scala](README.md#scala)