# Strategy

## 动机（Motivation）
+ 在软件构建过程中，某些对象使用的算法可能多种多样，经常改变，如果将这些算法都编码到对象中，将会使对象变得异常复杂；而且有时候支持不使用的算法也是一个性能负担。
+ 如何在运行时根据需要透明地更改对象的算法？将算法与对象本身解耦，从而避免上述问题？

## 模式定义
定义一系列算法，把它们一个个封装起来，并且使它们可互相替换（变化）。该模式使得算法可独立于使用它的客户程序(稳定)而变化（扩展，子类化）。
——《设计模式》 GoF

## 结构演化

### 阶段一：枚举 + if/else（strategy1.cpp）—— 紧耦合

```mermaid
classDiagram
    direction TB

    class TaxBase {
        <<enumeration>>
        CN_Tax
        US_Tax
        DE_Tax
        FR_Tax
    }

    class SalesOrder {
        -TaxBase tax
        +CalculateTax() double
    }
```

> 问题：`CalculateTax()` 内部用大段 `if/else` 判断 `tax` 枚举值计算税款。新增税率需修改现有代码（违反开闭原则）。

### 阶段二：Strategy 模式（strategy2.cpp）—— 算法对象化

```mermaid
classDiagram
    direction TB

    class TaxStrategy {
        <<abstract>>
        +Calculate(context Context) double*
    }

    class CNTax {
        +Calculate(context Context) double
    }
    class USTax {
        +Calculate(context Context) double
    }
    class DETax {
        +Calculate(context Context) double
    }
    class FRTax {
        +Calculate(context Context) double
    }

    class SalesOrder {
        -TaxStrategy* strategy
        +CalculateTax() double
    }

    TaxStrategy <|-- CNTax
    TaxStrategy <|-- USTax
    TaxStrategy <|-- DETax
    TaxStrategy <|-- FRTax
    SalesOrder --> TaxStrategy : strategy
```

> 完美：`SalesOrder` 只依赖 `TaxStrategy` 抽象。新增税率只需增加子类，无需改动现有代码。策略可在运行时替换。

## 要点总结
+ Strategy及其子类为组件提供了一系列可重用的算法，从而可以使得类型在运行时方便地根据需要在各个算法之间进行切换。
+ Strategy模式提供了用条件判断语句以外的另一种选择，消除条件判断语句，就是在解耦合。含有许多条件判断语句的代码通常都需要Strategy模式。
+ 如果Strategy对象没有实例变量，那么各个上下文可以共享同一个Strategy对象，从而节省对象开销。
