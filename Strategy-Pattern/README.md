## 策略模式

```mermaid
classDiagram
    direction TB

    class Duck {
        <<abstract>>
        -unique_ptr~FlyBehavior~ flyBehavior
        -unique_ptr~QuackBehavior~ quackBehavior
        +performFly() void
        +performQuack() void
        +swim() void
        +display() void*
    }

    class MallardDuck {
        +display() void
    }
    class RedHeadDuck {
        +display() void
    }
    class RubberDuck {
        +display() void
    }
    class DecoyDuck {
        +display() void
    }

    class FlyBehavior {
        <<interface>>
        +fly() void*
    }
    class FlyWithWings {
        +fly() void
    }
    class FlyNoWay {
        +fly() void
    }

    class QuackBehavior {
        <<interface>>
        +quack() void*
    }
    class Quack {
        +quack() void
    }
    class Squeak {
        +quack() void
    }
    class MuteQuack {
        +quack() void
    }

    Duck <|-- MallardDuck
    Duck <|-- RedHeadDuck
    Duck <|-- RubberDuck
    Duck <|-- DecoyDuck
    FlyBehavior <|-- FlyWithWings
    FlyBehavior <|-- FlyNoWay
    QuackBehavior <|-- Quack
    QuackBehavior <|-- Squeak
    QuackBehavior <|-- MuteQuack
    Duck --> FlyBehavior
    Duck --> QuackBehavior
```

> 飞行行为和叫声行为被抽象为独立的策略接口。每种鸭子组合不同的行为策略，且可在运行时替换。

| 鸭子 | 飞行 | 叫声 |
|------|------|------|
| MallardDuck | FlyWithWings | Quack |
| RedHeadDuck | FlyWithWings | Quack |
| RubberDuck | FlyNoWay | Squeak |
| DecoyDuck | FlyNoWay | MuteQuack |

![](https://img-blog.csdnimg.cn/20190527222242725.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlc3Ricm9va2xpdQ==,size_16,color_FFFFFF,t_70)
