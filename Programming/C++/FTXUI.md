# FTXUI Шпаргалка: Компоненты vs Элементы

## **КОМПОНЕНТЫ (Components)** — `ftxui::Component`
```cpp
// === ИНТЕРАКТИВНЫЕ (с обработкой ввода) ===
auto input = Input(&text, "placeholder");    // Поле ввода
auto btn = Button("Текст", []{...});         // Кнопка  
auto checkbox = Checkbox("Метка", &checked); // Чекбокс
auto radiobox = Radiobox({opt1,opt2}, &selected); // Радиокнопки
auto menu = Menu({"Пункт1","Пункт2"}, &selected); // Меню
auto slider = Slider("Метка:", &value, 0, 100); // Слайдер
auto dropdown = Dropdown(&options, &selected); // Выпадающий список

// === КОНТЕЙНЕРЫ (управление фокусом) ===
auto vert = Container::Vertical({comp1, comp2}); // Вертикальный (Tab/Shift+Tab)
auto horiz = Container::Horizontal({comp1, comp2}); // Горизонтальный
auto tabs = Container::Tab({tab1, tab2}, &tab_index); // Вкладки
```

## **ЭЛЕМЕНТЫ (Elements)** — `ftxui::Element`
```cpp
// === ОТОБРАЖЕНИЕ ===
auto txt = text("Текст");                 // Простой текст
auto para = paragraph("Длинный\nтекст");  // Текст с переносами
auto sep = separator();                   // Разделитель ────
auto sep_light = separatorLight();        // Легкий разделитель
auto filler_el = filler();                // Заполнитель пространства
auto gauge_el = gauge(0.75);              // Прогресс-бар [█████░░░]

// === ГРАФИКА ===
auto canvas = Canvas(width, height);      // Холст для рисования
canvas.DrawPoint(x, y, true);             // Точка на canvas
canvas.DrawText(x, y, "text");            // Текст на canvas

// === КОМПОНОВКА ===
auto vbox_el = vbox({elem1, elem2});      // Вертикальный стек
auto hbox_el = hbox({elem1, elem2});      // Горизонтальный ряд
auto grid = gridbox({...});               // Таблица/сетка
```

## 🎭 **ДЕКОРАТОРЫ (Decorators)** — `| оператор`
```cpp
// === РАМКИ ===
elem | border;                    // ┌─┐
elem | doubleBorder;              // ╔═╗  
elem | roundedBorder;             // ╭─╮
elem | hevyBorder;                // ┏━┓
elem | borderStyled(DOUBLE);      // Стилизованная

// === ВЫРАВНИВАНИЕ ===
elem | center;                    // По центру
elem | align_right;               // Вправо  
elem | align_center;              // Горизонтально по центру
elem | vcenter;                   // Вертикально по центру

// === ЦВЕТА ===
elem | color(Color::Red);         // Цвет текста
elem | bgcolor(Color::Blue);      // Цвет фона
elem | bold;                      // Жирный
elem | dim;                       // Тусклый
elem | inverted;                  // Инвертировать цвета

// === РАЗМЕРЫ ===
elem | size(WIDTH, EQUAL, 20);    // Фиксированная ширина
elem | size(HEIGHT, EQUAL, 10);   // Фиксированная высота
elem | flex;                      // Растягивается
elem | flex_grow;                 // Занимает всё доступное
elem | notflex;                   // Не растягивается
```

## 🔄 **ПРЕОБРАЗОВАНИЯ**

### **Компонент → Элемент**
```cpp
ftxui::Component comp = Button("Click", []{});
ftxui::Element elem = comp->Render();  // Обязательно для декораторов!
```

### **Renderer: Компонент + Элементы**
```cpp
auto component = Container::Vertical({btn1, btn2});  // Компонент
auto decorated = Renderer(component, [=] {          // Component!
    return vbox({                                   // Element внутри!
        text("Заголовок"),
        hbox({
            btn1->Render() | border,               // Компонент → Элемент
            filler(),
            btn2->Render() | color(Color::Green)
        })
    }) | border;
});
```

## 🎮 **ЭКРАН И ЗАПУСК**
```cpp
auto screen = ScreenInteractive::Fullscreen();  // Полноэкранный режим
auto screen = ScreenInteractive::TerminalOutput(); // Только вывод
auto screen = ScreenInteractive::FixedSize(width, height); // Фиксированный

screen.Loop(component);  // Запуск главного цикла
screen.Exit();           // Выход из цикла
```

## ⚡ **БЫСТРЫЕ РЕЦЕПТЫ**

### Кнопка с действием:
```cpp
auto btn = Button("Сохранить", []{ saveData(); });

// С доступом к членам класса:
auto btn = Button("Выйти", [this] { screen.Exit(); });
```

### Поле ввода с переменной:
```cpp
std::string text;
auto input = Input(&text, "Введите запрос...");
```

### Вертикальная форма:
```cpp
auto form = Container::Vertical({
    Input(&name, "Имя"),
    Input(&email, "Email"),
    Button("Отправить", submit)
});
```

### Горизонтальная панель:
```cpp
auto toolbar = Container::Horizontal({
    Button("Новый", newFile),
    Button("Открыть", openFile),
    Button("Сохранить", saveFile)
});
```

## **ВАЖНЫЕ ПРАВИЛА:**
1. **Компоненты** → для интерактивности (ввод, фокус)
2. **Элементы** → только отображение (декораторы применяются к ним)
3. **`Container::Vertical/Horizontal`** → управление фокусом (Tab/Shift+Tab)
4. **`vbox/hbox`** → только компоновка элементов (без фокуса)
5. **Всегда**: `comp->Render()` перед использованием в элементах

