## Laboratory work VI

## Homework

Представьте, что вы стажер в компании "Formatter Inc.".
### Задание 1
Вам поручили перейти на систему автоматизированной сборки **CMake**.
Исходные файлы находятся в директории [formatter_lib](formatter_lib).
В этой директории находятся файлы для статической библиотеки *formatter*.
Создайте `CMakeList.txt` в директории [formatter_lib](formatter_lib),
с помощью которого можно будет собирать статическую библиотеку *formatter*.

```
$cmake --version
```
```
cmake version 3.22.1
CMake suite maintained and supported by Kitware (kitware.com/cmake).
```
### formatter_lib/CMakeLists.txt:
```
make_minimum_required(VERSION 3.22) 
project(formatter_lib)
set(SOURCE_LIB formatter.cpp formatter.h)       
add_library(formatter_lib STATIC ${SOURCE_LIB}) 
```

```
$cmake -H. -B_build
```
```
-- The C compiler identification is GNU 10.5.0
-- The CXX compiler identification is GNU 10.5.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/zarok/projects/tp_labs/tp_projects/lab03/formatter_lib/_build
```
```
$cmake --build ./_build
```
```
[ 50%] Building CXX object CMakeFiles/formatter_lib.dir/formatter.cpp.o
[100%] Linking CXX static library libformatter_lib.a
[100%] Built target formatter_lib
```

### Задание 2
У компании "Formatter Inc." есть перспективная библиотека,
которая является расширением предыдущей библиотеки. Т.к. вы уже овладели
навыком созданием `CMakeList.txt` для статической библиотеки *formatter*, ваш 
руководитель поручает заняться созданием `CMakeList.txt` для библиотеки 
*formatter_ex*, которая в свою очередь использует библиотеку *formatter*.

```
$ cd ..
$cd formatter_ex_lib/
```
### formatter_ex_lib/CMakeLists.txt:
```
cmake_minimum_required(VERSION 3.22)
project(formatter_ex_lib)
set(SOURCE_LIB formatter_ex.cpp)
add_library(formatter_ex_lib STATIC ${SOURCE_LIB})
add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../formatter_lib formatter_lib)
target_link_libraries(formatter_ex_lib PUBLIC formatter_lib)
target_include_directories(formatter_ex_lib PUBLIC
				"${CMAKE_CURRENT_SOURCE_DIR}"
				"${CMAKE_CURRENT_SOURCE_DIR}/../formatter_lib")
```

### Задание 3
Конечно же ваша компания предоставляет примеры использования своих библиотек.
Чтобы продемонстрировать как работать с библиотекой *formatter_ex*,
вам необходимо создать два `CMakeList.txt` для двух простых приложений:
* *hello_world*, которое использует библиотеку *formatter_ex*;
* *solver*, приложение которое испольует статические библиотеки *formatter_ex* и *solver_lib*.

```
$cd ..
$cd hello_world_application/
```

### hello_world_application/CMakeLists.txt:
```
make_minimum_required(VERSION 3.22) 
project(hello_world)				
set(SOURCE_EXE hello_world.cpp)			 
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/../formatter_ex_lib/)
add_executable(main ${SOURCE_EXE})	
add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../formatter_ex_lib/ formatter_ex_lib)
target_link_libraries(main formatter_ex_lib)

```

```
$cd ..
$cd solver_application/
```
### solver_application/CMakeLists.txt:
```
cmake_minimum_required(VERSION 3.22)
project(solver)
set(SOURCE_EXE equation.cpp)
include_directories("${CMAKE_CURRENT_SOURCE_DIR}/../formatter_ex_lib/"
                                        "${CMAKE_CURRENT_SOURCE_DIR}/../solver_lib/")
add_executable(main ${SOURCE_EXE})      
add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../formatter_ex_lib/ formatter_ex_lib)
add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../solver_lib/ solver_lib)
target_link_libraries(main formatter_ex_lib solver_lib)

```
```
$cd ..
$cd solver_lib/
```
### solver_lib/CMakeLists.txt:
```
cmake_minimum_required(VERSION 3.22)
project(solver_lib)
set(SOURCE_LIB solver.cpp solver.h)             
add_library(solver_lib STATIC ${SOURCE_LIB})
```


### Компиляция
 - formatter_lib
```
$cmake -H. -B_build
$cmake --build ./_build
```
```
-- The C compiler identification is GNU 10.5.0
-- The CXX compiler identification is GNU 10.5.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/zarok/projects/tp_labs/tp_projects/lab03/formatter_lib/_build

[ 50%] Building CXX object CMakeFiles/formatter_lib.dir/formatter.cpp.o
[100%] Linking CXX static library libformatter_lib.a
[100%] Built target formatter_lib
```

 - formatter_ex_lib
```
$cmake -H. -B_build
$cmake --build ./_build
```
```
-- The C compiler identification is GNU 10.5.0
-- The CXX compiler identification is GNU 10.5.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/zarok/projects/tp_labs/tp_projects/lab03/formatter_ex_lib/_build

zarok@ubuntu:~/projects/tp_labs/tp_projects/lab03/formatter_ex_lib$ cmake --build ./_build
[ 25%] Building CXX object formatter_lib/CMakeFiles/formatter_lib.dir/formatter.cpp.o
[ 50%] Linking CXX static library libformatter_lib.a
[ 50%] Built target formatter_lib
[ 75%] Building CXX object CMakeFiles/formatter_ex_lib.dir/formatter_ex.cpp.o
[100%] Linking CXX static library libformatter_ex_lib.a
[100%] Built target formatter_ex_lib

```

 - hello_world_application
