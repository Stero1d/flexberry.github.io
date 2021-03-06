---
title: Обработка множества объектов (в т.ч. и разнотипных)
sidebar: flexberry-orm_sidebar
keywords: Flexberry ORM, Public
toc: true
permalink: en/fo_processing-of-multiple-objects.html
folder: products/flexberry-orm/
lang: en
---

## Порядок обновления объектов

Для того, чтобы обновить в хранилище несколько объектов данных, будь они однотипные или разнотипные, не важно, следует вызвать метод [сервиса данных](fo_data-service.html) [`UpdateObjects`](fo_data-service.html) с параметром — одномерным массивом (типа [`DataObject`](fo_dataobject.html)) этих объектов данных. Запросы для всех переданных объектов будут выполнены в единой транзакции.

Следует помнить, что обновление объекта может вызвать [обновление связанных с ним объектов](fo_update-related-objects.html), которые явно не были указаны в массиве.

В общем случае [сервис данных](fo_data-service.html) умеет сам выстраивать порядок запросов на обновление объектов данных. Но возможны ситуации, когда для связанных объектов важен порядок следования объектов в массиве, например:

* Наличие циклов в графе типов.
В частности, при удалении объектов в случае циклических ссылок программист должен не только проконтролировать порядок объектов в массиве, но и осознанно занулить какую-нибудь ссылку.
* Удаление объекта и его мастера в одной транзакции.
Важен порядок, в котором объекты пришли в сервис данных. 
* Ситуация, когда агрегатор и детейл имеют мастера одного типа. 
* Другие варианты при наличии связанных объектов с разными [статусами](fo_object-status-and-loading-state.html), т.е. когда часть объектов добавляется, часть - обновляется, часть - удаляется.

## Общая рекомендация

В случае связанных объектов, мастеровые объекты должны располагаться вначале массива. Т.е., если имеется ситуация вида:

![](/images/pages/products/flexberry-orm/tutorial-programmer-casseberry/primer-7.jpg)

и есть объекты: a, m1, m2, m3, то правильный массив будет такой: 

```csharp
new DataObject[]){m3,m2,m1,a}
```

## Ошибочное вычисление статуса объекта после обновления

Если указанное правило размещения мастеров в массиве не соблюдается, возможно ошибочное вычисление [статуса](fo_object-status-and-loading-state.html) объекта после обновления, если до обновления объекты имели [статус](fo_object-status-and-loading-state.html) `Сreated`. 
А именно, создаём объект и мастера для него. т.е. оба объекта со [ObjectStatusAndLoadingState|статусом) `Сreated`. Отправляем объект на обновление. Однако после этого получаем 

```csharp
GetStatus() == ObjectStatus.Altered 
```
 и в [копии данных](fo_data-object-copy.html) мастера нет.

Например, создаем следующий граф объектов с мастером и агрегатором:

![](/images/pages/products/flexberry-aspnet/aspnet/model.png)

в `UpdateObjects` следует передавать объекты в определенном порядке: мастеровой объект (`Модель`) должен быть добавлен раньше, чем детейловый объект (`Автомобиль`), использующий его:

```csharp
    var objects = new List<DataObject>();

    var модель = new Модель { Наименование = "ВАЗ" };
    var клиент = new Клиент { Фамилия = "Иванов", Имя = "Петр" };
    var авто = new Автомобиль { ГодВыпуска = 2012, /*Клиент = клиент, - агрегатор проставится автоматически*/ Модель = модель };
    клиент.Автомобиль.Add(авто);
    objects.Add(модель);  // мастер добавляем раньше
    objects.Add(клиент);

    var dataObjects = objects.ToArray();
    ds.UpdateObjects(ref dataObjects);
```

В противном случае детейловые объекты (`Автомобиль`) могут иметь [статус](fo_object-status-and-loading-state.html) `Altered` из-за некорректной [копии данных](fo_data-object-copy.html).


__Замечание__:  Если [сервис данных](fo_data-service.html) является наследником [`SQLDataService`](fo_sql-data-service.html) для решения проблемы неверного автоматического определения [сервисом данных](fo_data-service.html) порядка обработки объектов предлагается использовать метод [`UpdateObjectsOrdered`](fo_sql-data-service.html), который выполняет обновление объектов последовательно в том порядке, в котором они приходят в этот метод.

