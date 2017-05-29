---
title: Связь моделей с системно-технологической архитектурой приложений
sidebar: flexberry-designer_sidebar
keywords: Ключевые понятия
toc: true
permalink: en/fd_communication-models-applications.html
folder: products/flexberry-designer/about/
lang: en
---

## Логические уровни
В соответствии с [Системно-технологической архитектурой приложений](fw_flexberry-winforms-architecture.html),  имеются три логических уровня:

1. Пользовательский интерфейс
2. Бизнес-логика
3. Данные, метаданные и доступ к данным

Эти логические уровни могут быть различным образом реализованы физически в конкретной прикладной системе.

`Flexberry PlugIns` расширяют диаграмму классов `UML` таким образом, чтобы проектирование систем выполнялось в этих логических и физических уровнях.

## Данные, метаданные и доступ к данным

Для описания структуры данных прикладной системы служат [классы данных (классы со стереотипом `implementation` или без указания стереотипа, их атрибуты, методы)](fd_data-classes.html) и связи между ними ([наследование](fo_inheritance.html), [ассоциация](fd_master-association.html), [агрегация](fo_detail-associations-properties.html)). Примеры классов данных: Счёт, Банк, Контрагент и т.п.

Архитектура `Flexberry Platform` и модули - генераторы кода устроены таким образом, что с единой модели данных происходит как генерация структур данных для хранения (напр., реляционные таблицы), так и структур данных языка программирования для обработки данных (соответствующие классы и отношения между ними).

## Бизнес-логика

Для описания бизнес-логики прикладной системы (общесистемных бизнес-операций) служат т.н. [бизнес-серверы (классы со стереотипом `businessserver`)](fo_business-servers.html), совместно с которыми могут быть созданы т.н. "заглушки" для предоставления [бизнес-сервера как COM+ компоненты или как Веб-сервис, а также бизнес-фасад](fo_business-servers.html).

## Пользовательский интерфейс

Для описания пользовательского интерфейса служат: 
1. [Формы редактирования (классы со стереотипом `editform`)](fd_classes-with-stereotype-editform.html) 
2. [Формы списка (классы со стереотипом `listform`)](fw_listform.html) 
3. Пользовательские формы (классы со стереотипом `userform`) 
4. [Приложения (классы со стереотипом `application`)](fd_application.html) 

## Общие элементы модели для любых уровней

При описании модели в любом из уровней используются:

* [Интерфейсы (классы со стереотипом `interface`)](fd_interfaces.html) 
* Типы данных (указываются типами атрибутов, параметров методов и т.п.): 
    * [Синонимы типов (классы со стереотипом `typedef`)](fd_classes-with-stereotype-typedef.html), вводятся для удобства чтения моделей и удобства трансформации в код. Примеры: СтрокаНазвания, НомерТелефона, Строка255; 
    * [Перечислимые типы данных (классы со стереотипом `enumeration`)](fo_enumerations.html); 
    * Типы данных (классы со стереотипом `type`); 
    * [Внешние классы/типы (классы со стереотипом `external`)](fd_external-classes.html), не описанные на диаграммах, но известные в сгенерированном коде (например, в случае с C#, из какой-либо подключенной сборки); 
* События 
* Параметры событий ([классы со стереотипом `eventarg`](fd_classes-with-stereotype-eventarg.html)) 
* Роль в системе полномочий (классы со стереотипом `role`) 

При описании элемента, относящегося к какому-либо уровню, могут использоваться элементы других уровней, поскольку все они представляют собой, в самом общем случае, некоторые "типы", как на уровне языка моделирования, так и на уровне объектно-ориентированного языка программирования. Принципы создания структурных частей объектно-ориентированной модели и исходного кода на объектно-ориентированном языке программирования, - аналогичны. Так, например, у форм могут быть методы, возвращающие или принимающие параметры, объявленные как классы данных. Бизнес-сервисы будут иметь методы, в основном работающие с параметрами, объявленными как классы данных.

Перечисленные уровни никак не регламентируются и не навязываются при моделировании в Flexberry. Это просто логическая поясняющая группировка, помогающая понять соответствие стереотипов логическим уровням системно-технологической архитектуры. Соответственно, можно располагать классы с любыми [стереотипами](fd_key-concepts.html) на диаграммах так, как это удобно. Расположение не имеет значения.