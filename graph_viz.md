# Практика: GraphViz

## 1. Описание предметной области и сущностей

Проведено упражнение на проектирование Fluent API (текучего интерфейса) на базе паттерна Builder. Было применено разделение интерфейсов и лямбда-контексты With(), чтобы перенести проверки ограничений DOT-формата на уровень компиляции.

## 2. Диаграмма классов (Mermaid)

```mermaid
classDiagram
    class IGraphBuilder {
        <<interface>>
        +AddNode(nodeName: string) INodeBuilder
        +AddEdge(from: string, to: string) IEdgeBuilder
        +Build() string
    }

    class INodeBuilder {
        <<interface>>
        +With(configure: Action~INodeAttributes~) IGraphBuilder
    }

    class IEdgeBuilder {
        <<interface>>
        +With(configure: Action~IEdgeAttributes~) IGraphBuilder
    }

    class INodeAttributes {
        <<interface>>
        +Color(colorName: string) INodeAttributes
        +FontSize(size: int) IEdgeAttributes
        +Label(labelText: string) INodeAttributes
        +Shape(shapeType: NodeShape) INodeAttributes
    }

    class IEdgeAttributes {
        <<interface>>
        +Color(colorName: string) IEdgeAttributes
        +FontSize(size: int) IEdgeAttributes
        +Label(labelText: string) IEdgeAttributes
        +Weight(weightValue: double) IEdgeAttributes
    }

    class NodeShape {
        <<enum>>
        Box
        Ellipse
    }

    class DotGraphBuilder {
        -Graph myGraph
        -GraphNode nodeInstance
        -GraphEdge edgeInstance
        +DirectedGraph(graphName: string)\$ IGraphBuilder
        +UndirectedGraph(graphName: string)\$ IGraphBuilder
    }

    IGraphBuilder <|-- INodeBuilder
    IGraphBuilder <|-- IEdgeBuilder

    IGraphBuilder <|.. DotGraphBuilder
    INodeBuilder <|.. DotGraphBuilder
    IEdgeBuilder <|.. DotGraphBuilder
    INodeAttributes <|.. DotGraphBuilder
    IEdgeAttributes <|.. DotGraphBuilder

    INodeAttributes ..> NodeShape
```
