# lab3

   <Task 1>
    
    Создание CMakeLists.txt для автоматической сборки данной библиотеки
```
$ cd formatter_lib
$ cat > CMakeLists.txt <<EOF
cmake_minimum_required(VERSION 3.10) 
project(formatter) 
EOF
```
    2. Установка стандарта языка
```
$ cat >> CMakeLists.txt <<EOF
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
EOF
```
    3. Сборка статической библиотеки с именем formatter и данным файлом formatter.cpp
```
$ cat >> CMakeLists.txt <<EOF
add_library(formatter STATIC \${CMAKE_CURRENT_SOURCE_DIR}/formatter.cpp)
EOF
```
    4. Добавление директории с заголовочными файлами
```
$ cat >> CMakeLists.txt <<EOF
include_directories(\${CMAKE_CURRENT_SOURCE_DIR})
EOF
```
    5. Создаем директорию для использования корневой папки для сборки
```
$ cmake -H. -B_build
```
    6. Проверка библиотеки formatter на наличие.
```
$ ls _build/libformatter.a
_build/libformatter.a
```
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
	<Task 2>
	
    1. Создание CMakeLists.txt для автоматизированной сборки библиотеки в папке formatter_ex_lib
```
$ cd formatter_ex_lib
$ cat > CMakeLists.txt <<EOF
cmake_minimum_required(VERSION 3.10)
project(formatter_ex)
EOF
```
    2. Установка стандарта языка и делаем его обязательным, а так же изменим переменную CMAKE_CURRENT_SOURCE_DIR
```
$ cat >> CMakeLists.txt <<EOF
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CURRENT_SOURCE_DIR /Home//Heinene/workspace/projects/lab03dz)
EOF
```
    3. Сборка статической библиотеки с именем formatter_ex_lib и исходным файлом formatter.cpp
```
$ cat >> CMakeLists.txt <<EOF
add_library(formatter_ex STATIC \${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex.cpp)
EOF
```
    4. Добавляем директории с заголовочными файлами
```
$ cat >> CMakeLists.txt <<EOF
include_directories(\${CMAKE_CURRENT_SOURCE_DIR})
EOF
```
    5. Указание директории где находятся заголовочные файлы
```
$ cat >> CMakeLists.txt <<EOF
include_directories(\${CMAKE_CURRENT_SOURCE_DIR}/formatter_lib)
target_link_libraries(formatter_ex formatter)
EOF
```
    6. Создание директории для использования корневой папки для сборки
```
$ cmake -H. -B_build
```
    7. Проверка библиотеки formatter на наличие.
```
$ ls _build/libformatter_ex.a
_build/libformatter_ex.a
```
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
	<Task 3>

    hello_world, которое использует библиотеку formatter_ex;

    1. Создание CMakeLists.txt для автоматизированной сборки библиотеки в папке formatter_ex_lib
```
$ cd hello_world_application
$ cat > CMakeLists.txt <<EOF
cmake_minimum_required(VERSION 3.10)
project(hello_world)
EOF
```
    2. Установливаем стандарт языка и делаем его обязательным, а так же изменим переменную CMAKE_CURRENT_SOURCE_DIR
```
$ cat >> CMakeLists.txt <<EOF
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CURRENT_SOURCE_DIR /Home//Heinene/workspace/projects/lab03dz)
EOF
```
    3. Добавление исполняемого файла hello_world.cpp
```
$ cat >> CMakeLists.txt <<EOF
add_executable(hello_world \${CMAKE_CURRENT_SOURCE_DIR}/hello_world_application/hello_world.cpp)
EOF
```
    4. Указываем директории где находятся заголовочные файлы библиотек formatter и formatter_ex.
```
$ cat >> CMakeLists.txt <<EOF
include_directories(\${CMAKE_CURRENT_SOURCE_DIR}/formatter_lib \${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex_lib)
find_library(formatter NAMES libformatter.a PATHS \${CMAKE_CURRENT_SOURCE_DIR}/formatter_lib)
find_library(formatter_ex NAMES libformatter_ex.a PATHS \${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex_lib)
EOF
```
    5. Линкуем проект
```
$ cat >> CMakeLists.txt <<EOF
target_link_libraries(hello_world \${formatter} \${formatter_ex})
EOF
```
    6. Собираем проект с помощью CMake в директории _build
```
$ cmake -H. -B_build
```
    7. Запускаем проект
```
$ _build/hello_world &&
```
    8. solver, приложение которое использует статические библиотеки formatter_ex и solver_lib
```
$ cd solver_application
$ cat > CMakeLists.txt <<EOF
cmake_minimum_required(VERSION 3.10)
project(solver)
EOF
```
    9. Установливаем стандарт языка и делаем его обязательным, а так же изменим переменную CMAKE_CURRENT_SOURCE_DIR
```
$ cat >> CMakeLists.txt <<EOF
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CURRENT_SOURCE_DIR /Home//Heinene/workspace/projects/lab03dz)
EOF
```
    10. Сборка статической библиотеки с именем solver_lib и исходным файлом solver.cpp
```
$ cat >> CMakeLists.txt <<EOF
add_library(solver_lib STATIC \${CMAKE_CURRENT_SOURCE_DIR}/solver_lib/solver.cpp)
EOF
```
    11. Указываем директорию где находятся заголовочные файлы библиотек
```
$ cat >> CMakeLists.txt <<EOF
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/solver_lib ${CMAKE_CURRENT_SOURCE_DIR}/formatter_lib ${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex_lib)
EOF
```
    12. Создаем директорию для сборки проекта с помощью CMakе
```
$ cmake -H. -B_build
```
    13. Добавляем исполняемый файл
```
$ cat >> CMakeLists.txt <<EOF
add_executable(solver \${CMAKE_CURRENT_SOURCE_DIR}/solver_application/equation.cpp)
EOF
```
    14. Указываем директории, где находятся заголовочные файлы библиотек.
```
$ cat >> CMakeLists.txt <<EOF
find_library(formatter NAMES libformatter.a PATHS \${CMAKE_CURRENT_SOURCE_DIR}/formatter_lib)
find_library(formatter_ex NAMES libformatter_ex.a PATHS \${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex_lib)
find_library(solver_lib NAMES libsolver_lib.a PATHS \${CMAKE_CURRENT_SOURCE_DIR}/solver_lib)
EOF
```
    15. Линкуем проект (union)
```
$ cat >> CMakeLists.txt <<EOF
target_link_libraries(solver \${formatter} \${formatter_ex} \${solver_lib})
EOF
```
    16. Собираем проект с помощью CMake
```
$ cmake -H. -B_build
```
    17. Собираем проект с помощью CMake в директории _build
```
$ cmake --build _build # Сборка всего проекта
$ cmake --build _build --target solver_lib 
$ cmake --build _build --target solver 
```
    18. Запускаем проект
```
$ _build/solver && echo
```
