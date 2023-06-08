# Лабораторная работа №6
После того, как вы настроили взаимодействие с системой непрерывной интеграции, обеспечив автоматическую сборку и тестирование ваших изменений, стоит задуматься о создание пакетов для измениний, которые помечаются тэгами (см. вкладку releases). Пакет должен содержать приложение solver из предыдущего задания

# Task 1

Создадим CMakeLists.txt для formatter_ex_lib, formatter_lib и также solver_application

CMakeLists.txt для formatter_ex_lib:

![](https://github.com/sippyuy/timp6/blob/main/screens/1.png)

CMakeLists.txt для formatter_lib:

![](https://github.com/sippyuy/timp6/blob/main/screens/2.png)

CMakeLists.txt для solver_application:

![](https://github.com/sippyuy/timp6/blob/main/screens/3.png)

Теперь создадим CMakeLists.txt для линковки всех директорий

CMakeLists.txt:

![](https://github.com/sippyuy/timp6/blob/main/screens/4.png)

Создадим файл CPackConfig.cmake (утилита пакетирования)
CPackConfig.cmake:

![](https://github.com/sippyuy/timp6/blob/main/screens/extra.png)

Создадим лицензию и описание для работы нашей программы

Создаём два скрипта Action.yml и Release.yml чтобы запакетировать и запустить программу

Action.yml:

![](https://github.com/sippyuy/timp6/blob/main/screens/6.png)

Release.yml:

![](https://github.com/sippyuy/timp6/blob/main/screens/7.png)
