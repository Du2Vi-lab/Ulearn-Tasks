# Практика: Дифференцирование

## 1. Описание предметной области и сущностей

Выполнено упражнение по реализации системы символьного дифференцирования математических выражений на основе библиотеки System.Linq.Expressions. С использованием современного механизма сопоставления шаблонов (Pattern Matching) была построена рекурсивная обработка узлов дерева выражений для правил сложения, умножения, синуса и косинуса, а также настроена информативная обработка исключений ArgumentException для нестроковых или неподдерживаемых операций.

## 2. Диаграмма классов (Mermaid)

```mermaid

classDiagram
    class Algebra {
        -Dictionary~ExpressionType, Func~ OpRules$
        -Dictionary~MethodInfo, Func~ FuncRules$
        -MethodInfo SinMethod$
        -MethodInfo CosMethod$
        +Differentiate(Expression~Func~ function)$ Expression~Func~
        -Differentiate(Expression expr)$ Expression
    }

    class Expression {
        +Add(left, right)$
        +Multiply(left, right)$
        +Call(method, arguments)$
        +Constant(value)$
        +Lambda(body, parameters)$
    }

    class MethodInfo {
    }

    Algebra ..> Expression : Использует
    Algebra ..> MethodInfo : Хранит
    
```
