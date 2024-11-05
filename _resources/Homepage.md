

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
for (let page of dv.pages('"Tasks"').where(p => p.startdate)) {
    const date = new Date(page.startdate);
    const yyyy = date.getFullYear();
    let mm = date.getMonth() + 1; // Months start at 0!
    let dd = date.getDate();
    if (dd < 10) dd = '0' + dd;
    if (mm < 10) mm = '0' + mm;
    const formattedDate = yyyy + "-" + mm + '-' + dd;
	//dv.span("<br>" + formattedDate) // uncomment for troubleshooting
	
	calendarData.entries.push({
		date: formattedDate,     // (required) Format YYYY-MM-DD
		intensity: 1, // (required) the data you want to track, will 
	})
}

renderHeatmapCalendar(this.container, calendarData)
``` 


```dataviewjs
dv.span("** Meetings Heatmap **") 
const calendarData = {
	
entries: [],                // (required) populated in the DataviewJS loop below
}

//DataviewJS loop
for (let page of dv.pages('"Meetings"').where(p => p.starttime)) {
    const date = new Date(page.starttime);
    const yyyy = date.getFullYear();
    let mm = date.getMonth() + 1; // Months start at 0!
    let dd = date.getDate();
    if (dd < 10) dd = '0' + dd;
    if (mm < 10) mm = '0' + mm;
    const formattedDate = yyyy + "-" + mm + '-' + dd;
	//dv.span("<br>" + formattedDate) // uncomment for troubleshooting
	
	calendarData.entries.push({
		date: formattedDate,     // (required) Format YYYY-MM-DD
		intensity: 1, // (required) the data you want to track, will 
	})
}

renderHeatmapCalendar(this.container, calendarData)
``` 
