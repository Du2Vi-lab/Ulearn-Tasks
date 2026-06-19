# Практика: Дифференцирование

## 1. Описание предметной области и сущностей

Было выполнено упражнение по реализации системы символьного (не численного) диф.-я мат.-х выражений при использовании LINQ Expressions.

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
