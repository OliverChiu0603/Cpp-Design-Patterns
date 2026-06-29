## 命令模式

```mermaid
classDiagram
    direction TB

    class Command {
        <<interface>>
        +execute() void*
    }

    class LightOnCommand {
        -string light
        +execute() void
    }

    class GarageDoorOpen {
        -string door
        +execute() void
    }

    class SimpleRemoteControl {
        -Command* slot
        +setCommand(Command*) void
        +buttonWasPressed() void
    }

    Command <|-- LightOnCommand
    Command <|-- GarageDoorOpen
    SimpleRemoteControl --> Command : slot
```

> `SimpleRemoteControl` 是 Invoker（调用者），`LightOnCommand` / `GarageDoorOpen` 是 ConcreteCommand（具体命令）。Invoker 只依赖 `Command` 接口，与具体命令解耦。

![](https://img-blog.csdnimg.cn/20190616212348873.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlc3Ricm9va2xpdQ==,size_16,color_FFFFFF,t_70)