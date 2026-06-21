# Практика: Дифференцирование

## 1. Описание предметной области и сущностей

Было выполнено упражнение по реализации системы символьного (не численного) диф.-я мат.-х выражений при использовании LINQ Expressions.

## 2. Диаграмма классов (Mermaid)

```mermaid
classDiagram
    class Algebra {
        <<static>>
        -Dictionary~ExpressionType, Func~ OpRules\$
        -Dictionary~MethodInfo, Func~ FuncRules\(-MethodInfo SinMethod\)
        -MethodInfo CosMethod\(+Differentiate(Expression~Func~ function)\) Expression~Func~
        -Differentiate(Expression expr)\$ Expression
    }

    class Expression {
        <<System.Linq.Expressions>>
        +Add(left, right)\$
        +Multiply(left, right)\(+Call(method, arguments)\)
        +Constant(value)\(+Lambda(body, parameters)\)
    }

    class ConstantExpression {
        +Value object
    }

    class BinaryExpression {
        +Expression Left
        +Expression Right
    }

    class MethodCallExpression {
        +MethodInfo Method
        +ReadOnlyCollection~Expression~ Arguments
    }

    class ParameterExpression {
        +string Name
    }

    class MethodInfo {
        <<System.Reflection>>
    }

    Expression <|-- ConstantExpression : наследует
    Expression <|-- BinaryExpression : наследует
    Expression <|-- MethodCallExpression : наследует
    Expression <|-- ParameterExpression : наследует

    Algebra ..> Expression : Использует
    Algebra ..> MethodInfo : Хранит
```
