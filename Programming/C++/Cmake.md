Содержание
1. [[#Команды]]
2. [[#Аргументы]]
3. [[#Комментарии]]
4. 

[Статья](https://telegra.ph/Polnoe-rukovodstvo-po-CMake-CHast-pervaya-Sintaksis-02-27)
[https://telegra.ph/Polnoe-rukovodstvo-po-CMake-CHast-tretya-Testirovanie-i-paketirovanie-02-27](https://telegra.ph/Polnoe-rukovodstvo-po-CMake-CHast-tretya-Testirovanie-i-paketirovanie-02-27 "https://telegra.ph/Polnoe-rukovodstvo-po-CMake-CHast-tretya-Testirovanie-i-paketirovanie-02-27")


### Команды
1. ```
```cmake
message("CMake " "is " "the " "most " "powerful " "buildsystem!")
```


#### Аргументы

Аргументы, обрамлённые в двойные кавычки, позволяют внутри себя совершать экранирование и подстановку переменных. Необрамлённые аргументы не позволяют производить подобных вещей и не могут включать в себя символы `()#"\` и пробелы, однако более удобны для использования. Пример:

  
#### Напечатает "Hello, my lovely CMake", один таб и "!":
```cmake
message("Hello, my lovely CMake\t!")
```

#### Комментарии

Комментарии начинаются с символа решётки и заканчиваются в конце той строки, где они были напечатаны. Текст, заключённый в комментариях, игнорируется системой сборки и не оказывает никакого эффекта на её работу. Примеры выше также демонстрируют использование комментариев.


#### Переменные
Переменные можно определить путём вызова команды [`set`](https://web.archive.org/web/20220528140418/https://cmake.org/cmake/help/latest/command/set.html), а удалить вызовом [`unset`](https://web.archive.org/web/20220528140418/https://cmake.org/cmake/help/latest/command/unset.html). Получить значение переменной можно по конструкции `${VARIABLE}`. Если переменная ещё не определена и где-то потребовалось получить её значение, то данная переменная обратится в пустую строку. Пример:

  
```cmake

Определить переменную VARIABLE со значением "Mr. Thomas":
set(VARIABLE "Mr. Thomas")

 Напечает "His name is: Mr. Thomas":
message("His name is: " ${VARIABLE})

 Напечатает "'BINGO' is equal to: []", так как "BINGO" не определена:
message("'BINGO' is equal to: [${BINGO}]")

 Удалить переменную VARIABLE:
unset(VARIABLE)
```



#### Опции

CMake поддерживает задание опций, подлежащих модицификациям пользователей. Опции похожи на переменные и задаются командой [`option`](https://web.archive.org/web/20220528140418/https://cmake.org/cmake/help/latest/command/option.html), принимающей всего три аргумента: имя переменной, строковое описание переменной и значение переменной по умолчанию (`ON` или `OFF`):

```cmake
Задать опцию `USE_ANOTHER_LIBRARY` с описанием
 "Do you want to use an another library?" и значением "OFF":
option(USE_ANOTHER_LIBRARY "Do you want to use an another library?" OFF)
```


#### Логические выражения

Прежде чем приступать к изучению условных операторов и циклических конструкций, необходимо понимать работу логических выражений. Логические выражения используются при проверки условий и могут принимать одно из двух значений: правда или ложь. Например, выражение `52 LESS 58` обратится в правду, так как 52 < 58. Выражение `88 EQUAL 88` обратится в правду, `63 GREATER 104` обратится в ложь. Сравнивать можно не только числа, но и строки, версии, файлы, принадлежность к списку и регулярные выражения. Полный список логических выражений можно посмотреть [тут](https://web.archive.org/web/20220528140418/https://cmake.org/cmake/help/latest/command/if.html#command:if).

#### Условные операторы

Условные операторы в CMake работают в точности как в других языках программирования. В данном примере сработает лишь первый условный оператор, который проверяет, что 5 > 1. Второе и третье условия ложны, так как 5 не может быть меньше или равняться одному. Блоки команд [`elseif`](https://web.archive.org/web/20220528140418/https://cmake.org/cmake/help/latest/command/elseif.html) и [`else`](https://web.archive.org/web/20220528140418/https://cmake.org/cmake/help/latest/command/else.html) необязательны, а [`endif`](https://web.archive.org/web/20220528140418/https://cmake.org/cmake/help/latest/command/endif.html) обязательна и сигнализирует о завершении предыдущих проверок.

  ```cmake
 Напечатает "Of course, 5 > 1!":
if(5 GREATER 1)
    message("Of course, 5 > 1!")
elseif(5 LESS 1)
    message("Oh no, 5 < 1!")
else()
    message("Oh my god, 5 == 1!")
endif()
```

#### Циклы

Циклы в CMake подобны циклам других языков программирования. В приведённом примере устанавливается значение переменной `VARIABLE` в `Airport`, а затем четыре вложенные команды последовательно исполняются пока значение переменной `VARIABLE` будет равняться `Airport`. Последняя четвёртая команда `set(VARIABLE "Police station")` устанавливает значение проверяемой переменной в `Police station`, поэтому цикл сразу остановится, не дойдя до второй итерации. Команда [`endwhile`](https://web.archive.org/web/20220528140418/https://cmake.org/cmake/help/latest/command/endwhile.html) сигнализирует о завершении списка вложенных в цикл команд.

  
```cmake
Напечатает в консоль три раза "VARIABLE is still 'Airport'":
set(VARIABLE Airport)
while(${VARIABLE} STREQUAL Airport)
    message("VARIABLE is still '${VARIABLE}'")
    message("VARIABLE is still '${VARIABLE}'")
    message("VARIABLE is still '${VARIABLE}'")
    set(VARIABLE "Police station")
endwhile()
```
  

Данный пример цикла [`foreach`](https://web.archive.org/web/20220528140418/https://cmake.org/cmake/help/latest/command/foreach.html) работает следующим образом: на каждой итерации данного цикла переменной `VARIABLE` присваивается следующее значение из списка `Give me the sugar please!`, а затем исполняется команда `message(${VARIABLE})`, которая выводит текущее значение переменной `VARIABLE`. Когда значений в списке не остаётся, то цикл завершает своё выполнение. Команда [`endforeach`](https://web.archive.org/web/20220528140418/https://cmake.org/cmake/help/latest/command/endforeach.html) сигнализирует о завершении списка вложенных в цикл команд.

  
```cmake
# Напечатает "Give me the sugar please!" с новых строк:
foreach(VARIABLE Give me the sugar please!)
    message(${VARIABLE})
endforeach()


Существуют ещё 3 формы записи цикла [`foreach`](https://web.archive.org/web/20220528140418/https://cmake.org/cmake/help/latest/command/foreach.html). Первый цикл в данном примере на место списка генерирует целые числа от 0 до 10, второй цикл генерирует в диапазоне от 3 до 15, а третий цикл работает в сегменте от 50 до 90, но с шагом 10.
```
  

```cmake
Напечатает "0 1 2 3 4 5 6 7 8 9 10" с новых строк:
foreach(VARIABLE RANGE 10)
    message(${VARIABLE})
endforeach()

# Напечатает "3 4 5 6 7 8 9 10 11 12 13 14 15" с новых строк:
foreach(VARIABLE RANGE 3 15)
    message(${VARIABLE})
endforeach()

# Напечатает "50 60 70 80 90" с новых строк:
foreach(VARIABLE RANGE 50 90 10)
    message(${VARIABLE})
endforeach()
```


#### Функции и макросы

Синтаксис CMake позволяет определять собственные команды, которые можно будет вызывать в точности как встроенные. Приведённый ниже пример демонстрирует использование функций и макросов: сначала определяются функция и макрос со своими собственными командами, а при их вызове их команды исполняются последовательно.

  

# Определение функции "print_numbers":
function(print_numbers NUM1 NUM2 NUM3)
    message(${NUM1} " " ${NUM2} " " ${NUM3})
endfunction()

# Определение макроса "print_words":
macro(print_words WORD1 WORD2 WORD3)
    message(${WORD1} " "  ${WORD2} " " ${WORD3})
endmacro()

# Вызов функции "print_numbers", которая напечатает "12 89 225":
print_numbers(12 89 225)

# Вызов макроса "print_words", который напечатает "Hey Hello Goodbye":
print_words(Hey Hello Goodbye)

  

Команда [`function`](https://web.archive.org/web/20220528140418/https://cmake.org/cmake/help/latest/command/function.html) первым аргументов принимает имя будущей функции, а остальные аргументы — это имена параметров, с которыми можно работать как с обычными переменными. Параметры видимы лишь определяемой функции, значит вне функции доступ к её параметрам мы получить не можем. Более того, все другие переменные, определяемые и переопределяемые внутри функции, видны лишь ей самой.

  

Макросы аналогичны функциям за тем исключением, что они не имеют собственной области видимости: все переменные внутри макросов рассматриваются как глобальные. Более подробно о различиях макросов и функций Вы можете почитать [здесь](https://web.archive.org/web/20220528140418/https://stackoverflow.com/questions/24297999/function-vs-macro-in-cmake).

  

Как [отметили](https://web.archive.org/web/20220528140418/https://habr.com/post/431428/#comment_19428914) в комментариях, макросы в CMake подобны макросам в препроцессоре языка Си: если в тело макроса поместить команду [`return`](https://web.archive.org/web/20220528140418/https://cmake.org/cmake/help/latest/command/return.html), то произойдёт выход из вызвавшей функции (или из всего скрипта), что демонстрирует данный пример:

  

# Определить макрос, содержащий команду выхода:
macro(demonstrate_macro)
    return()
endmacro()

# Определить функцию, вызывающую предыдущий макрос:
function(demonstrate_func)
    demonstrate_macro()
    message("The function was invoked!")
endfunction()

# Напечатает "Something happened with the function!"
demonstrate_func()
message("Something happened with the function!")

  

В приведённом выше примере функция `demonstrate_func` не успеет напечатать сообщение `The function was invoked!`, так как прежде, на место вызова макроса `demonstrate_macro` будет подставлена и выполнена команда выхода.

  

#### Разбор аргументов

Как [отметили](https://web.archive.org/web/20220528140418/https://habr.com/post/432096/#comment_19455074) в комментариях, Мощный механизм [`cmake_parse_arguments`](https://web.archive.org/web/20220528140418/https://cmake.org/cmake/help/latest/command/cmake_parse_arguments.html) позволяет производить разбор аргументов, переданных в функцию или макрос.

  

Данная команда принимает префикс, используемый в определении переменных (смотреть следующий абзац), список опций, используемых без последующих значений, список ключевых слов, после которых следует одно значение, список ключевых слов, после которых следуют множества значений и список всех значений, переданных в функцию или макрос.

  

Работа механизма разбора аргументов заключается в преобразовании полученных аргументов в значения переменных. Таким образом, рассмотренная команда для каждой опции и ключевого слова определяет собственную переменную вида `<Prefix>_<OptionOrKeyword>`, инкапсулирующую некоторое значение. Для опций — это булевы значения (истина — опция указана, иначе — ложь), а для ключевых слов — все переданные значения, расположенные после них.

  

Функция `custom_function` содержит вызов команды [`cmake_parse_arguments`](https://web.archive.org/web/20220528140418/https://cmake.org/cmake/help/latest/command/cmake_parse_arguments.html), а затем команды печати значений определённых переменных. Далее, функция вызывается с аргументами `LOW NUMBER 30 COLORS red green blue`, после чего производится печать на экран:

  

function(custom_function)
    # Вызвать механизм обработки аргументов для текущей функции:
    cmake_parse_arguments(CUSTOM_FUNCTION "LOW;HIGH" "NUMBER" "COLORS" ${ARGV})

    # Напечатает "'LOW' = [TRUE]":
    message("'LOW' = [${CUSTOM_FUNCTION_LOW}]")
    #Напечатает "'HIGH' = [FALSE]":
    message("'HIGH' = [${CUSTOM_FUNCTION_HIGH}]")
    # Напечатает "'NUMBER' = [30]":
    message("'NUMBER' = [${CUSTOM_FUNCTION_NUMBER}]")
    # Напечатает "'COLORS' = [red;green;blue]":
    message("'COLORS' = [${CUSTOM_FUNCTION_COLORS}]")
endfunction()

# Вызвать функцию "custom_function" с произвольными аргументами:
custom_function(LOW NUMBER 30 COLORS red green blue)

  

#### Области видимости

В предыдущем разделе Вы узнали о том, что некоторые конструкции в CMake могут определять собственные области видимости. На самом деле, все переменные по умолчанию считаются глобальными (доступ к ним есть везде), за исключением тех, которые были определены и переопределены в функциях. Также имеются [кэш-переменные](https://web.archive.org/web/20220528140418/https://cmake.org/cmake/help/latest/command/set.html#set-cache-entry), у которых своя собственная область видимости, но они применяются не столь часто.

  

Как [упомянули](https://web.archive.org/web/20220528140418/https://habr.com/post/431428/#comment_19428914) в комментариях, переменные можно определять в "родительской" области видимости с помощью команды `set(VARIABLE ... PARENT_SCOPE)`. Данный пример демонстрирует эту особенность:

  

# Функция, определяющая переменную "VARIABLE" со значением
# "In the parent scope..." в родительской области видимости:
function(demonstrate_variable)
    set(VARIABLE "In the parent scope..." PARENT_SCOPE)
endfunction()

# Определить переменную "VARIABLE" в текущей области видимости:
demonstrate_variable()

# Теперь возможно получить к переменной "VARIABLE" доступ:
message("'VARIABLE' is equal to: ${VARIABLE}")

  

Если из определения переменной `VARIABLE` убрать `PARENT_SCOPE`, то переменная будет доступна лишь функции `demonstrate_variable`, а в глобальной области видимости она примет пустое значение.

  

#### Запуск скриптовых файлов

Команда [`include`](https://web.archive.org/web/20220528140444/https://cmake.org/cmake/help/latest/command/include.html) заменяет строку своего вызова кодом заданного файла, действуя аналогично препроцессорной команде `include` языков C/C++. Этот пример запускает скриптовый файл `MyCMakeScript.cmake` описанной командой:

  

message("'TEST_VARIABLE' is equal to [${TEST_VARIABLE}]")

#### Запустить скрипт MyCMakeScript.cmake на выполнение:
```cmake
include(MyCMakeScript.cmake)

message("'TEST_VARIABLE' is equal to [${TEST_VARIABLE}]")
```





#### Компиляция исполняемых файлов

Команда [`add_executable`](https://web.archive.org/web/20220528140444/https://cmake.org/cmake/help/latest/command/add_executable.html) компилирует исполняемый файл с заданным именем из списка исходников. Важно отметить, что окончательное имя файла зависит от целевой платформы (например, `<ExecutableName>.exe` или просто `<ExecutableName>`). Типичный пример вызова данной команды:

  

```cmake
Скомпилировать исполняемый файл "MyExecutable" из
# исходников "ObjectHandler.c", "TimeManager.c" и "MessageGenerator.c":
add_executable(MyExecutable ObjectHandler.c TimeManager.c MessageGenerator.c)

  

#### Компиляция библиотек

Команда [`add_library`](https://web.archive.org/web/20220528140444/https://cmake.org/cmake/help/latest/command/add_library.html) компилирует библиотеку с указанным видом и именем из исходников. Важно отметить, что окончательное имя библиотеки зависит от целевой платформы (например, `lib<LibraryName>.a` или `<LibraryName>.lib`). Типичный пример вызова данной команды:

  

# Скомпилировать статическую библиотеку "MyLibrary" из
# исходников "ObjectHandler.c", "TimeManager.c" и "MessageConsumer.c":
add_library(MyLibrary STATIC ObjectHandler.c TimeManager.c MessageConsumer.c)

  

- Статические библиотеки задаются ключевым словом `STATIC` вторым аргументом и представляют из себя архивы объектных файлов, связываемых с исполняемыми файлами и другими библиотеками во время компиляции;
- Динамические библиотеки задаются ключевым словом `SHARED` вторым аргументом и представляют из себя двоичные библиотеки, загружаемые операционной системой во время выполнения программы;
- Модульные библиотеки задаются ключевым словом `MODULE` вторым аргументом и представляют из себя двоичные библиотеки, загружаемые посредством техник выполнения самим исполняемым файлом;
- Объектные библиотеки задаются ключевым словом `OBJECT` вторым аргументом и представляют из себя набор объектных файлов, связываемых с исполняемыми файлами и другими библиотеками во время компиляции.
```

#### Добавление исходников к цели

Бывают случаи, требующие многократного добавления исходных файлов к цели. Для этого предусмотрена команда [`target_sources`](https://web.archive.org/web/20220528140444/https://cmake.org/cmake/help/latest/command/target_sources.html), способная добавлять исходники к цели множество раз.


Первым аргументом команда [`target_sources`](https://web.archive.org/web/20220528140444/https://cmake.org/cmake/help/latest/command/target_sources.html) принимает название цели, ранее указанной с помощью команд [`add_library`](https://web.archive.org/web/20220528140444/https://cmake.org/cmake/help/latest/command/add_library.html) или [`add_executable`](https://web.archive.org/web/20220528140444/https://cmake.org/cmake/help/latest/command/add_executable.html), а последующие аргументы являются списком добавляемых исходных файлов.
  
Повторяющиеся вызовы команды [`target_sources`](https://web.archive.org/web/20220528140444/https://cmake.org/cmake/help/latest/command/target_sources.html) добавляют исходные файлы к цели в том порядке, в каком они были вызваны, поэтому нижние два блока кода являются функционально эквивалентными:

  ```cmake
# Задать исполняемый файл "MyExecutable" из исходников
# "ObjectPrinter.c" и "SystemEvaluator.c":
add_executable(MyExecutable ObjectPrinter.c SystemEvaluator.c)

# Добавить к цели "MyExecutable" исходник "MessageConsumer.c":
target_sources(MyExecutable MessageConsumer.c)
# Добавить к цели "MyExecutable" исходник "ResultHandler.c":
target_sources(MyExecutable ResultHandler.c)

  

# Задать исполняемый файл "MyExecutable" из исходников
# "ObjectPrinter.c", "SystemEvaluator.c", "MessageConsumer.c" и "ResultHandler.c":
add_executable(MyExecutable ObjectPrinter.c SystemEvaluator.c MessageConsumer.c
ResultHandler.c)

  

#### Генерируемые файлы

Местоположение выходных файлов, сгенерированных командами [`add_executable`](https://web.archive.org/web/20220528140444/https://cmake.org/cmake/help/latest/command/add_executable.html) и [`add_library`](https://web.archive.org/web/20220528140444/https://cmake.org/cmake/help/latest/command/add_library.html), определяется только на стадии генерации, однако данное правило можно изменить несколькими переменными, определяющими конечное местоположение двоичных файлов:

  

- Переменные [`RUNTIME_OUTPUT_DIRECTORY`](https://web.archive.org/web/20220528140444/https://cmake.org/cmake/help/latest/prop_tgt/RUNTIME_OUTPUT_DIRECTORY.html#prop_tgt:RUNTIME_OUTPUT_DIRECTORY) и [`RUNTIME_OUTPUT_NAME`](https://web.archive.org/web/20220528140444/https://cmake.org/cmake/help/latest/prop_tgt/RUNTIME_OUTPUT_NAME.html#prop_tgt:RUNTIME_OUTPUT_NAME) определяют местоположение целей выполнения;
- Переменные [`LIBRARY_OUTPUT_DIRECTORY`](https://web.archive.org/web/20220528140444/https://cmake.org/cmake/help/latest/prop_tgt/LIBRARY_OUTPUT_DIRECTORY.html#prop_tgt:LIBRARY_OUTPUT_DIRECTORY) и [`LIBRARY_OUTPUT_NAME`](https://web.archive.org/web/20220528140444/https://cmake.org/cmake/help/latest/prop_tgt/LIBRARY_OUTPUT_NAME.html#prop_tgt:LIBRARY_OUTPUT_NAME) определяют местоположение библиотек;
- Переменные [`ARCHIVE_OUTPUT_DIRECTORY`](https://web.archive.org/web/20220528140444/https://cmake.org/cmake/help/latest/prop_tgt/ARCHIVE_OUTPUT_DIRECTORY.html#prop_tgt:ARCHIVE_OUTPUT_DIRECTORY) и [`ARCHIVE_OUTPUT_NAME`](https://web.archive.org/web/20220528140444/https://cmake.org/cmake/help/latest/prop_tgt/ARCHIVE_OUTPUT_NAME.html#prop_tgt:ARCHIVE_OUTPUT_NAME) определяют местоположение архивов.

  

Исполняемые файлы всегда рассматриваются целями выполнения, статические библиотеки — архивными целями, а модульные библиотеки — библиотечными целями. Для "не-DLL" платформ динамические библиотеки рассматриваются библиотечными целями, а для "DLL-платформ" — целями выполнения. Для объектных библиотек таких переменных не предусмотрено, поскольку такой вид библиотек генерируется в недрах каталога `CMakeFiles`.

  

Важно подметить, что "DLL-платформами" считаются все платформы, основанные на Windows, в том числе и [Cygwin](https://web.archive.org/web/20220528140444/https://cygwin.com/).
```
#### Компоновка с библиотеками

Команда [`target_link_libraries`](https://web.archive.org/web/20220528140444/https://cmake.org/cmake/help/latest/command/target_link_libraries.html) компонует библиотеку или исполняемый файл с другими предоставляемыми библиотеками. Первым аргументом данная команда принимает название цели, сгенерированной с помощью команд [`add_executable`](https://web.archive.org/web/20220528140444/https://cmake.org/cmake/help/latest/command/add_executable.html) или [`add_library`](https://web.archive.org/web/20220528140444/https://cmake.org/cmake/help/latest/command/add_library.html), а последующие аргументы представляют собой названия целей библиотек или полные пути к библиотекам. Пример:

  

# Скомпоновать исполняемый файл "MyExecutable" с
# библиотеками "JsonParser", "SocketFactory" и "BrowserInvoker":
target_link_libraries(MyExecutable JsonParser SocketFactory BrowserInvoker)

  

Стоит отметить, что модульные библиотеки не подлежат компоновке с исполняемыми файлами или другими библиотеками, так как они предназначены исключительно для загрузки техниками выполнения.

  

#### Работа с целями

Как [упомянули](https://web.archive.org/web/20220528140444/https://habr.com/post/432096/#comment_19455088) в комментариях, цели в CMake тоже подвержены ручному манипулированию, однако весьма ограниченному.

  

Имеется возможность управления свойствами целей, предназначенных для задания процесса сборки проекта. Команда [`get_target_property`](https://web.archive.org/web/20220528140444/https://cmake.org/cmake/help/latest/command/get_target_property.html) присваивает предоставленной переменной значение свойства цели. Данный пример выводит значение свойства [`C_STANDARD`](https://web.archive.org/web/20220528140444/https://cmake.org/cmake/help/latest/prop_tgt/C_STANDARD.html) цели `MyTarget` на экран:

  

# Присвоить переменной "VALUE" значение свойства "C_STANDARD":
get_target_property(VALUE MyTarget C_STANDARD)

# Вывести значение полученного свойства на экран:
message("'C_STANDARD' property is equal to [${VALUE}]")

  

Команда [`set_target_properties`](https://web.archive.org/web/20220528140444/https://cmake.org/cmake/help/latest/command/set_target_properties.html) устанавливает указанные свойства целей заданными значениями. Данная команда принимает список целей, для которых будут установлены значения свойств, а затем ключевое слово `PROPERTIES`, после которого следует список вида `<название свойства> <новое значение>`:

  

# Установить свойству 'C_STANDARD' значение "11",
# а свойству 'C_STANDARD_REQUIRED' значение "ON":
set_target_properties(MyTarget PROPERTIES C_STANDARD 11 C_STANDARD_REQUIRED ON)

  

Пример выше задал цели `MyTarget` свойства, влияющие на процесс компиляции, а именно: при компиляции цели `MyTarget` CMake затребует компилятора о использовании стандарта C11. Все известные именования свойств целей перечисляются на [этой странице](https://web.archive.org/web/20220528140444/https://cmake.org/cmake/help/latest/manual/cmake-properties.7.html#target-properties).

  

Также имеется возможность проверки ранее определённых целей с помощью конструкции `if(TARGET <TargetName>)`:

  

# Выведет "The target was defined!" если цель "MyTarget" уже определена,
# а иначе выведет "The target was not defined!":
if(TARGET MyTarget)
    message("The target was defined!")
else()
    message("The target was not defined!")
endif()

  

#### Добавление подпроектов

Команда [`add_subdirectory`](https://web.archive.org/web/20220528140444/https://cmake.org/cmake/help/latest/command/add_subdirectory.html) побуждает CMake к незамедлительной обработке указанного файла подпроекта. Пример ниже демонстрирует применение описанного механизма:

  

# Добавить каталог "subLibrary" в сборку основного проекта,
# а генерируемые файлы расположить в каталоге "subLibrary/build":
add_subdirectory(subLibrary subLibrary/build)

  

В данном примере первым аргументом команды [`add_subdirectory`](https://web.archive.org/web/20220528140444/https://cmake.org/cmake/help/latest/command/add_subdirectory.html) выступает подпроект `subLibrary`, а второй аргумент необязателен и информирует CMake о папке, предназначенной для генерируемых файлов включаемого подпроекта (например, `CMakeCache.txt` и `cmake_install.cmake`).

  

Стоит отметить, что все переменные из родительской области видимости унаследуются добавленным каталогом, а все переменные, определённые и переопределённые в данном каталоге, будут видимы лишь ему (если ключевое слово `PARENT_SCOPE` не было определено аргументом команды [`set`](https://web.archive.org/web/20220528140444/https://cmake.org/cmake/help/latest/command/set.html)). Данную особенность [упомянули](https://web.archive.org/web/20220528140444/https://habr.com/post/431428/#comment_19428934) в комментариях к [предыдущей статье](https://web.archive.org/web/20220528140444/https://habr.com/post/431428).

  

#### Поиск пакетов

  

Команда [`find_package`](https://web.archive.org/web/20220528140444/https://cmake.org/cmake/help/latest/command/find_package.html) находит и загружает настройки внешнего проекта. В большинстве случаев она применяется для последующей линковки внешних библиотек, таких как [Boost](https://web.archive.org/web/20220528140444/https://www.boost.org/) и [GSL](https://web.archive.org/web/20220528140444/https://www.gnu.org/software/gsl). Данный пример вызывает описанную команду для поиска библиотеки [GSL](https://web.archive.org/web/20220528140444/https://www.gnu.org/software/gsl) и последующей линковки:

  

# Загрузить настройки пакета библиотеки "GSL":
find_package(GSL 2.5 REQUIRED)

# Скомпоновать исполняемый файл с библиотекой "GSL":
target_link_libraries(MyExecutable GSL::gsl)

# Уведомить компилятор о каталоге заголовков "GSL":
target_include_directories(MyExecutable ${GSL_INCLUDE_DIRS})

  

В приведённом выше примере команда [`find_package`](https://web.archive.org/web/20220528140444/https://cmake.org/cmake/help/latest/command/find_package.html) первым аргументом принимает наименование пакета, а затем требуемую версию. Опция `REQUIRED` требует печати фатальной ошибки и завершении работы CMake, если требуемый пакет не найден. Противоположность — это опция `QUIET`, требующая CMake продолжать свою работу, даже если пакет не был найден.

  

Далее исполняемый файл `MyExecutable` линкуется с библиотекой GSL командой [`target_link_libraries`](https://web.archive.org/web/20220528140444/https://cmake.org/cmake/help/latest/command/target_link_directories.html) с помощью переменной `GSL::gsl`, инкапсулирующей расположение уже скомпилированной GSL.

  

В конце вызывается команда [`target_include_directories`](https://web.archive.org/web/20220528140444/https://cmake.org/cmake/help/latest/command/target_include_directories.html), информирующая компилятора о расположении заголовочных файлов библиотеки GSL. Обратите внимание на то, что используется переменная `GSL_INCLUDE_DIRS`, хранящая местоположение описанных мною заголовков (это пример импортированных настроек пакета).

  

Вам, вероятно, захочеться проверить результат поиска пакета, если Вы указали опцию `QUIET`. Это можно сделать путём проверки переменной `<PackageName>_FOUND`, автоматически определяемой после завершения команды [`find_package`](https://web.archive.org/web/20220528140444/https://cmake.org/cmake/help/latest/command/find_package.html). Например, в случае успешного импортирования настроек GSL в Ваш проект, переменная `GSL_FOUND` обратится в истину.

  

В общем случае, команда [`find_package`](https://web.archive.org/web/20220528140444/https://cmake.org/cmake/help/latest/command/find_package.html) имеет две разновидности запуска: модульную и конфигурационную. Пример выше применял модульную форму. Это означает, что во время вызова команды CMake ищет скриптовый файл вида `Find<PackageName>.cmake` в директории [`CMAKE_MODULE_PATH`](https://web.archive.org/web/20220528140444/https://cmake.org/cmake/help/latest/variable/CMAKE_MODULE_PATH.html#variable:CMAKE_MODULE_PATH), а затем запускает его и импортирует все необходимые настройки (в данном случае CMake запустила стандартный файл `FindGSL.cmake`).

  

#### Способы включения заголовков

Информировать компилятора о располжении включаемых заголовков можно посредством двух команд: [`include_directories`](https://web.archive.org/web/20220528140444/https://cmake.org/cmake/help/latest/command/include_directories.html) и [`target_include_directories`](https://web.archive.org/web/20220528140444/https://cmake.org/cmake/help/latest/command/target_include_directories.html). Вы решаете, какую из них использовать, однако стоит учесть некоторые различия между ними (идея предложена в [комментариях](https://web.archive.org/web/20220528140444/https://habr.com/post/432096/#comment_19457688)).

  

Команда [`include_directories`](https://web.archive.org/web/20220528140444/https://cmake.org/cmake/help/latest/command/include_directories.html) влияет на область каталога. Это означает, что все директории заголовков, указанные данной командой, будут применяться для всех целей текущего `CMakeLists.txt`, а также для обрабатываемых подпроектов (смотреть [`add_subdirectory`](https://web.archive.org/web/20220528140444/https://cmake.org/cmake/help/latest/command/add_subdirectory.html)).

  

Команда [`target_include_directories`](https://web.archive.org/web/20220528140444/https://cmake.org/cmake/help/latest/command/target_include_directories.html) влияет лишь на указанную первым аргументом цель, а на другие цели никакого воздействия не оказывается. Пример ниже демонстрирует разницу между этими двумя командами:

  

add_executable(RequestGenerator RequestGenerator.c)
add_executable(ResponseGenerator ResponseGenerator.c)

# Применяется лишь для цели "RequestGenerator":
target_include_directories(RequestGenerator headers/specific)

# Применяется для целей "RequestGenerator" и "ResponseGenerator":
include_directories(headers)

  

В комментариях [упомянуто](https://web.archive.org/web/20220528140444/https://habr.com/post/432096/#comment_19457870), что в современных проектах применение команд [`include_directories`](https://web.archive.org/web/20220528140444/https://cmake.org/cmake/help/latest/command/include_directories.html) и [`link_libraries`](https://web.archive.org/web/20220528140444/https://cmake.org/cmake/help/latest/command/link_libraries.html) является нежелательным. Альтернатива — это команды [`target_include_directories`](https://web.archive.org/web/20220528140444/https://cmake.org/cmake/help/latest/command/target_include_directories.html) и [`target_link_libraries`](https://web.archive.org/web/20220528140444/https://cmake.org/cmake/help/latest/command/target_link_libraries.html), действующие лишь на конкретные цели, а не на всю текущую область видимости.

  

#### Установка проекта

Команда [`install`](https://web.archive.org/web/20220528140444/https://cmake.org/cmake/help/latest/command/install.html) генерирует установочные правила для Вашего проекта. Данная команда способна работать с целями, файлами, папками и многим другим. Сперва рассмотрим установку целей.

  

Для установки целей необходимо первым аргументом описанной функции передать ключевое слово `TARGETS`, за которым должен следовать список устанавливаемых целей, а затем ключевое слово `DESTINATION` с расположением каталога, в который установятся указанные цели. Данный пример демонстрирует типичную установку целей:

  

# Установить цели "TimePrinter" и "DataScanner" в директорию "bin":
install(TARGETS TimePrinter DataScanner DESTINATION bin)

  

Процесс описания установки файлов аналогичен, за тем исключением, что вместо ключевого слова `TARGETS` следует указать `FILES`. Пример, демонстрирующий установку файлов:

  

# Установить файлы "DataCache.txt" и "MessageLog.txt" в директорию "~/":
install(FILES DataCache.txt MessageLog.txt DESTINATION ~/)

  

Процесс описания установки папок аналогичен, за тем исключением, что вместо ключевого слова `FILES` следует указать `DIRECTORY`. Важно подметить, что при установке будет копироваться всё содержимое папки, а не только её название. Пример установки папок выглядит следующим образом:

  

# Установить каталоги "MessageCollection" и "CoreFiles" в директорию "~/":
install(DIRECTORY MessageCollection CoreFiles DESTINATION ~/)

  

После завершения обработки CMake всех Ваших файлов Вы можете выполнить установку всех описанных объектов командой `sudo checkinstall` (если CMake генерирует `Makefile`), или же выполнить данное действие интегрированной средой разработки, поддерживающей CMake.

  

#### Наглядный пример проекта

Данное руководство было бы неполным без демонстрации реального примера использования системы сборки CMake. Рассмотрим схему простого проекта, использующего CMake в качестве единственной системы сборки:

  

+ MyProject
      - CMakeLists.txt
      - Defines.h
      - StartProgram.c
      + core
            - CMakeLists.txt
            - Core.h
            - ProcessInvoker.c
            - SystemManager.c

  

Главный файл сборки `CMakeLists.txt` описывает компиляцию всей программы: сперва происходит вызов команды [`add_executable`](https://web.archive.org/web/20220528140444/https://cmake.org/cmake/help/latest/command/add_executable.html), компилирующей исполняемый файл, затем вызывается команда [`add_subdirectory`](https://web.archive.org/web/20220528140444/https://cmake.org/cmake/help/latest/command/add_subdirectory.html), побуждающая обработку подпроекта, и наконец, исполняемый файл линкуется с собранной библиотекой:

  

# Задать минимальную версию CMake:
cmake_minimum_required(VERSION 3.0)

# Указать характеристики проекта:
project(MyProgram VERSION 1.0.0 LANGUAGES C)

# Добавить в сборку исполняемый файл "MyProgram":
add_executable(MyProgram StartProgram.c)

# Требовать обработку файла "core/CMakeFiles.txt":
add_subdirectory(core)

# Скомпоновать исполняемый файл "MyProgram" со
# скомпилированной статической библиотекой "MyProgramCore":
target_link_libraries(MyProgram MyProgramCore)

# Установить исполняемый файл "MyProgram" в директорию "bin":
install(TARGETS MyProgram DESTINATION bin)

  

Файл `core/CMakeLists.txt` вызывается главным файлом сборки и компилирует статическую библиотеку `MyProgramCore`, предназначенную для линковки с исполняемым файлом:

  

# Задать минимальную версию CMake:
cmake_minimum_required(VERSION 3.0)

# Добавить в сборку статическую библиотеку "MyProgramCore":
add_library(MyProgramCore STATIC ProcessInvoker.c SystemManager.c)

  

После череды команд `cmake . && make && sudo checkinstall` работа системы сборки CMake завершается успешно. Первая команда запускает обработку файла `CMakeLists.txt` в корневом каталоге проекта, вторая команда окончательно компилирует необходимые двоичные файлы, а третья команда устанавливает скомпонованный исполняемый файл `MyProgram` в систему.





