# Практика: Роботы

## 1. Описание предметной области и сущностей

Проведено упражнение на ковариацию и контравариацию. Были применены обобщенные Generics интерфейсы, чтобы архитектура стала строго типизированной.

## 2. Диаграмма классов (Mermaid)

```mermaid

classDiagram
    %% Interfaces
    class IRobotAI~TCommand~ {
        <<interface>>
        +GetCommand() TCommand
    }

    class IDevice~TCommand~ {
        <<interface>>
        +ExecuteCommand(TCommand command) string
    }

    %% Abstract Classes
    class RobotAI~TCommand~ {
        <<abstract>>
        +GetCommand()* TCommand
    }

    class Device~TCommand~ {
        <<abstract>>
        +ExecuteCommand(TCommand command)* string
    }

    %% Concrete Classes
    class ShooterAI {
        -int counter
        +GetCommand() ShooterCommand
    }

    class BuilderAI {
        -int counter
        +GetCommand() BuilderCommand
    }

    class Mover {
        +ExecuteCommand(IMoveCommand _command) string
    }

    class ShooterMover {
        +ExecuteCommand(IShooterMoveCommand _command) string
    }

    class Robot~TCommand~ {
        -IRobotAI~TCommand~ ai
        -IDevice~TCommand~ device
        +Robot(IRobotAI~TCommand~ ai, IDevice~TCommand~ executor)
        +Start(int steps) IEnumerable~string~
    }

    class RobotFactory {
        <<static>>
        +Create~TCommand~(IRobotAI~TCommand~ ai, IDevice~TCommand~ executor) Robot~TCommand~
    }

    %% Relationships
    RobotAI~TCommand~ ..|> IRobotAI~TCommand~ : Реализует
    Device~TCommand~ ..|> IDevice~TCommand~ : Реализует

    ShooterAI --|> RobotAI~ShooterCommand~ : Наследует
    BuilderAI --|> RobotAI~BuilderCommand~ : Наследует

    Mover --|> Device~IMoveCommand~ : Наследует
    ShooterMover --|> Device~IShooterMoveCommand~ : Наследует

    Robot~TCommand~ --> IRobotAI~TCommand~ : Композиция (ai)
    Robot~TCommand~ --> IDevice~TCommand~ : Композиция (device)
    RobotFactory ..> Robot~TCommand~ : Создает
    
```
