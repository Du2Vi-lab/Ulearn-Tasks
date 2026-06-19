# Практика: Генератор отчетов

## 1. Описание предметной области и сущностей

(заполнить)

## 2. Диаграмма классов (Mermaid)

```mermaid

    class ReportMaker {
        -Func~string, string~ MakeCaption
        -Func~string~ BeginList
        -Func~string, string, string~ MakeItem
        -Func~string~ EndList
        -Func~IEnumerable~double~, object~ MakeStatistics
        -string Caption
        +ReportMaker(string Caption, Func~string, string~ MakeCaption, ...)
        +MakeReport(IEnumerable~Measurement~ measurements) string
    }

    class ReportMakerHelper {
        <<static>>
        -HtmlMakeCaption(string caption)* string
        -HtmlBeginList()* string
        -HtmlEndList()* string
        -HtmlMakeItem(string valueType, string entry)* string
        -MdMakeCaption(string caption)* string
        -MdBeginList()* string
        -MdEndList()* string
        -MdMakeItem(string valueType, string entry)* string
        -MeanAndStdStats(IEnumerable~double~ rawData)* object
        -MedianStats(IEnumerable~double~ rawData)* object
        +MeanAndStdHtmlReport(IEnumerable~Measurement~ data)$ string
        +MedianMarkdownReport(IEnumerable~Measurement~ data)$ string
        +MeanAndStdMarkdownReport(IEnumerable~Measurement~ measurements)$ string
        +MedianHtmlReport(IEnumerable~Measurement~ measurements)$ string
    }

    class Measurement {
        <<struct/class>>
        +double Temperature
        +double Humidity
    }

    class MeanAndStd {
        <<struct/class>>
        +double Mean
        +double Std
    }

    ReportMakerHelper ..> ReportMaker : Создает
    ReportMaker ..> Measurement : Читает
    ReportMakerHelper ..> Measurement : Принимает
    ReportMakerHelper ..> MeanAndStd : Возвращает
    
```
