# Лабораторная работа №6
После того, как вы настроили взаимодействие с системой непрерывной интеграции, обеспечив автоматическую сборку и тестирование ваших изменений, стоит задуматься о создание пакетов для измениний, которые помечаются тэгами (см. вкладку releases). Пакет должен содержать приложение solver из предыдущего задания

# Task 1

Создадим CMakeLists.txt для formatter_ex_lib, formatter_lib и также solver_application

CMakeLists.txt для formatter_ex_lib:

![](https://github.com/sippyuy/timp6/blob/main/screens/1.png)
```
cmake_minimum_required(VERSION 3.4)

project(formatter_ex_lib)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../formatter_lib formatter_lib_dir)

add_library(formatter_ex_lib STATIC ${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex.cpp)

target_include_directories(formatter_ex_lib PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/../formatter_lib )

target_link_libraries(formatter_ex_lib formatter_lib)
```

CMakeLists.txt для formatter_lib:

![](https://github.com/sippyuy/timp6/blob/main/screens/2.png)
```
cmake_minimum_required(VERSION 3.4)

project(formatter_lib)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_library(formatter_lib STATIC ${CMAKE_CURRENT_SOURCE_DIR}/formatter.cpp ${CMAKE_CURRENT_SOURCE_DIR}/formatter.h)
```

CMakeLists.txt для solver_application:

![](https://github.com/sippyuy/timp6/blob/main/screens/3.png)
```
cmake_minimum_required(VERSION 3.4)

project(solver)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_subdirectory(/../formatter_ex_lib formatter_ex_lib_dir)

add_library(solver_lib /../solver_lib/solver.cpp /../solver_lib/solver.h)
add_executable(solver /equation.cpp)

target_include_directories(formatter_ex_lib PUBLIC /../formatter_lib /../formatter_ex_lib /../solver_lib)

target_link_libraries(solver formatter_ex_lib formatter_lib solver_lib)
```

Теперь создадим CMakeLists.txt для линковки всех директорий

CMakeLists.txt:

![](https://github.com/sippyuy/timp6/blob/main/screens/4.png)

```
cmake_minimum_required(VERSION 3.4)

project(solver)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex_lib formatter_ex_lib_dir)

add_library(solver_lib ${CMAKE_CURRENT_SOURCE_DIR}/solver_lib/solver.cpp)
add_executable(solver ${CMAKE_CURRENT_SOURCE_DIR}/solver_application/equation.cpp)

target_include_directories(formatter_ex_lib PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/formatter_lib ${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex_lib ${CMAKE_CURRENT_SOURCE_DIR}/solver_lib)

target_link_libraries(solver formatter_ex_lib formatter_lib solver_lib)

install(TARGETS solver
    RUNTIME DESTINATION bin
)

include(CPackConfig.cmake)
```

Создадим файл CPackConfig.cmake (утилита пакетирования)
CPackConfig.cmake:

![](https://github.com/sippyuy/timp6/blob/main/screens/5.png)
```
include(InstallRequiredSystemLibraries)

set(CPACK_PACKAGE_CONTACT inkey.cherry@gmail.com)
set(CPACK_PACKAGE_VERSION ${PRINT_VERSION})
set(CPACK_PACKAGE_DESCRIPTION_FILE ${CMAKE_CURRENT_SOURCE_DIR}/DESCRIPTION)
set(CPACK_PACKAGE_DESCRIPTION_SUMMARY "C++ app for solving quadratic equations")
set(CPACK_RESOURCE_FILE_LICENSE ${CMAKE_CURRENT_SOURCE_DIR}/LICENSE)
set(CPACK_RESOURCE_FILE_README ${CMAKE_CURRENT_SOURCE_DIR}/README.md)

set(CPACK_SOURCE_IGNORE_FILES  "\\\\.cmake;/build/;/.git/;/.github/")

set(CPACK_SOURCE_INSTALLED_DIRECTORIES "${CMAKE_SOURCE_DIR}; /")

set(CPACK_SOURCE_GENERATOR "TGZ;ZIP")

set(CPACK_DEBIAN_PACKAGE_NAME "solverapp-dev")
set(CPACK_DEBIAN_FILE_NAME "solver-${PRINT_VERSION}.deb")
set(CPACK_DEBIAN_PACKAGE_VERSION ${PRINT_VERSION})
set(CPACK_DEBIAN_PACKAGE_ARCHITECTURE "all")
set(CPACK_DEBIAN_PACKAGE_MAINTAINER "ledibonibell")
set(CPACK_DEBIAN_PACKAGE_RELEASE 1)

set(CPACK_GENERATOR "DEB")

include(CPack)
```

Создадим лицензию и описание для работы нашей программы

Создаём два скрипта Action.yml и Release.yml чтобы запакетировать и запустить программу

Action.yml:

![](https://github.com/sippyuy/timp6/blob/main/screens/6.png)
```
name: CMake

on:
 push:
  branches: [master]
 pull_request:
  branches: [master]

jobs: 
 build_Linux:

  runs-on: ubuntu-latest

  steps:
  - uses: actions/checkout@v3

  - name: Configure Solver
    run: cmake ${{github.workspace}} -B ${{github.workspace}}/build

  - name: Build Solver
    run: cmake --build ${{github.workspace}}/build

$ git add Action.yml
$ git commit -m "Action"
$ git push origin master
**Вводим логин и токен

$ cat >> Release.yml << EOF
>EOF
$ nano Release.yml
```

Release.yml:

![](https://github.com/sippyuy/timp6/blob/main/screens/7.png)
```
name: CMake

on:
 push:
   tags:
     - v**

permissions:
  contents: write

jobs: 

  build_packages_Linux:

    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v3

    - name: Configure Solver
      run: cmake ${{github.workspace}} -B ${{github.workspace}}/build -D PRINT_VERSION=${GITHUB_REF_NAME#v}

    - name: Build Solver
      run: cmake --build ${{github.workspace}}/build

    - name: Build package
      run: cmake --build ${{github.workspace}}/build --target package

    - name: Build source package
      run: cmake --build ${{github.workspace}}/build --target package_source

    - name: Make a release
      uses: ncipollo/release-action@v1.10.0
      with:
        artifacts: "build/*.deb,build/*.tar.gz,build/*.zip"
        replacesArtifacts: false
        GITHUB_TOKEN: ${{ secrets.GH_PAT }}
        allowUpdates: true
```
