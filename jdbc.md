[Learning Java](README.md#java)

# JDBC

+ [Введение](#Введение)
+ [Connectionn Pool](#connection-pool)

## Введение

__Java-приложение -> JDBC API (есть в Java, но нет реализации) -> Driver (предоставляет реализации)__

__JDBC (Java Database Connectivity)__ - технология, которая связывает Java-приложение с любой реляционной базой данных (пакет java.sql - основные классы и интерфейсы для работы с БД). Конкретная реализация для каждой СУБД описаны в соответствующей jar-библиотеке JDBC-драйвера.

__Драйвер__ - определенный класс, который связывает Java и БД. Драйвер есть на каждый тип СУБД.

__JDBC API__ содержит интерфейсы: Connection, Statement/PreparedStatement, ResultSet.

__Driver__ предоставляет реализации (напр., PostgreSQL): PgConnection, PgStatement/PgPreparedStatement, PgResultSet.

+ DriverManager - класс, который выполняет подключение доступного драйвера и предоставляет подлкючение к СУБД;
+ Connection - интерфейс, описывающий сеанс подключения к БД;
+ Statement - интерфейс, описывающий объект для отправки запросов в СУБД;
+ PreparedStatement - интерфейс, описывающий объект для отправки подготовленных запросов в СУБД;
+ ResultSet - интерфейс, описывающий объект, представляющий из себя итератор по данным, которые пришли из СУБД.

Стандартный сеанс взаимодействия через JDBC (dbProperties загружен свойствами из файла):
```java
//language=SQL (подсказка при написании SQL запроса)
final String SQL_SELECT_ALL = "select * from table_name order by column_name";
//подключение к БД
try (Connection connection = DriverManeger.getConnection(
        dbProperties.getProperty("db.url"),
        dbProperties.getProperty("db.username"),
        dbProperties.getProperty("db.password"));
    //создание объекта для отправки запросов
    Statement statement = connection.sreateStatement()) {
    //выполнение запроса и получение результата
    try (ResultSet resultSet = statemen.executeQuery(SQL_SELECT_ALL)) {
        //перебор строк
        while (resultSet.next()) {
            System.out.println(resultSet.getString("another_column_name"));
        }
    }
} catch (SQLException e) {
    throw new IllegalStateException (e);
}
```
__PreparedStatement__ - подготовленный запрос, используется для предотвращения sql-инъекции. В данном запросе для каждого входного параметра (обозначается как ?) указывается тип значения.
```java
Connection connection = DriverManager.getConnection(
        dbProperties.getPorperty("db.url"),
        dbProperties.getPorperty("db.username"),
        dbProperties.getPorperty("db.password"));
Scanner scanner = new Scanner(System.in);
String str = scanner.newxtLine();
String sql = "insert into table_name(column_name) values (?)";
PreparedStatement statement = connection.preparedStatement(sql);
statement.setString(1, str);
statement.executeUpdate();
```

[К оглавлению](#jdbc)

## Connection Pool

При работе с БД может быть сразу направлено несколько запросов, т.о. использование одного Connection будет замедлять приложение. Для решения проблемы используется паттерн __Connection Poll__ (библиотека HikariCP):
+ набор постоянно открытых Connection на протяжении жизни приложения;
+ очередь запросов, которая направляется в освободившийся Connection
```java
HikariDataSource hikari = new HikariDataSource();
hikari.setPassword(dbProperties.getProperty("db.password"));
hikari.setUsername(dbProperties.getProperty("db.username"));hikari.setJdbcUrl(dbProperties.getProperty("db.url"));
hikari.setMaximumPoolSize(Integer.parseInt(dbProperties.getProperty("db.hikari.maxPoolSize")));

DataSource dataSource = hikari;

final String SQL_SELECT_ALL = "select * from student order by id";
        
try (Connection connection = dataSource.getConnection());
Statement statement = connection.createStatement()) {
// выполнение запроса и получение результата
    try (ResultSet resultSet = statement.executeQuery(SQL_SELECT_ALL)) {
    // последовательный перебор результирующего набора строк
        while (resultSet.next()) {
            // печать значения столбца
            System.out.println(resultSet.getString("first_name"));
        }
    }
} catch (SQLException e) {
throw new IllegalStateException(e);
}
```

[К оглавлению](#jdbc)

[Learning Java](README.md#java)