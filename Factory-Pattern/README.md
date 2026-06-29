## 工厂模式

```mermaid
classDiagram
    direction TB

    class Pizza {
        -string name
        -string dough
        -string sauce
        -vector~string~ toppings
        +prepare() void
        +bake() void
        +cut() void
        +box() void
    }

    class NYStyleCheesePizza {
        +NYStyleCheesePizza()
    }
    class NYStylePepperoniPizza {
        +NYStylePepperoniPizza()
    }
    class ChicagoStyleCheesePizza {
        +ChicagoStyleCheesePizza()
    }
    class ChicagoStylePepperoniPizza {
        +ChicagoStylePepperoniPizza()
    }

    class PizzaStore {
        <<abstract>>
        +orderPizza(type) Pizza*
        +createPizza(type) Pizza*
    }

    class NYPizzaStore {
        +createPizza(type) Pizza*
    }
    class ChicagoPizzaStore {
        +createPizza(type) Pizza*
    }

    Pizza <|-- NYStyleCheesePizza
    Pizza <|-- NYStylePepperoniPizza
    Pizza <|-- ChicagoStyleCheesePizza
    Pizza <|-- ChicagoStylePepperoniPizza
    PizzaStore <|-- NYPizzaStore
    PizzaStore <|-- ChicagoPizzaStore
    PizzaStore ..> Pizza : creates
```

> `PizzaStore::orderPizza()` 是模板方法，调用子类实现的 `createPizza()` 工厂方法。

![](https://img-blog.csdnimg.cn/20190623171420456.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlc3Ricm9va2xpdQ==,size_16,color_FFFFFF,t_70)