# Практика: TaxiOrder

## 1. Описание предметной области и сущностей

Система управления заказами такси построена на принципах DDD. В рамках задания выполнена переработка анемичной модели в богатую (Rich) доменную модель. Вся бизнес-логика и проверки инкапсулированы внутри доменных объектов, а технические детали и работа с данными вынесены в инфраструктурные компоненты.

## 2. Диаграмма классов (Mermaid)

```mermaid
classDiagram
    direction TB

    namespace Infrastructure {
        class Entity_T_ {
            <<abstract>>
            +T Id
            +Equals(object obj)* bool
            +GetHashCode()* int
        }
        class ValueType_T_ {
            <<abstract>>
            +Equals(object obj) bool
            +GetHashCode() int
            +ToString() string
        }
    }

    namespace Domain {
        class TaxiOrder {
            +PersonName ClientName
            +Address Start
            +Address Destination
            +Driver Driver
            +TaxiOrderStatus Status
            +DateTime CreationTime
            +DateTime DriverAssignmentTime
            +DateTime CancelTime
            +DateTime StartRideTime
            +DateTime FinishRideTime
            +TaxiOrder(int id, PersonName clientName, Address start, DateTime creationTime)
            +UpdateDestination(Address destination) void
            +AssignDriver(Driver driver, DateTime time) void
            +UnassignDriver() void
            +Cancel(DateTime time) void
            +StartRide(DateTime time) void
            +FinishRide(DateTime time) void
        }

        class Driver {
            +PersonName Name
            +Car Car
            +Driver(int id, PersonName name, string carColor, string carModel, string carPlateNumber)
        }

        class Car {
            +string Color
            +string Model
            +string PlateNumber
            +Car(string color, string model, string plateNumber)
        }

        class PersonName {
            +string FirstName
            +string LastName
        }

        class Address {
            +string Street
            +string Building
        }

        class DriversRepository {
            +GetDriver(int driverId) Driver
        }

        class TaxiApi {
            -DriversRepository driversRepo
            -Func~DateTime~ currentTime
            -int idCounter
            +TaxiApi(DriversRepository driversRepo, Func~DateTime~ currentTime)
            +CreateOrderWithoutDestination(...) TaxiOrder
            +UpdateDestination(TaxiOrder order, ...) void
            +AssignDriver(TaxiOrder order, int driverId) void
            +UnassignDriver(TaxiOrder order) void
            +GetDriverFullInfo(TaxiOrder order) string
            +GetShortOrderInfo(TaxiOrder order) string
            +Cancel(TaxiOrder order) void
            +StartRide(TaxiOrder order) void
            +FinishRide(TaxiOrder order) void
        }
    }

    Entity_T_ <|-- TaxiOrder : наследует
    Entity_T_ <|-- Driver : наследует
    ValueType_T_ <|-- Car : наследует

    TaxiOrder "1" *-- "1" PersonName : ClientName
    TaxiOrder "1" *-- "1" Address : Start
    TaxiOrder "1" *-- "0..1" Address : Destination
    TaxiOrder "1" --> "0..1" Driver : Driver

    Driver "1" *-- "1" PersonName : Name
    Driver "1" *-- "1" Car : Car

    TaxiApi ..> TaxiOrder : управляет процессами
    TaxiApi --> DriversRepository : запрашивает данные
    DriversRepository ..> Driver : поставляет доменные сущности
```
