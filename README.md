[![CMake CI](https://github.com/Wartheree/lab05/actions/workflows/ci.yml/badge.svg)](https://github.com/Wartheree/lab05/actions/workflows/ci.yml)
## Отчёт к lab05
В рамках выполнения данной лабораторной работы мною были выполнены команды из tutorial с некоторыми изменениями:
1) Скопирован репозиторий из lab04:
```bash
$ git clone https://github.com/${GITHUB_USERNAME}/lab04 projects/lab05
Клонирование в «projects/lab05»...
remote: Enumerating objects: 48, done.
remote: Counting objects: 100% (48/48), done.
remote: Compressing objects: 100% (29/29), done.
remote: Total 48 (delta 11), reused 40 (delta 9), pack-reused 0 (from 0)
Получение объектов: 100% (48/48), 12.30 КиБ | 179.00 КиБ/с, готово.
Определение изменений: 100% (11/11), готово.
$ cd projects/lab05
$ git remote remove origin
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab05
```
2) Установлен и закоммичен gtest более актуальной версии, чем предложено в tutorial:
```bash
$ git submodule add https://github.com/google/googletest third-party/gtest
Клонирование в «/home/danila/Wartheree/workspace/projects/lab05/third-party/gtest»...
remote: Enumerating objects: 28627, done.
remote: Counting objects: 100% (61/61), done.
remote: Compressing objects: 100% (46/46), done.
Получение объектов:  13% (3722/28627), 1.14 МиБ | 2.Получение объектов:  14% (4008/28627), 1.14 МиБ | 2.Получение объектов:  15% (4295/28627), 1.14 МиБ | 2.Получение объектов:  16% (4581/28627), 1.14 МиБ | 2.Получение объектов:  17% (4867/28627), 1.14 МиБ |
...
5Получение объектов:  98% (28055/28627), 8.40 МиБ | 5Получение объектов:  99% (28341/28627), 8.40 МиБ | 5remote: Total 28627 (delta 32), reused 15 (delta 15), pack-reused 28566 (from 2)
Получение объектов: 100% (28627/28627), 13.54 МиБ | Получение объектов: 100% (28627/28627), 13.74 МиБ | 6.63 МиБ/с, готово.
Определение изменений: 100% (21273/21273), готово.

$ cd third-party/gtest && git checkout release-1.15.2 && cd ../..
$ git add third-party/gtest
$ git commit -m"added gtest framework v1.15.2"
[main a01e205] added gtest framework v1.15.2
 2 files changed, 4 insertions(+)
 create mode 100644 .gitmodules
 create mode 160000 third-party/gtest
```

