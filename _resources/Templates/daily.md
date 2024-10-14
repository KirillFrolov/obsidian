<span style="font-size:12px; color:#888888;">Created: <% tp.date.now("DD.MM.YYYY HH:mm")%></span>

# Задачи

```dataview
TABLE done  as "Выполнено", deadline as "Срок"

WHERE startdate = date(

    split(this.file.name, "\.")[2] + "-" +

    split(this.file.name, "\.")[1] + "-" +

    split(this.file.name, "\.")[0]

)
    
```


# Встречи


```dataview
TABLE done as "Выполнено", starttime as "Начало", link as "Ссылка"

WHERE date(dateformat(starttime, "yyyy-MM-dd")) = date(

    split(this.file.name, "\.")[2] + "-" +

    split(this.file.name, "\.")[1] + "-" +

    split(this.file.name, "\.")[0]
    )
    
```


