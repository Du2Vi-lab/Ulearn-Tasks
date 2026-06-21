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

    IGraphBuilder <|-- INodeBuilder : расширяет
    IGraphBuilder <|-- IEdgeBuilder : расширяет

    class INodeAttributes {
        <<interface>>
        +Color(colorName: string) INodeAttributes
        +FontSize(size: int) INodeAttributes
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
    INodeAttributes ..> NodeShape : использует

    class DotGraphBuilder {
        -Graph myGraph
        +DirectedGraph(graphName: string)$ IGraphBuilder
        +UndirectedGraph(graphName: string)$ IGraphBuilder
        +AddNode(nodeName: string) INodeBuilder
        +AddEdge(from: string, to: string) IEdgeBuilder
        +Build() string
    }
    IGraphBuilder <|.. DotGraphBuilder : реализует

    class GraphNodeBuilder {
        -IGraphBuilder graphBuilder
        -GraphNode currentNode
        +With(configure: Action~INodeAttributes~) IGraphBuilder
        +Color(colorName: string) INodeAttributes
        +FontSize(size: int) INodeAttributes
        +Label(labelText: string) INodeAttributes
        +Shape(shapeType: NodeShape) INodeAttributes
        +AddNode(nodeName: string) INodeBuilder
        +AddEdge(from: string, to: string) IEdgeBuilder
        +Build() string
    }
    INodeBuilder <|.. GraphNodeBuilder : реализует
    INodeAttributes <|.. GraphNodeBuilder : реализует
    GraphNodeBuilder --> IGraphBuilder : делегирует вызовы

    class GraphEdgeBuilder {
        -IGraphBuilder graphBuilder
        -GraphEdge currentEdge
        +With(configure: Action~IEdgeAttributes~) IGraphBuilder
        +Color(colorName: string) IEdgeAttributes
        +FontSize(size: int) IEdgeAttributes
        +Label(labelText: string) IEdgeAttributes
        +Weight(weightValue: double) IEdgeAttributes
        +AddNode(nodeName: string) INodeBuilder
        +AddEdge(from: string, to: string) IEdgeBuilder
        +Build() string
    }
    IEdgeBuilder <|.. GraphEdgeBuilder : реализует
    IEdgeAttributes <|.. GraphEdgeBuilder : реализует
    GraphEdgeBuilder --> IGraphBuilder : делегирует вызовы
    
```
