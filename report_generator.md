# Практика: Генератор отчетов

## 1. Описание предметной области и сущностей

В ходе рефакторинга логика формирования отчетов была заменена на делегирование через функциональные типы. Это позволило передавать правила оформления текста и методы расчета статистики в качестве параметров, разделив логику генерации структуры отчета и логику его конкретного наполнения.

---

## 2. Диаграмма классов (Mermaid)

```mermaid
classDiagram
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

    class ReportFormatter {
        +HtmlMakeCaption(string caption) string
        +HtmlBeginList() string
        +HtmlEndList() string
        +HtmlMakeItem(string valueType, string entry) string
        +MdMakeCaption(string caption) string
        +MdBeginList() string
        +MdEndList() string
        +MdMakeItem(string valueType, string entry) string
    }

    class ReportCalculator {
        +MeanAndStdStats(IEnumerable~double~ rawData) object
        +MedianStats(IEnumerable~double~ rawData) object
    }

    class ReportMakerHelper {
        <<static>>
        +MeanAndStdHtmlReport(IEnumerable~Measurement~ data)\$ string
        +MedianMarkdownReport(IEnumerable~Measurement~ data)\$ string
        +MeanAndStdMarkdownReport(IEnumerable~Measurement~ measurements)\$ string
        +MedianHtmlReport(IEnumerable~Measurement~ measurements)\$ string
    }

    class Measurement {
        +double Temperature
        +double Humidity
    }

    class MeanAndStd {
        +double Mean
        +double Std
    }

    ReportMakerHelper ..> ReportMaker : создает
    ReportMakerHelper ..> ReportFormatter : использует
    ReportMakerHelper ..> ReportCalculator : использует
    ReportMaker ..> Measurement : читает
```
