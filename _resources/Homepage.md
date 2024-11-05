

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
dv.span("** Tasks Heatmap **") /* optional ⏹️💤⚡⚠🧩↑↓⏳📔💾📁📝🔄📝🔀⌨️🕸️📅🔍✨ */
const calendarData = {
	
	showCurrentDayBorder: true, // (optional) defaults to true
	defaultEntryIntensity: 4,   // (optional) defaults to 4
	intensityScaleStart: 10,    // (optional) defaults to lowest value passed to entries.intensity
	intensityScaleEnd: 100,     // (optional) defaults to highest value passed to entries.intensity
	entries: [],                // (required) populated in the DataviewJS loop below
}

//DataviewJS loop
for (let page of dv.pages('"Tasks"').where(p => p.done)) {
	//dv.span("<br>" + page.file.name) // uncomment for troubleshooting
	calendarData.entries.push({
		date: page.startdate,     // (required) Format YYYY-MM-DD
		intensity: page.exercise, // (required) the data you want to track, will map color intensities automatically
		content: "🏋️",           // (optional) Add text to the date cell
		color: "orange",          // (optional) Reference from *calendarData.colors*. If no color is supplied; colors[0] is used
	})
}

renderHeatmapCalendar(this.container, calendarData)
``` 


