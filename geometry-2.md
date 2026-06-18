# Практика: Геометрия-2

## 1. Описание предметной области и сущностей

В данной практике релизуется шаблон Visitor, позволяющий добавлять в программу новые операции, не изменяя классы тех объектов, над которыми эти операции выполняются.
Через интерфейс IVisitor написаны два обработчика: BoundingBoxVisitor (считает габариты фигуры) и BoxifyVisitor (превращает фигуры в параллелепипеды). Метод Accept внутри фигур принимает объект посетителя и вызывает нужное действие.

## 2. Диаграмма классов (Mermaid)

```mermaid

classDiagram
    %% Базовый класс и интерфейс
    class Body {
        <<abstract>>
        +Vector3 Position
        +Accept(IVisitor visitor)* Body
    }
    
    class IVisitor {
        <<interface>>
        +Visit(Ball ball) Body
        +Visit(RectangularCuboid cuboid) Body
        +Visit(Cylinder cylinder) Body
        +Visit(CompoundBody compound) Body
    }

    %% Конкретные классы фигур
    class Ball {
        +double Radius
        +Accept(IVisitor visitor) Body
    }
    class RectangularCuboid {
        +double SizeX
        +double SizeY
        +double SizeZ
        +Accept(IVisitor visitor) Body
    }
    class Cylinder {
        +double SizeZ
        +double Radius
        +Accept(IVisitor visitor) Body
    }
    class CompoundBody {
        +IReadOnlyList~Body~ Parts
        +Accept(IVisitor visitor) Body
    }

    %% Конкретные посетители
    class BoundingBoxVisitor {
        +Visit(Ball ball) Body
        +Visit(RectangularCuboid cuboid) Body
        +Visit(Cylinder cylinder) Body
        +Visit(CompoundBody compound) Body
    }
    class BoxifyVisitor {
        +Visit(Ball ball) Body
        +Visit(RectangularCuboid cuboid) Body
        +Visit(Cylinder cylinder) Body
        +Visit(CompoundBody compound) Body
    }

    %% Связи наследования и реализации
    Body <|-- Ball
    Body <|-- RectangularCuboid
    Body <|-- Cylinder
    Body <|-- CompoundBody

    IVisitor <|.. BoundingBoxVisitor
    IVisitor <|.. BoxifyVisitor

    CompoundBody o-- Body : содержит Parts

    %% Зависимости
    BoundingBoxVisitor ..> Body : обрабатывает
    BoxifyVisitor ..> Body : обрабатывает
    
```
