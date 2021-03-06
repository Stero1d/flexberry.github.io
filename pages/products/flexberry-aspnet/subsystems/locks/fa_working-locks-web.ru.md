---
title: Работа с блокировками в web-системах
sidebar: flexberry-aspnet_sidebar
keywords: Flexberry ASP-NET
toc: true
permalink: ru/fa_working-locks-web.html
folder: products/flexberry-aspnet/
lang: ru
---

Логика работы с блокировками:

* При загрузке страницы редактирования объекта блокировка устанавливается всегда (если объект не заблокирован другим пользователем), т.е. если в базе уже есть запись с просроченной блокировкой объекта, то она обновляется. Если записи нет, то она создается. Запись о блокировке актуальна в течение 30 секунд, если в течение этого времени она не продлена, то объект считается не заблокированным.
* Далее, каждые 30 секунд страница редактирования посылает ajax-запросы на продление блокировки. Через ajax-запрос можно только продлить блокировку (т.е. изменить существующую в БД запись, но не создать новую). Если записи в базе не окажется, то пользователь увидит соответствующее предупреждение и ему будет предложено открыть объект только для чтения или уйти на предыдущую страницу
* Перед созданием или продлением блокировке в обоих описанных выше пунктов, сначала проверяется, что в данный момент объект не заблокирован другим пользователем. Если окажется, что объект заблокирован, то пользователь увидит предупреждение и сможет вернуться на предыдущую страницу или открыть объект на просмотр.
* В папку `Security` добавлена форма `Lock\LockL` со списком существующих на данный момент блокировок. Удаление блокировки со списка удаляет соответствующую запись из БД.