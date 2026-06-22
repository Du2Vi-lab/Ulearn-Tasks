# Практика: Сбои

## 1. Описание предметной области и сущностей

Программа выводит список устройств, в которых до определенной даты случились критические сбои.

## 2. Диаграмма классов
```mermaid
%%{init: {
  'theme': 'base',
  'themeVariables': {
    'primaryColor': '#E6E6FA',
    'primaryTextColor': '#000000',
    'primaryBorderColor': '#9370DB',
    'lineColor': '#666666',
    'secondaryColor': '#F5F5F5',
    'tertiaryColor': '#FFFFFF'
  }
}}%%
classDiagram
    class FailureType {
        UnexpectedShutdown = 0
        ShortNonResponding = 1
        HardwareFailures = 2
        ConnectionProblems = 3
    }

    class Common {
        +IsFailureSerious(failureType: int) int\$
        +Earlier(v: object[], day: int, month: int, year: int) int\$
    }

    class Device {
        +Id: int
        +Name: string
        +Device(id: int, name: string)
    }

    class Failure {
        +FailureType: int
        +Date: DateTime
        +Failure(failureType: int, date: DateTime)
        +IsFailureSerious() int
    }

    class ReportMaker {
        +FindDevicesFailedBeforeDate(date: DateTime, failures: Failure[], deviceId: int[], devices: List~Device~) List~string~\$
        +FindDevicesFailedBeforeDateObsolete(day: int, month: int, year: int, failureTypes: int[], deviceId: int[], times: object[][], devices: List~Dictionary~string, object~~) List~string~\$
    }

    ReportMaker ..> Common : вызывает устаревший метод
    ReportMaker ..> Device : обрабатывает сущности устройств
    ReportMaker ..> Failure : обрабатывает сущности сбоев
    Failure ..> FailureType : использует для классификации
```
