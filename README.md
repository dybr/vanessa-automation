﻿# vanessa-behavior

[![Открытый чат проекта https://gitter.im/silverbulleters/vanessa-behavoir](https://badges.gitter.im/Join Chat.svg)](https://gitter.im/silverbulleters/vanessa-behavoir?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)
[![Build Status](http://ci.silverbulleters.org/buildStatus/icon?job=Vanessa-Behavior-Develop)](http://ci.silverbulleters.org/job/Venessa-Behavior-Develop/)

## BDD for 1С:Enterprise (snipets generator and runner)

Порядок установки под Windows


* [интерпретатор Python3](https://www.python.org/) - для работы с иходными файлами 1С с помощью проекта precommit1C
* [утилита для сборки обработок 1С V8Unpack.exe](https://github.com/dmpas/v8unpack) - утилита должна быть доступной в переменной Path окружения Windows
* [утилита для формирования отчётов о проверки Allure](http://allure.qatools.ru/)

Клонируйте данный репозиторий с помощью **ms-git**

```
git clone https://github.com/silverbulleters/vanessa-behavior.git
```

Или используйте [шаблон работы по проекту 1С](https://github.com/silverbulleters/vanessa-bootstrap)

Инициализируйте подмодули репозитория с помощью **ms-git**

```
git submodule update --init --recursive
```

```
документация расширяется и накапливается на портале документации http://vanessa.services/docs/behavior

```

Обязательно ознакомьтесь с:

* руководством контрибьютора [CONTRIBUTING.md](./CONTRIBUTING.md)
* моделью спонсорства [DONATIONS.md](./DONATIONS.md)

## Описание простого использования

* пишем feature файлы в формате Gherkin (обычно используется редактор Notepad++ или связанный проект **vanessa-bdd-editor**

```Gherkin

# encoding: utf-8
# language: ru

Функционал: Запуск и получение результатов запуска сценариев
Как любой разработчик продукта
Я хочу иметь возможность запустить проверку сценариев поведения на конфигурации 1С:Предприятие

# Контекст сценария выполняется всегда перед каждым сценарием

Контекст:
Когда существует разрабатываемая мною конфигурация 1С
И существуют требования заказчика к ожидаемому поведения в каталоге ".\features"

# Каждый сценарий состоит из последовательных связанных шагов

Сценарий: Запуск в консольном режиме
Дано Пусть существует файл ".\vb-execute-profile.json"
И в переменную окружения V83PATH установлено значение "C:\Program Files (x86)\1cv8\8.3.6.2151\bin\1cv8.exe"
Когда я запускаю командную строку '%V83PATH% /Execute .\vanessa-behavior.epf /C"StartFeaturePalyer;VBParams=.\vb-execute-profile.json'
Тогда появляется файл с результатами '.\BuildStatus.log'
И в каталоге ".\allurereport" существует HTML отчет о результатах проверки сценариев

Сценарий: Запуск в интерактивном режиме
Дано Пусть я открыл обработку "vanessa-behavior.epf"
Когда Я нажал кнопку "Загрузить фичи из каталога"
И указал каталог с требованиями заказчика равным ".\features"
И затем нажал кнопку "Сгенерировать шаблоны обработок"
Также в каталоге ".\features" возникли epf файлы идентичные имени feature файла
И при нажатии кнопки "Запустить сценарии" я вижу автоматизированный запуск обработок с признаком "pending" (ожидает реализации)
```

Инструкции сгруппированы [в плейлисте YouTube](https://www.youtube.com/playlist?list=PL2zlgf113YhFG_uRARjDtP1_Obj55UmY4) 

### Классический вариант использования (без интерактивного режима)

Фактически классический вариант использования представляет собой следующий рутинный порядок

* зафиксировали требования к информационной системе
* создали автоматизированные сценарии проверки в виде epf файлов
* наполнили шаги сценариев (сниппеты) кодом проверки поведения
* запустили сценарии проверки поведения и убедились, что они НЕ работают
* разработали функционал
* запустили сценарии проверки поведения
* убедились что сценарии проверки работают и отчет о проверки показывает "Зелёный" статус

### Использования в режиме проверки поведения пользовательского интерфейса

для команд уже имеющих функционал, или производящих доработку типовых конфигураций в режиме Taxi действует упрощенный порядок использования

* зафиксировали требования к информационной системе
* создали автоматизированные сценарии проверки в виде epf файлов
* разработали управляемые формы или рабочие столы конфигурации в режиме прототипирования
* запустили запись интерактивных действий пользователя в режиме **менеджера тестирования**
* получившимся кодом наполнили обработки проверки поведения
* дополнили код проверки, кодом проверки данных если это необходимо
* разработали основной функционал
* запустили сценарии проверки поведения
* убедились что сценарии проверки работают и отчет о проверки показывает "Зелёный" статус

## Кто пишет feature файлы ?

Обратите внимание, что фактически feature файлы могут писать все участники команды:

* менеджер проекта - если обнаружил что заказчику необходимо новое поведение
* бизнес или системный аналитик - на основе собранных требований и технических заданий
* ведущий разработки - если обнаружил, что требования недостаточно структурированы
* архитектор или эксперт 1С - если текущие сценарии некорректно спроектированы с точки зрения метаданных

Для редактирования feature файлов используется проект [По автоматизации сбора требований](https://github.com/silverbulleters/vanessa-bdd-editor) - на текущий момент имеет статус *pre-alpha*

Если вы не уверены в правильности ожидаемого поведения, используйте для этого системы тэгов, как то:

* "@Draft@"  - черновик требования
* "@Предварительно" - начальные заметки

и подобные им обозначения

## Файл профиля запуска обработки

Для запуска в консольном режиме используется понятие *профиль консольного запуска*. Профиль консольного запуска предназначен для удобной передачи параметров. Профиль запуска представляет собой текстовый файл в формате JSON.

Текущие параметры запуска:

* **Каталог фич** - каталог где собраны требования заказчика описанные на языке Gherkin
* **ВыполнитьСценарии** - признак того, что необходимо запустить выполнение сценариев
* **ДелатьОтчетВФорматеАллюр** - признак того, что необходимо формировать HTML отчёт о результатах проверки
* **КаталогOutputAllureБазовый** - адрес каталога для где будет формироваться HTML отчёт
* **ЗавершитьРаботуСистемы** - признак того, что окончанию работы необходимо завершить работу 1С предприятия
* **ВыгружатьСтатусВыполненияСценариевВФайл** - признак, что необходимо формировать файл с финальным статусом проверки
* **ПутьКФайлуДляВыгрузкиСтатуасВыполненияСценариев** - по данному пути будет сформирован файл со статусом проверки (обычно используется на серверах сборки для автоматизированного указания статуса сборки)
* **СписокТеговИсключение** - массив текстовых тэгов, для исключения из проверки (используется например для черновиков сценариев и требований)

Пример подобного JSON файла профиля:

```
{
"КаталогФич": "C:\vanessa-behavior\features",
"ВыполнитьСценарии": "Истина",
"ДелатьОтчетВФорматеАллюр": "Истина",
"КаталогOutputAllureБазовый": "C:\allurereport",
"ЗавершитьРаботуСистемы": "Истина",
"ВыгружатьСтатусВыполненияСценариевВФайл": "Истина",
"ПутьКФайлуДляВыгрузкиСтатусаВыполненияСценариев": "C:\BuildStatus.log",
"СписокТеговИсключение":[
"IgnoreOnCIMainBuild",
"Draft",
]
}
```

Профиль запуска предназначен для простого консольного запуска, пример подобной командной строки выглядит так:

```
%V83PATH% /Execute C:\vanessa-behavior\vanessa-behavior.epf /C"StartFeaturePlayer;VBParams=C:\VBParams.json"
```

## Создается при поддержке

как попасть в этот раздел [./DONATIONS.md]

## Замечания:

* в процессе подготовки редакции 1.0 идут активные изменения, вследствие чего обратная совместимость с редакциями ниже 1.0 может не соблюдаться
* пожелания к использованию можно фиксировать в виде Github Issues
* структура каталогов проекта соответствует шаблону https://github.com/silverbulleters/vanessa-bootstrap

## Известные публикации

* [Первичная публикация](http://habrahabr.ru/post/252473/)
* [Пример отчета Allure на основе тестов](http://youtu.be/982gF1wY8sM)

## Вдохновение черпается

* [Огурец](https://cukes.info/)
* [Автоматизированное тестирование 1С](http://v8.1c.ru/overview/Term_000000816.htm)
* [Yandex Allure](http://allure.qatools.ru/)
* [Silenium](http://docs.seleniumhq.org/)
* [ТРИЗ](https://ru.wikipedia.org/wiki/Теория_решения_изобретательских_задач)
* [Дэн Норт](http://en.wikipedia.org/wiki/Acceptance_test-driven_development)

## Заметки для желающих поучаствовать в доработке

* мы используем подход git-flow для реализации функциональности
* мы используем precommit1c для фиксации исходников Epf обработки в git
* мы используем принцип самопроверки через feature файлы, поэтому перед разработкой новой функциональности мы также - разрабатываем feature файлы, генерируем шаблоны сценариев и наполняем их кодом для проверки. Поэтому к доработкам без feature файлов мы относимся "холодно".

более подробно в файле [CONTRIBUTING.md](https://github.com/silverbulleters/vanessa-behavior/blob/develop/CONTRIBUTING.md)


## Лицензии

* основная лицензия продукта - BSD v3
* лицензии стороннего кода - Apache License, GitHub CLA, Freeware, etc

## Поддержка OpenSource команды

* мы используем https://salt.bountysource.com/checkout/amount?team=silverbulleters

## Enterprise Support

платная подддержка содержит в себе

* обучение навыкам работы с BDD при разработке на 1С
* обучение навыкам написания на языке Gherkin
* обучение навыкам написания сценариев проверки поведения

для заказа платной поддержки необходимо отравить заявку на адрес education@silverbulleters.org


[![ZenHub] (https://raw.githubusercontent.com/ZenHubIO/support/master/zenhub-badge.png)] (https://zenhub.io)
