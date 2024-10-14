

# Задачи сегодня

```dataview
TABLE done  as "Выполнено", deadline as "Срок"

WHERE startdate = date.now

    
```

`
# Встречи на сегодня


```dataview
TABLE done as "Выполнено", starttime as "Начало", link as "Ссылка"

WHERE date(dateformat(starttime, "yyyy-MM-dd")) = date.now
    
```

#  Прочие задачи

