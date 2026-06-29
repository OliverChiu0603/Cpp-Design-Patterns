# Abstract Factory

## 动机(motivation)
+ 在软件系统中，经常面临着“一系列相互依赖的对象工作”；同时，由于需求的变化，往往存在更多系列对象的创建工作。
+ 如何应对这种变化？如何绕过常规的对象创建方法(new)，提供一种“封装机制”来避免客户程序和这种“多系列具体对象创建工作”的紧耦合。

## 模式定义
提供一个接口，让该接口负责创建一系列”相关或者相互依赖的对象“，无需指定它们具体的类。
——《设计模式》GoF
## 结构演化

### 阶段一：无工厂（EmployeeDAO1.cpp）—— 紧耦合

```mermaid
classDiagram
    direction TB

    class EmployeeDAO {
        +GetEmployees() vector~Employee~
    }

    class SqlConnection {
        +ConnectionString string
    }

    class SqlCommand {
        +CommandText string
    }

    class SqlDataReader {
        +Read() DataRow
    }

    EmployeeDAO ..> SqlConnection : new
    EmployeeDAO ..> SqlCommand : new
    EmployeeDAO ..> SqlDataReader : new
```

> 问题：`EmployeeDAO` 直接 `new` SQL Server 相关类，与具体数据库紧耦合，无法切换数据库。

### 阶段二：独立工厂（EmployeeDAO2.cpp）—— 工厂分离但不保证系列

```mermaid
classDiagram
    direction TB

    class EmployeeDAO {
        -IDBConnectionFactory* dbConnectionFactory
        -IDBCommandFactory* dbCommandFactory
        -IDataReaderFactory* dataReaderFactory
        +GetEmployees()
    }

    class IDBConnectionFactory {
        <<abstract>>
        +CreateDBConnection() IDBConnection*
    }
    class SqlConnectionFactory {
        +CreateDBConnection() IDBConnection*
    }
    class OracleConnectionFactory {
        +CreateDBConnection() IDBConnection*
    }

    class IDBCommandFactory {
        <<abstract>>
        +CreateDBCommand() IDBCommand*
    }
    class SqlCommandFactory {
        +CreateDBCommand() IDBCommand*
    }
    class OracleCommandFactory {
        +CreateDBCommand() IDBCommand*
    }

    class IDataReaderFactory {
        <<abstract>>
        +CreateDataReader() IDataReader*
    }
    class SqlDataReaderFactory {
        +CreateDataReader() IDataReader*
    }
    class OracleDataReaderFactory {
        +CreateDataReader() IDataReader*
    }

    IDBConnectionFactory <|-- SqlConnectionFactory
    IDBConnectionFactory <|-- OracleConnectionFactory
    IDBCommandFactory <|-- SqlCommandFactory
    IDBCommandFactory <|-- OracleCommandFactory
    IDataReaderFactory <|-- SqlDataReaderFactory
    IDataReaderFactory <|-- OracleDataReaderFactory

    EmployeeDAO --> IDBConnectionFactory
    EmployeeDAO --> IDBCommandFactory
    EmployeeDAO --> IDataReaderFactory
```

> 问题：三个独立工厂，可能混搭不同系列产品（如 SQL Connection + Oracle Command），破坏"系列"约束。

### 阶段三：抽象工厂（EmployeeDAO3.cpp）—— 保证系列一致性

```mermaid
classDiagram
    direction TB

    class EmployeeDAO {
        -IDBFactory* dbFactory
        +GetEmployees()
    }

    class IDBFactory {
        <<abstract>>
        +CreateDBConnection() IDBConnection*
        +CreateDBCommand() IDBCommand*
        +CreateDataReader() IDataReader*
    }

    class SqlDBFactory {
        +CreateDBConnection() IDBConnection*
        +CreateDBCommand() IDBCommand*
        +CreateDataReader() IDataReader*
    }

    class OracleDBFactory {
        +CreateDBConnection() IDBConnection*
        +CreateDBCommand() IDBCommand*
        +CreateDataReader() IDataReader*
    }

    class IDBConnection {
        <<abstract>>
    }
    class SqlConnection {
    }
    class OracleConnection {
    }

    class IDBCommand {
        <<abstract>>
    }
    class SqlCommand {
    }
    class OracleCommand {
    }

    class IDataReader {
        <<abstract>>
    }
    class SqlDataReader {
    }
    class OracleDataReader {
    }

    IDBFactory <|-- SqlDBFactory
    IDBFactory <|-- OracleDBFactory
    EmployeeDAO --> IDBFactory

    IDBConnection <|-- SqlConnection
    IDBConnection <|-- OracleConnection
    IDBCommand <|-- SqlCommand
    IDBCommand <|-- OracleCommand
    IDataReader <|-- SqlDataReader
    IDataReader <|-- OracleDataReader

    SqlDBFactory ..> SqlConnection : creates
    SqlDBFactory ..> SqlCommand : creates
    SqlDBFactory ..> SqlDataReader : creates
    OracleDBFactory ..> OracleConnection : creates
    OracleDBFactory ..> OracleCommand : creates
    OracleDBFactory ..> OracleDataReader : creates
```

> 完美：单个 `IDBFactory` 接口统一创建整系列对象，消除混搭风险。添加新数据库系列只需新增一个工厂类。
## 要点总结
+ 如果没有应对”多系列对象创建“的需求变化，则没有必要使用Abstract Factory模式，这时候使用简单的工厂即可。
+ ”系列对象“指的是在某一个特定系列的对象之间有相互依赖、或作用的关系。不同系列的对象之间不能相互依赖。
+ Abstract Factory模式主要在于应用”新系列“的需求变动。其缺点在与难以应对”新对象“的需求变动。
