

# Задачи сегодня

```dataview
TABLE done  as "Выполнено", deadline as "Срок"
FROM "Tasks"

WHERE startdate = date(today)

    
```

# Встречи на сегодня


```dataview
TABLE done as "Выполнено", starttime as "Начало", link as "Ссылка"

WHERE date(dateformat(starttime, "yyyy-MM-dd")) = date(today)
    
```

#  Прочие задачи

```dataview
TABLE startdate  as "Начало", deadline as "Срок"
FROM "Tasks"
WHERE done = false

```