```
$cmake -H. -B_build
$cmake --build ./_build
```
```
-- The C compiler identification is GNU 10.5.0
-- The CXX compiler identification is GNU 10.5.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/zarok/projects/tp_labs/tp_projects/lab03/hello_world_application/_build

zarok@ubuntu:~/projects/tp_labs/tp_projects/lab03/hello_world_application$ cmake --build ./_build
[ 16%] Building CXX object formatter_ex_lib/formatter_lib/CMakeFiles/formatter_lib.dir/formatter.cpp.o
[ 33%] Linking CXX static library libformatter_lib.a
[ 33%] Built target formatter_lib
[ 50%] Building CXX object formatter_ex_lib/CMakeFiles/formatter_ex_lib.dir/formatter_ex.cpp.o
[ 66%] Linking CXX static library libformatter_ex_lib.a
[ 66%] Built target formatter_ex_lib
[ 83%] Building CXX object CMakeFiles/main.dir/hello_world.cpp.o
[100%] Linking CXX executable main
[100%] Built target main

```

 - solver_lib
```
$cmake -H. -B_build
$cmake --build ./_build
```
```
-- The C compiler identification is GNU 10.5.0
-- The CXX compiler identification is GNU 10.5.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/zarok/projects/tp_labs/tp_projects/lab03/solver_lib/_build

Consolidate compiler generated dependencies of target solver_lib
[ 50%] Building CXX object CMakeFiles/solver_lib.dir/solver.cpp.o
[100%] Linking CXX static library libsolver_lib.a
[100%] Built target solver_lib


```

 - solver_application
```
$cmake -H. -B_build
$cmake --build ./_build
```
```
-- The C compiler identification is GNU 10.5.0
-- The CXX compiler identification is GNU 10.5.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/zarok/projects/tp_labs/tp_projects/lab03/solver_application/_build

zarok@ubuntu:~/projects/tp_labs/tp_projects/lab03/solver_application$ cmake --build ./_build
[ 12%] Building CXX object solver_lib/CMakeFiles/solver_lib.dir/solver.cpp.o
[ 25%] Linking CXX static library libsolver_lib.a
[ 25%] Built target solver_lib
[ 37%] Building CXX object formatter_ex_lib/formatter_lib/CMakeFiles/formatter_lib.dir/formatter.cpp.o
[ 50%] Linking CXX static library libformatter_lib.a
[ 50%] Built target formatter_lib
[ 62%] Building CXX object formatter_ex_lib/CMakeFiles/formatter_ex_lib.dir/formatter_ex.cpp.o
[ 75%] Linking CXX static library libformatter_ex_lib.a
[ 75%] Built target formatter_ex_lib
[ 87%] Building CXX object CMakeFiles/main.dir/equation.cpp.o
[100%] Linking CXX executable main
[100%] Built target main

```
### Создать .github/workflows/test.sh и .github/workflows/action.yml
```
$ sudo mkdir .github/
$ sudo mkdir .github/workflows
```
### test.sh
```
root=./
cmake --build $root/formatter_lib
cmake --build $root/formatter_ex_lib 
cmake --build $root/hello_world_application 
cmake --build $root/solver_lib
cmake --build $root/solver_application

$root/hello_world_application/_build/main
echo -e '1\n\2\n3' | $root/solver_application/_build/main
```
### action.yml
```
name: tp-lab-actions
run-name: ${{ github.actor }} is running TPLab04
on: [push]
jobs:
  container-job:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Test project
        run: ./.github/workflows/test.sh
```
```
zarok@ubuntu:~/projects/tp_labs/tp_projects/lab04/lab03/.github/workflows$ sudo chmod +x ../../hello_world_application/_build/main
zarok@ubuntu:~/projects/tp_labs/tp_projects/lab04/lab03/.github/workflows$ sudo chmod +x ../../solver_application/_build/main
zarok@ubuntu:~/projects/tp_labs/tp_projects/lab04/lab03/.github/workflows$ cd ../..
zarok@ubuntu:~/projects/tp_labs/tp_projects/lab04/lab03$ ./.github/workflows/test.sh
Error: could not load cache
Error: could not load cache
Error: could not load cache
Error: could not load cache
Error: could not load cache
-------------------------
hello, world!
-------------------------
-------------------------
x1 = -0.000000
-------------------------
-------------------------
x2 = 0.000000
-------------------------
```


## Links
- [Основы сборки проектов на С/C++ при помощи CMake](https://eax.me/cmake/)
- [CMake Tutorial](http://neerc.ifmo.ru/wiki/index.php?title=CMake_Tutorial)
- [C++ Tutorial - make & CMake](https://www.bogotobogo.com/cplusplus/make.php)
- [Autotools](http://www.gnu.org/software/automake/manual/html_node/Autotools-Introduction.html)
- [CMake](https://cgold.readthedocs.io/en/latest/index.html)

```
Copyright (c) 2015-2021 The ISC Authors
```
