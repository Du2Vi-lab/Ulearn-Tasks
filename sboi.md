# Практика: Сбои

## 1. Описание предметной области и сущностей

Программа выводит список устройств, в которых до определенной даты случились критические сбои.

Device — Устройство с параметрами ID и Name
Failure — нужен для регистрации факта поломки. Он хранит код типа ошибки (`FailureType`) и точное время, когда она случилась (`Date`). Также он сам умеет определять, является ли сбой серьезным (`IsFailureSerious()`).
Common — вспомогательный класс из старой версии программы. Он нужен для обратной совместимости, так как содержит старые функции для проверки критичности ошибок по их коду и для ручного сравнения дат по дням, месяцам и годам.
ReportMaker — центральный класс, который собирает всю логику вместе. Он нужен, чтобы принять новые списки объектов (`Device` и `Failure`), правильно перегнать их данные в старые форматы (массивы и словари) и отфильтровать названия тех устройств, которые сломались раньше указанного срока.
## 2. Диаграмма классов (Mermaid)

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

    ReportMaker ..> Common : вызывает старые методы
    ReportMaker ..> Device : использует список устройств
    ReportMaker ..> Failure : обрабатывает массив сбоев
    
```