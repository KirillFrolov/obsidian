

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

---

> [!info] Прочие задачи
> ```dataview
> TABLE  WITHOUT ID  link(file.name) as "Задача",  startdate  as "Дата", deadlinedate as "Срок"
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


[[English words]]

```dataviewjs
dv.span("** Tasks Heatmap **") 
const calendarData = {
	
entries: [],                // (required) populated in the DataviewJS loop below
}

//DataviewJS loop
for (let page of dv.pages('"Tasks"').where(p => p.weight)) {
	dv.span("<br>" + page.startdate) // uncomment for troubleshooting
	calendarData.entries.push({
		date: page.startdate,     // (required) Format YYYY-MM-DD
		intensity: page.weight, // (required) the data you want to track, will 
		content: await dv.span(`[](${page.file.name})`)
	})
}

renderHeatmapCalendar(this.container, calendarData)
``` 


