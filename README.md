### Задача №1 - Максимальное покрытие
Вам нужно взять уже полюбившуюся вам функцию расчёта комиссии при переводе и написать для неё автотесты:
![](https://github.com/netology-code/kt-homeworks/raw/master/04_functions/pic/vk-commission.png)

Подключите JUnit4 и JaCoCo и добейтесь того, чтобы покрытие кода по branch'ам было не менее 80%:
![](https://github.com/netology-code/kt-homeworks/raw/master/04_functions/pic/branches.png)

Информацию о том, что значит по branch'ам вы найдёте на [официальном сайте JaCoCo](https://www.eclemma.org/jacoco/trunk/doc/counters.html).

### Задача №2 - CI
Запускать тесты на своём компьютере - хорошо, а запускать их при каждом пуше в облаке – ещё лучше. Когда вы будете работать в команде, сразу будет видно, кто "сломал" сборку, а кто прислал "нерабочий PR (Pull-Request)". В этом вся прелесь командной работы!

Мы настроим CI на базе GitHub Actions - уже встроенной в GitHub системы.

После того, как вы сделали задачу №1, перейдите в ваш репозиторий на вкладку GitHub Actions:

GitHub Actions предложить вам настроить Simple Workflow, соглашаемся:
![](https://github.com/netology-code/kt-homeworks/raw/master/04_functions/pic/actions2.png)

В открывшемся окне делаем по шагам:
- Меняем имя файла на `gradle.yml`
- Меняем весь код, на тот что ниже
- Нажимаем "Start Commit"
- Нажимаем "Commit new file"

![](https://github.com/netology-code/kt-homeworks/raw/master/04_functions/pic/actions3.png)

Код для build.gradle (скопируйте и замените целиком - не переписывайте вручную):
```yml
name: Kotlin CI with Gradle

on:
push:
branches: [ master ]
pull_request:
branches: [ master ]

jobs:
build:

    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v2
    - name: Set up JDK 1.8
      uses: actions/setup-java@v1
      with:
        java-version: 1.8
    - name: Grant execute permission for gradlew
      run: chmod +x gradlew
    - name: Build with Gradle
      run: ./gradlew build --info
```
"Everything as code" и "Configuration as code" - большинство современных CI систем следуют этому подходу, когда все необходимые настройки хранятся в текстовом файле в самом репозитории. В случае GitHub Actions в вашем репозитории создастся файл `./github/workflows/gradle.yml`, поэтому не забудьте сделать git pull, чтобы он попал и в ваш локальный репозиторий.

Если вы скопируете конфигурацию неправильно, GitHub Actions сообщит вам об этом (мы удалили у первого слова первую букву):
![](https://github.com/netology-code/kt-homeworks/raw/master/04_functions/pic/actions4.png)

После того, как вы закоммитите корректный файл конфигурации, на вкладке GitHub Actions можно посмотреть прогресс выполнения:
![](https://github.com/netology-code/kt-homeworks/raw/master/04_functions/pic/actions5.png)

Через какое-то время получим общий итог: PASS (зелёный флажок) или FAIL (красный крестик):
![](https://github.com/netology-code/kt-homeworks/raw/master/04_functions/pic/actions6.png)

Для каждого коммита, для которого производилась сборка, на всех страницах GitHub будет отображаться статус (иконки статуса кликабельны):
![](https://github.com/netology-code/kt-homeworks/raw/master/04_functions/pic/actions7.png)

Можно "провалиться" внутрь, чтобы посмотреть логи сборки:
![](https://github.com/netology-code/kt-homeworks/raw/master/04_functions/pic/actions8.png)

И раскрыть интересующий нас раздел:
![](https://github.com/netology-code/kt-homeworks/raw/master/04_functions/pic/actions9.png)

Если мы специально или случайно уроним сборку, то это тоже будет видно:
![](https://github.com/netology-code/kt-homeworks/raw/master/04_functions/pic/actions10.png)

Полностью настроенный CI на основе примеров с лекции вы можете найти по адресу: [https://github.com/netology-code/ktci](https://github.com/netology-code/ktci).

#### Задача
Соответственно, ваша задача - подключить к вашему репозиторию GitHub Actions, следуя пошаговой инструкции, которая была приведена выше.

Чтобы удостовериться, что CI действительно работает - добавьте (как мы в примере) коммит, ломающий сборку (выставьте в тестах неправильное ожидаемое значение). Убедитесь, что после Push'а вам покажут эту проблему.

**Важно**: вам не нужно каждый раз создавать заново файл конфигурации GitHub Actions, достаточно добавлять его в новый репозиторий так же, как вы это делаете с `.gitignore`.