

> [!tip] Задачи на сегодня
> ```dataview
> TABLE  WITHOUT ID  link(file.name) as "Задача",  done  as "Выполнено", deadline as "Срок"
> FROM "Tasks"
> 
> WHERE startdate = date(today)
> ```


> [!tip] Встречи на сегодня
> ```dataview
> TABLE  WITHOUT ID  link(file.name) as "Встреча",  done as "Выполнено", starttime as "Начало", link as "Ссылка"
> 
> WHERE date(dateformat(starttime, "yyyy-MM-dd")) = date(today)
> ```


> [!info] Прочие задачи
> ```dataview
> TABLE  WITHOUT ID  link(file.name) as "Задача",  startdate  as "Дата", deadline as "Срок"
> FROM "Tasks"
> WHERE done = false and startdate != date(today)
> SORT startdate
> ```


> [!info] Прочие встречи
> ```dataview
> TABLE  WITHOUT ID  link(file.name) as "Встреча",  done as "Выполнено", starttime as "Начало", link as "Ссылка"
> 
> WHERE date(dateformat(starttime, "yyyy-MM-dd")) > date(today)
> ```




