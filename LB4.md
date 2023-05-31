## Лабораторная работа №4
## Данная лабораторная работа посвещена изучению систем непрерывной интеграции на примере сервиса Github Actions
## Homework
Вы продолжаете проходить стажировку в "Formatter Inc." (см подробности).

В прошлый раз ваше задание заключалось в настройке автоматизированной системы CMake.

Сейчас вам требуется настроить систему непрерывной интеграции для библиотек и приложений, с которыми вы работали в прошлый раз. Настройте сборочные процедуры на различных платформах:

    используйте TravisCI для сборки на операционной системе Linux с использованием компиляторов gcc и clang;
    используйте AppVeyor для сборки на операционной системе Windows.
## Примечание
В лабораторной работе используем Github Actions
## Ход работы
- Клонируем репозиторий 3 лабораторной работы в репозиторий 4
  ![](https://github.com/sippyuy/timp4/blob/main/screens/1.png)                               
  ![](https://github.com/sippyuy/timp4/blob/main/screens/2.png)                           
  ![](https://github.com/sippyuy/timp4/blob/main/screens/3.png)                        
  ![](https://github.com/sippyuy/timp4/blob/main/screens/4.png)                    
- Создаём необходимые директории для работы с Github Actions
  ![](https://github.com/sippyuy/timp4/blob/main/screens/5.png)                             
  ![](https://github.com/sippyuy/timp4/blob/main/screens/6.png)                      
  ![](https://github.com/sippyuy/timp4/blob/main/screens/7.png)                         
  ![](https://github.com/sippyuy/timp4/blob/main/screens/8.png)                     
- Создаём файл yml с кодом сборки на Linux
  ![](https://github.com/sippyuy/timp4/blob/main/screens/9.png)
  ![](https://github.com/sippyuy/timp4/blob/main/screens/10.png)
  ![](https://github.com/sippyuy/timp4/blob/main/screens/11.png)
- Создаём токен с правами доступа workflow
- Пушим изменения на гитхаб
  ![](https://github.com/sippyuy/timp4/blob/main/screens/12.png)
  ![](https://github.com/sippyuy/timp4/blob/main/screens/13.png)
  ![](https://github.com/sippyuy/timp4/blob/main/screens/14.png)
  ![](https://github.com/sippyuy/timp4/blob/main/screens/15.png)
- Создаём файл yml с кодом сборки на Windows
  ![](https://github.com/sippyuy/timp4/blob/main/screens/16.png)
  ![](https://github.com/sippyuy/timp4/blob/main/screens/17.png)
  ![](https://github.com/sippyuy/timp4/blob/main/screens/18.png)
- Пушим изменения на гитхаб
  ![](https://github.com/sippyuy/timp4/blob/main/screens/19.png)
  ![](https://github.com/sippyuy/timp4/blob/main/screens/20.png)
  ![](https://github.com/sippyuy/timp4/blob/main/screens/21.png)
  ![](https://github.com/sippyuy/timp4/blob/main/screens/22.png)
  
