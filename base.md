
# Общее ..........................
### ООП - парадигма программирования
> За много лет работы у программистов выработался определнный стиль (подход) к разработке ПО.
>И ООП это =>

`Объектно-ориентированное программирование`
-- это парадигма программирования, которая означает что порграмма представлена ввиде множества объектов
-- объекты являются экземплярами класса
-- классы образуют цепочку (иерархию) наследования


- `Инкапсуляция`
-- Защита данных из вне  protect => getters +setter => '_'
- `Наследование`
-- Создания новых классов на основе существующих
- `Полиморфизм`
-- способность разных классов обрабатывать данные по-разному имея один интерфейс
- `Абстракция`
-- Моделирование взаимодействий сущностей в виде абстрактных классов и интерфейсов для представления системы  

[Подробнее](https://habr.com/ru/post/463125/)

---
<!-- TOC --><a name="solid"></a>
### SOLID -  принципы ООП
- `Single Responsibility Principle (Принцип единственной ответственности)`
-- Класс должен отвечать только за что-то одно.
- `Open-Closed Principle (Принцип открытости-закрытости)`
-- Программные сущности должны быть открыты для расширения, но закрыты для модификации.
- `Liskov Substitution Principle (Принцип подстановки Барбары Лисков)`
-- Наследующий класс должен дополнять, а не замещать поведение базового класса.
- `Interface Segregation Principle (Принцип разделения интерфейса)`
-- Не должно имплеменитроваться то что не используется.
- `Dependency Inversion Principle (Принцип инверсии зависимостей)`
-- Модули верхних уровней не должны зависеть от модулей нижних уровней. 
-- Классы верхних, и нижних уровней должны зависеть от одних и тех же абстракций.

- `KISS (Keep It Simple, Stupid)`

--- 
<!-- TOC --><a name="dao-dto-vo-bo"></a>
### DAO, DTO, VO, BO - шаблоны проектирования
- `DAO (Data Access Object, объект доступа к данным)`
-- дает доступ к данным (sharedpref, repository)
-- как usecase или service

- `DTO (Data Transfer Object, объект переноса данных)`
-- это объект для передачи данных между слоями
-- происходит инкапсуляция данных
-- может включать методы серилизации (десерилизации)
-- не содержит бизнес логику

- `BO (Business Object, объект бизнеса)` 
-- взаимодействует с DAO (объект доступа к данным)

- `VO (Value Object, объект-значение)`
-- должен быть неизменяемым после создания
-- любые изменения значения должны приводить к созданию нового объекта
```Dart 
// Value Object
class Money {
  final double amount;
  final String currency;

  const Money(this.amount, this.currency);

  @override
  bool operator ==(Object other) =>
    other is Money && other.amount == this.amount && other.currency == this.currency;

  @override
  int get hashCode => amount.hashCode ^ currency.hashCode;

  Money add(Money other) {
    if (this.currency != other.currency) {
      throw Exception("Cannot add money of different currencies.");
    }
    return Money(this.amount + other.amount, this.currency);
  }
}
```


<!-- TOC --><a name="--21"></a>
## Паттерны разработки
`Порождающие` `Структурные` `Поведенческие`

`Порождающие`. Отвечают за удобное и безопасное создание новых объектов.
`Структурные`. Отвечают за построение удобных в поддержке иерархий классов.
`Поведенческие`. Решают задачи эффективного и безопасного взаимодействия между объектами программы.  


### Порождающие
`Отвечают за удобное и безопасное создание новых объектов`

- `Singleton`.  у класса есть только один экземпляр, и предоставляет к нему глобальную точку доступа.
- `Builder`.  позволяет создавать сложные объекты пошагово. Строитель даёт возможность использовать один и тот же код строительства для получения разных представлений объектов. PizzaBuilder Pizza
- `Prototype` (Прототип). копировать объекты, не вдаваясь в подробности их реализации. (abstrtact +  @override)

- `Factory Method (Фабричный Метод)`. паттерн который определяет общий интерфейс для создания объектов в суперклассе, позволяя подклассам изменять тип создаваемых объектов.
```dart
class SMSNotification implements Notification {
  @override
  void send(String message) {
    print("Sending SMS with message: $message");
  }
}

class NotificationFactory {
  static Notification createNotification(String type) {
    if (type == "email") {
      return EmailNotification();
    } else if (type == "sms") {
      return SMSNotification();
    } else {
      throw Exception('Notification type not supported.');
    }
  }
}

var emailNotification = NotificationFactory.createNotification("email");
emailNotification.send("Hello! This is an email.");
```
- `Abstract Factory` (Абстрактная Фабрика). Это когда у нас есть разные фабрики 

```Dart
abstract class OSFactory {
  Button createButton();
  Checkbox createCheckbox();
}

class WindowsFactory implements OSFactory {
  @override
  Button createButton() {
    return WindowsButton();
  }

  @override
  Checkbox createCheckbox() {
    return WindowsCheckbox();
  }
}

class MacOSFactory implements OSFactory {
  @override
  Button createButton() {
    return MacOSButton();
  }

  @override
  Checkbox createCheckbox() {
    return MacOSCheckbox();
  }

  void main {
    GUIFactory factory;

    String os = 'Windows';

    if (os == 'Windows') {
      factory = WindowsFactory();
    } else {
      factory = MacOSFactory();
    }
  }
}
```

### Структурные
`Отвечают за построение удобных в поддержке иерархий классов.`

- Bridge (Мост).  разделяет один или несколько классов на две отдельные иерархии — абстракцию и реализацию, позволяя изменять их независимо друг от друга.
- Decorator (Декоратор). Структурный паттерн проектирования, который позволяет динамически добавлять объектам новую функциональность, оборачивая их в полезные «обёртки».
- Adapter (Адаптер).  позволяет объектам с несовместимыми интерфейсами работать вместе.
обворачиваем старый класс новым Адаптером
```Dart
abstract class NewUserTarget {
  void processName(String name);
  void processEmail(String email);
}

class OldUserProcessor {
  void updateUserName(String name) {
    print("Updating user name to $name");
  }

  void updateUserEmail(String email) {
    print("Updating user email to $email");
  }
}

class UserAdapter implements NewUserTarget {
  final OldUserProcessor _oldUserProcessor;

  UserAdapter(this._oldUserProcessor);

  @override
  void processName(String name) {
    _oldUserProcessor.updateUserName(name);
  }

  @override
  void processEmail(String email) {
    _oldUserProcessor.updateUserEmail(email);
  }
}
```

### Поведенческие
`Решают задачи эффективного и безопасного взаимодействия между объектами программы`
- Observer (Наблюдатель). Поведенческий паттерн проектирования, который создаёт механизм подписки, позволяющий одним объектам следить и реагировать на события, происходящие в других объектах.
- State (Состояние). Поведенческий паттерн проектирования, который позволяет объектам менять поведение в зависимости от своего состояния. Извне создаётся впечатление, что изменился класс объекта.
- Iterator (Итератор). Поведенческий паттерн проектирования, который даёт возможность последовательно обходить элементы составных объектов, не раскрывая их внутреннего представления.

[Подробнее](https://github.com/scottt2/design-patterns-in-dart?tab=readme-ov-file) 
---

<!-- TOC --><a name="--1"></a>
### Императивное и декларативное программирование
- `Императивный стиль` - 
```Dart
function double (arr) {
  let results = []
  for (let i = 0; i < arr.length; i++){
    results.push(arr[i] * 2)
  }
  return results
}
```
- `Декларативный стиль` 
```Dart
function double (arr) {
  return arr.map((item) => item * 2)
}
```

---
<!-- TOC --><a name="di-service-locator"></a>
### DI и Service Locator
- `DI` - паттерн проектирования + Основная идея чтобы объекты не создавали зависимости сами а брали из вне. 
Например в классы мы передаем сервисы  и usecase и итераткоры и взаимодействуем с ними.

- `Service Locator` - патерн проектирования , основная идея в том , 
что это Class который содержет все зависимости приложения. Можно самому создать, а можно использовать GetIt.

Доступ к `Service Locator` может производиться из любого место в коде. В этом заключается его основной минус

---
 

