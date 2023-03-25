[Learning Java](README.md#java)

# Maven

+ [Введение](#Введение)
+ [Жизненный цикл Maven (Maven lifecycle)](#Жизненный_цикл_maven_maven_lifecycle)
+ [Установка Maven через командную строку](#Установка-maven-через-командную-строку)
+ [Создание Maven проекта в IntelliJ IDEA](#Создание-maven-проекта-в-intelliJ-iDEA)
+ [Создание Maven проекта из архетипа](#Создание-maven-проекта-из-архетипа)
+ [Структура Maven](#Структура-maven)
+ [Профиль сборки (build profile)](#Профиль_сборки_build_profile)

## Введение

__Maven__ - инструмент для сборки проекта. Maven позволяет:
+ подтянуть необходимые библиотеки (dependency);
+ определить как и во что нужно собрать проект (war, jar, executable jar);
+ задать версию проекта в одном месте, чтобы она была указана при сборке;
+ описать проект и его жизненный цикл;
+ добавлять плагины;
+ публиковать библиотеки в общем хранилище.

[К оглавлению](#maven)

## Жизненный цикл Maven (Maven lifecycle)

Жизненный цикл - набор задач, которые может выполнить Maven (задачи могут быть и другие). 

Сборка проекта разбита на фазы:
+ validate - проверяет корректность метаинформации о проекте;
+ compile - компилирует исходники;
+ test - прогоняет тесты классов из предыдущего шага;
+ package - упаковывает скомпилированные классы в удобноперемещаемый формат (war, jar);
+ verify - проверяет корректность пакета и удовлетворение требованиям качества;
+ install - загоняет папки в локальный репозиторий, откуда пакет будет доступен для использования как зависимость в других проектах;
+ deploy- отправляет пакет на удаленный production сервер, откуда другие разработчики могут его получить и использовать.

Шаги Maven последовательны (напр., если вызвать package, до него выполнятся validate, compile и test). Для запуска через консоль: `maven phaseName`.

Так же кроме стандартных фаз у Maven есть фазы `clean` - очистка папки target и `site` - создает документацию проекта.

Команды в Maven делятся на три цикла: clean, default, site.

У каждого цикла свой порядок фаз:
`clean`: pre-clean, clean, post-clean;
`site`: pre-site, site, post-site, site-deploy;
`default`: validate, generate-sources, process-sources, generate-resources, process-resources, compile, process-test-sources, process-test-resources, test-compile, test, package, install, deploy.

[К оглавлению](#maven)

## Установка Maven через командную строку

1) Скачать на оф. сайте последнюю версию.
2) Распаковать архив.
3) Создать переменную в "Переменные среды" с именем M2_HOME и значением путь до Maven (C:\apache-maven-2.2.1).
4) Добавляем в переменную PATH путь до исполняемых файлов Maven (C:\apache-maven-2.2.1\bin).
5) Проверяем работоспособность в командной строке:
`mvn -version`
Должна появиться инфо-ия о версии Maven, jre и OS:
`Maven version: 2.2.1
Java version: 1.6.0_10
OS name: "windows 2003" version: "5.2" arch: "x86" Family: "windows"`

Maven создат локальный репозиторий в личной папке, например C:\Documents and Settings\username\.m2\repository

[К оглавлению](#maven)

## Создание Maven проекта в IntelliJ IDEA

1) New -> Project -> Maven.
2) Указываем groupId и artifactId.
3) Указываем директорию для хранения проекта.

Создается проект со структурой:
`
ProjectName
    src
        main
            java
            resources
        test
            java
            resources
    target
    pom.xml
`

[К оглавлению](#maven)

## Создание Maven проекта из архетипа

__Архетип__ - стандартная компоновка файлов и каталогов в проектах различного рода (веб, swing-проекты и т.д.). Получиться список доступных архетипов `mvn archetype:generate`. Популярные архетипы:
+ maven-archetype-quickstart (пустой Java-проект);
+ maven-archetype-site (сайт);
+ maven-archetype-webapp (веб-приложение);
+ maven-archetype-j2ee-simple (сайт);
+ jpa-maven-archetype (для работы с Hibernate и JPA);
+ spring-mvc-quickstart (для работы со Spring).

`mvn archetype:create -DgroupId=com.mycompany.app -DartifactId=my-webapp -DarchetypeARtifactId=maven-archetype-webapp`

[К оглавлению](#maven)

## Структура Maven

Все, что нужно, находиться в `pom.xml` (информация о проекте, разработчик, в каком удаленном репозитории хранится).

### Блок `<project/>`

__<project>__ - главный блок, где содержится вся информация о проекте. Внутри открывающегося тега написано что-то такое:
`
<project xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xmlns="http://maven.apache.org/POM/4.0.0"
        xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                     http://maven.apache.org/xsd/maven-4.0.0.xsd">
`

Далее пишется что-то такое: `<modelVersion>4.0.0</modelVersion>` - это тоже по всех помниках.

### Архетип

Далее следует:

`
<groupId>com.github.study</groupId>
<artifactId>web-app</artifactId>
<version>1.0-SNAPSHOT</version>
<packaging>jar</packaging>

<name>Web App</name>
`

+ `groupId` - идентификатор девелоперской организации или инженера (обычно домен в обратном порядке);
+ `artifactId` - уникальное имя проекта;
+ `version` - версия проекта (что-то поменялось - увеличивается версия);
+ `packaging` - как Maven должен собрать проект (jar, war и т.д.);
+ `name` - название проекта.

### Зависимости и репозитории

Большинство популярных библиотек находятся в центральном репозитории `https://mvnrepository.com/`, их можно прописать в раздел `dependencies` `pom.xml` файла.
`
<dependencies>
      ...
      <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring</artifactId>
         <version>2.5.5</version>
      </dependency>
</dependencies>
`

Во время сборки Maven будет искать библиотеку (артефакт) в локальном репозитории, если не найдет, то в глобальном Maven-репозитории и потом загрузит в локальный (ускорение следующей сборки). Так же у крупных компаний обычно есть свой Maven-репозиторий с библиотеками. Подключение стороннего репозитория:
`
<repositories>
  ...  
  <repository>
  	<id>public-javarush-repo</id>
  	<name>Public JavaRush Repository</name>
  	<url>http://maven.javarush.com</url>
  </repository>
</repositories>
`

### Блок `<build/>`

В блоке `<build/>` находится блок `<plugins\`.

__Плагин__ - зависимость Maven, расширяющая его функционал (вставка новых шагов в стандартный цикл). Может привязываться к одной или нескольким фазам. Если не привязана к фазе - запускается вне фаз сборки с помощью прямого вызова.

`
<build>
    <plugins>
        ...
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-source-plugin</artifactId>
          <version>3.2.0</version>
          <executions>
            <execution>
                <id>attach-sources</id>
                <goals>
                    <goal>jar</goal>
                </goals>
                <configuration>
                    <tasks>
                        <echo>Hello</echo>
                    </tasks>
                </configuration>
            </execution>
        </executions>  
       </plugin>
    </plugins>
</build>
`

`<executions>` - выполнения
`<execution>` - выполнение
`<phase>` - на какой фазе
`<goal>` - триггер
`<task>` - что должен делать

Популярные плагины:
+ maven-compiler-plugin (управляет Java-компиляцией);
+ maven-resources-plugin (управляет включением ресурсов в сборку);
+ maven-source-plugin (управляет включением исходного кода в сборку);
+ maven-dependency-plugin (управляет процессом копирования библиотек зависимостей);
+ maven-jar-plugin (плагин для создания итогового jar-файла);
+ maven-war-plugin (плагин для создания итогового war-файла);
+ maven-surefire-plugin (управляет запуском тестов);
+ buildnumber-maven-plugin (генерирует номер сборки).

__Отличия jar и war-файлов__

jar-библиотека (__J__ava __Ar__chive)- просто zip архив. Обычно содержит: скомпилированные классы; ресурсы (properties-файлы и т.п.); манифест MANIFEST.MF; другие jar-библиотеки (редко).
`
META-INF/
    MANIFEST.MF
com/
    javarush/
        MyApplication.class
application.properties
`

war-файл (__W__eb __Ar__chive). Структура war-файла состоит обычно из 2-х частей: __Java-часть__ (скомпилированные классы, ресурсы для java-классов (properties-файлы и т.п.), другие jar-библиотеки (часто), манифест MANIFEST.MF) и __Web-часть__ (web-xml - дескриптор развертывания веб-сервиса, jsp-сервлеты, статические веб ресурсы (HTML, CSS, JS-файлы)).
`
META-INF/
    MANIFEST.MF
WEB-INF/
    web.xml
    jsp/
        first.jsp
    classes/
        static/
        templates/
        application.properties
    lib/
        // *.jar files as libs
`

### Переменные в Maven - properties

Часто встречающиеся параметры можно вынести в переменные (напр., версию Java, библиотеки, путь к ресурсам).
`
<properties>
    <junit.version>5.2</junit.version>
    ...
</properties>
`

Обращение происходит символом `$`.
`
<dependencies>
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>${junit.version}</version>
        <scope>test</scope>
    </dependency>
</dependencies>

[К оглавлению](#maven)

## Профиль сборки (build profile)

Профиль сборки - набор конфигурационных значений или свойств, которые могут быть использованы для переопределения стандартных значений сборщика Maven. Нужны, чтобы определить параметры сборки для различных окружений (development, testing, production). Вступают в силу на этапе сборки проекта. Есть три основных типа профиля: профиль проекта (pom.xml), файл settings.xml (в папке \Users\Home\.m2\settings.xml), глобальный профиль (\maven\config\settings.xml).

[К оглавлению](#maven)

[Learning Java](README.md#java)