3) К Cmake.txt добавлен фрагмент кода:
```cmake
if(BUILD_TESTS)
  enable_testing()
  add_subdirectory(third-party/gtest)
  file(GLOB \${PROJECT_NAME}_TEST_SOURCES tests/*.cpp)
  add_executable(check \${\${PROJECT_NAME}_TEST_SOURCES})
  target_link_libraries(check \${PROJECT_NAME} gtest_main)
  add_test(NAME check COMMAND check)
endif()
```
4) Написан тест:
```cpp
#include <print.hpp>

#include <gtest/gtest.h>

TEST(Print, InFileStream)
{
  std::string filepath = "file.txt";
  std::string text = "hello";
  std::ofstream out{filepath};

  print(text, out);
  out.close();

  std::string result;
  std::ifstream in{filepath};
  in >> result;

  EXPECT_EQ(result, text);
}
```
5) Произведена сборка и запущены тесты (с менее и более подробным выводом):
```bash
$ cmake -H. -B_build -DBUILD_TESTS=ON
CMake Deprecation Warning at CMakeLists.txt:1 (cmake_minimum_required):
  Compatibility with CMake < 3.10 will be removed from a future version of
  CMake.

  Update the VERSION argument <min> value.  Or, use the <min>...<max> syntax
  to tell CMake that the project requires at least <min> but has been updated
  to work with policies introduced by <max> or earlier.


-- The C compiler identification is GNU 14.2.0
-- The CXX compiler identification is GNU 14.2.0
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
-- Performing Test CMAKE_HAVE_LIBC_PTHREAD
-- Performing Test CMAKE_HAVE_LIBC_PTHREAD - Success
-- Found Threads: TRUE
-- Configuring done (2.6s)
-- Generating done (0.0s)
-- Build files have been written to: /home/danila/Wartheree/workspace/projects/lab05/_build
```
```bash
$ cmake --build _build
[  8%] Building CXX object CMakeFiles/print.dir/sources/print.cpp.o
[ 16%] Linking CXX static library libprint.a
[ 16%] Built target print
[ 25%] Building CXX object third-party/gtest/googletest/CMakeFiles/gtest.dir/src/gtest-all.cc.o
[ 33%] Linking CXX static library ../../../lib/libgtest.a
[ 33%] Built target gtest
[ 41%] Building CXX object third-party/gtest/googletest/CMakeFiles/gtest_main.dir/src/gtest_main.cc.o
[ 50%] Linking CXX static library ../../../lib/libgtest_main.a
[ 50%] Built target gtest_main
[ 58%] Building CXX object CMakeFiles/check.dir/tests/test1.cpp.o
[ 66%] Linking CXX executable check
[ 66%] Built target check
[ 75%] Building CXX object third-party/gtest/googlemock/CMakeFiles/gmock.dir/src/gmock-all.cc.o
[ 83%] Linking CXX static library ../../../lib/libgmock.a
[ 83%] Built target gmock
[ 91%] Building CXX object third-party/gtest/googlemock/CMakeFiles/gmock_main.dir/src/gmock_main.cc.o
[100%] Linking CXX static library ../../../lib/libgmock_main.a
[100%] Built target gmock_main
```
```bash
$ cmake --build _build --target test
Running tests...
Test project /home/danila/Wartheree/workspace/projects/lab05/_build
    Start 1: check
1/1 Test #1: check ............................   Passed    0.01 sec

100% tests passed, 0 tests failed out of 1

Total Test time (real) =   0.03 sec
```
```bash
$ _build/check
Running main() from /home/danila/Wartheree/workspace/projects/lab05/third-party/gtest/googletest/src/gtest_main.cc
[==========] Running 1 test from 1 test suite.
[----------] Global test environment set-up.
[----------] 1 test from Print
[ RUN      ] Print.InFileStream
[       OK ] Print.InFileStream (0 ms)
[----------] 1 test from Print (0 ms total)

[----------] Global test environment tear-down
[==========] 1 test from 1 test suite ran. (0 ms total)
[  PASSED  ] 1 test.
```
```bash
$ cmake --build _build --target test -- ARGS=--verbose
Running tests...
UpdateCTestConfiguration  from :/home/danila/Wartheree/workspace/projects/lab05/_build/DartConfiguration.tcl
UpdateCTestConfiguration  from :/home/danila/Wartheree/workspace/projects/lab05/_build/DartConfiguration.tcl
Test project /home/danila/Wartheree/workspace/projects/lab05/_build
Constructing a list of tests
Done constructing a list of tests
Updating test list for fixtures
Added 0 tests to meet fixture requirements
Checking test dependency graph...
Checking test dependency graph end
test 1
    Start 1: check

1: Test command: /home/danila/Wartheree/workspace/projects/lab05/_build/check
1: Working Directory: /home/danila/Wartheree/workspace/projects/lab05/_build
1: Test timeout computed to be: 10000000
1: Running main() from /home/danila/Wartheree/workspace/projects/lab05/third-party/gtest/googletest/src/gtest_main.cc
1: [==========] Running 1 test from 1 test suite.
1: [----------] Global test environment set-up.
1: [----------] 1 test from Print
1: [ RUN      ] Print.InFileStream
1: [       OK ] Print.InFileStream (1 ms)
1: [----------] 1 test from Print (1 ms total)
1:
1: [----------] Global test environment tear-down
1: [==========] 1 test from 1 test suite ran. (5 ms total)
1: [  PASSED  ] 1 test.
1/1 Test #1: check ............................   Passed    0.01 sec

100% tests passed, 0 tests failed out of 1

Total Test time (real) =   0.02 sec
```
6) В файл ci.yml дописана часть кода:
```yml
$ cat .github/workflows/ci.yml
name: CMake CI

on:
  push:
    branches: [ master, main ]
  pull_request:
    branches: [ master, main ]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v4
      with:
        submodules: true

    - name: Install CMake
      run: |
        sudo apt-get update
        sudo apt-get install -y cmake cmake-data

    - name: Configure CMake
      run: cmake -H. -B_build -DCMAKE_INSTALL_PREFIX=_install -DBUILD_TESTS=ON

    - name: Build
      run: cmake --build _build

    - name: Install
      run: cmake --build _build --target install

    - name: Run tests
      run: cmake --build _build --target test -- ARGS=--verbose
```
7) Все изменения закоммичены и запушены.
8) Репозиторий прошёл все тесты (бейдж об этом в начале отчёта).
