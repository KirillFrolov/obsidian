

> [!tip] Задачи на сегодня
> ```dataview
> TABLE  WITHOUT ID  link(file.name) as "Задача",  done  as "Выполнено", deadline as "Срок"
> FROM "Tasks"
> 
> WHERE startdate = date(today)
> ```

```dataviewjs
const calendarData = {
 colors: {   // optional, defaults to green
      orange:      ["#ffa244","#fd7f00","#dd6f00","#bf6000","#9b4e00"],
      pink:        ["#ff96cb","#ff70b8","#ff3a9d","#ee0077","#c30062"]
    },
	
entries: [],            
}

const intensityData = {};

for (let page of dv.pages('"Tasks"').where(p => p.startdate)) {
    const date = new Date(page.startdate);
    const yyyy = date.getFullYear();
    let mm = date.getMonth() + 1; // Months start at 0!
    let dd = date.getDate();
    if (dd < 10) dd = '0' + dd;
    if (mm < 10) mm = '0' + mm;
    const formattedDate = yyyy + "-" + mm + '-' + dd;
	//dv.span("<br>" + formattedDate) // uncomment for troubleshooting
	if (intensityData[formattedDate]){
	intensityData[formattedDate] += 1;
	} else {
	intensityData[formattedDate] = 1
	}
	

}

for ( const [date, intensity] of Object.entries(intensityData)) {
	calendarData.entries.push({
		date: date,     // (required) Format YYYY-MM-DD
		intensity: intensity, // (required) the data you want to track
	})
}

renderHeatmapCalendar(this.container, calendarData)
``` 


> [!tip] Встречи на сегодня
> ```dataview
> TABLE  WITHOUT ID  link(file.name) as "Встреча",  done as "Выполнено", starttime as "Начало", link as "Ссылка"
> 
> WHERE date(dateformat(starttime, "yyyy-MM-dd")) = date(today)
> ```

```dataviewjs
const calendarData = {
	
entries: [],                // (required) populated in the DataviewJS loop below
}

const intensityData = {};
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
	if (intensityData[formattedDate]){
	intensityData[formattedDate] += 1;
	} else {
	intensityData[formattedDate] = 1
	}
	

}

for ( const [date, intensity] of Object.entries(intensityData)) {
	calendarData.entries.push({
		date: date,     // (required) Format YYYY-MM-DD
		intensity: intensity, // (required) the data you want to track, will 
	})
}

renderHeatmapCalendar(this.container, calendarData)
``` 
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
const calendarData = {
	
entries: [],                // (required) populated in the DataviewJS loop below
}

const intensityData = {};
//DataviewJS loop
for (let page of dv.pages('"English/Dictionary"')) {
    const date = new Date(page.file.created);
    const yyyy = date.getFullYear();
    let mm = date.getMonth() + 1; // Months start at 0!
    let dd = date.getDate();
    if (dd < 10) dd = '0' + dd;
    if (mm < 10) mm = '0' + mm;
    const formattedDate = yyyy + "-" + mm + '-' + dd;
	dv.span("<br>" + page.file) // uncomment for troubleshooting
	if (intensityData[formattedDate]){
	intensityData[formattedDate] += 1;
	} else {
	intensityData[formattedDate] = 1
	}
	

}

for ( const [date, intensity] of Object.entries(intensityData)) {
	calendarData.entries.push({
		date: date,     // (required) Format YYYY-MM-DD
		intensity: intensity, // (required) the data you want to track, will 
	})
}

renderHeatmapCalendar(this.container, calendarData)
``` 





