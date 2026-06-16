# Практика: Сбои

## 1. Описание предметной области и сущностей

Программа выводит список устройств, в которых до определенной даты случились критические сбои.

## 1. Диаграмма классов
```mermaid
classDiagram
    class Common {
        + IsFailureSerious(failureType: int) int
        + Earlier(v: object[], day: int, month: int, year: int) int$
    }

    class Device {
        + Id: int
        + Name: string
        + Device(id: int, name: string)
    }

    class Failure {
        + FailureType: int
        + Date: DateTime
        + Failure(failureType: int, date: DateTime)
        + IsFailureSerious() bool
    }

    class ReportMaker {
        + FindDevicesFailedBeforeDate(date: DateTime, failures: Failure[], deviceId: int[], devices: List~Device~) List~string~$
        + FindDevicesFailedBeforeDateObsolete(day: int, month: int, year: int, failureTypes: int[], deviceId: int[], times: object[][], devices: List~Dictionary~string, object~~) List~string~$
    }

    ReportMaker ..> Common : вызывает старый метод
    ReportMaker --> Device : Смотрит список устройств
    ReportMaker --> Failure : Обрабатывает список сбоев
    
```
