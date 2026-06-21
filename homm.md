# Практика: HoMM

## 1. Описание предметной области и сущностей

Программа реализует поведение персонаж игрока и взаимодействует с различными объектами на карте. Всего существует три способа взаимодействия: 
* Сражаться с армией — проверка сил игрока против охраны объекта.
* Собирать сокровища — получение ресурсов или наград, находящихся внутри объекта.
* Присваивать объект себе — смена текущего владельца объекта на ID игрока.

## 2. Диаграмма классов (Mermaid)

```mermaid
classDiagram
    class InterfaceOwner {
        <<interface>>
        +int Owner
    }
    class InterfaceArmy {
        <<interface>>
        +Army Army
    }
    class InterfaceTreasure {
        <<interface>>
        +Treasure Treasure
    }

    class Dwelling {
        +int Owner
    }
    class Mine {
        +int Owner
        +Army Army
        +Treasure Treasure
    }
    class Creeps {
        +Army Army
        +Treasure Treasure
    }
    class Wolves {
        +Army Army
    }
    class ResourcePile {
        +Treasure Treasure
    }

    InterfaceOwner <|.. Dwelling
    InterfaceOwner <|.. Mine
    InterfaceArmy <|.. Mine
    InterfaceTreasure <|.. Mine
    InterfaceArmy <|.. Creeps
    InterfaceTreasure <|.. Creeps
    InterfaceArmy <|.. Wolves
    InterfaceTreasure <|.. ResourcePile

    class Army
    class Treasure
    class Player {
        +Id
        +CanBeat(Army) bool
        +Die()
        +Consume(Treasure)
    }

    InterfaceArmy --> Army : содержит
    InterfaceTreasure --> Treasure : содержит

    class Interaction {
        <<static>>
        +Make~TMapObject~(Player player, TMapObject mapObject)\$
    }

    Interaction ..> Player : использует
    Interaction ..> InterfaceArmy : проверяет
    Interaction ..> InterfaceOwner : проверяет
    Interaction ..> InterfaceTreasure : проверяет
```
