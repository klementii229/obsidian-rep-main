---
tags: 
изучил?: false
---
## Идентификация типа во время выполнения (RTTI)

Идентификация типа во время выполнения (RTTI) - это функция в C ++, которая позволяет вам получать информацию о типе объекта во время выполнения программы. Это может быть полезно при использовании динамической типизации, когда тип объекта может изменяться во время выполнения.

В C ++ существует два основных механизма для RTTI:

- `typeid` оператор
- `dynamic_cast` оператор

## оператор typeid

`typeid` это оператор, который возвращает ссылку на объект типа `std::type_info`, который содержит информацию о типе объекта. Файл заголовка `<typeinfo>` должен быть включен для использования `typeid`.

Вот пример:

```cpp
#include <iostream>
#include <typeinfo>

class Base { virtual void dummy() {} };
class Derived : public Base { /* ... */ };

int main() {
    Base* base_ptr = new Derived;

    // Using typeid to get the type of the object
    std::cout << "Type: " << typeid(*base_ptr).name() << '\n';

    delete base_ptr;
    return 0;
}
```

## оператор dynamic_cast

`dynamic_cast` это оператор приведения типов, который выполняет проверку типа во время выполнения и безопасно преобразует базовый указатель или ссылку в производный указатель или ссылку. Он возвращает null или выдает исключение bad_cast (если приведение ссылок), когда приведение завершается неудачей.

Вот пример:

```cpp
#include <iostream>

class Base { virtual void dummy() {} };
class Derived1 : public Base { /* ... */ };
class Derived2 : public Base { /* ... */ };

int main() {
    Base* base_ptr = new Derived1;

    // Using dynamic_cast to safely downcast the pointer
    Derived1* derived1_ptr = dynamic_cast<Derived1*>(base_ptr);
    if (derived1_ptr) {
        std::cout << "Downcast to Derived1 successful\n";
    }
    else {
        std::cout << "Downcast to Derived1 failed\n";
    }

    Derived2* derived2_ptr = dynamic_cast<Derived2*>(base_ptr);
    if (derived2_ptr) {
        std::cout << "Downcast to Derived2 successful\n";
    }
    else {
        std::cout << "Downcast to Derived2 failed\n";
    }

    delete base_ptr;
    return 0;
}
```

Пожалуйста, обратите внимание, что использование RTTI может привести к некоторым потерям производительности, поскольку это требует хранения и обработки дополнительной информации, генерируемой компилятором во время выполнения.