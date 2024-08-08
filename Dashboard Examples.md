
- links 
[Dataview Example Vault (s-blu.github.io)](https://s-blu.github.io/obsidian_dataview_example_vault/)
[Dataview in Obsidian: A Beginner's Guide - Obsidian Rocks](https://obsidian.rocks/dataview-in-obsidian-a-beginners-guide/)



```dataview
table clock-in, (dateformat(date(clock-in) + dur(8 hours 30 minutes), "hh:mm")) as "Expected Clock-out"
from "daily"
where clock-in
```


# Clock in-plus 8h-30m-plus overtime-plus breaks
```dataview
table clock-in as "Clock-In", breaks as Breaks, overtime as "Over Time", (dateformat(date(clock-in) + dur(8 hours 30 minutes) + dur(overtime) - dur(breaks), "hh:mm")) as "Expected Clock-out"
from "daily"
where clock-in
```


# Clock in-plus 8h30m-minus overtime-plus breaks
```dataview
table clock-in as "Clock-In", breaks as Breaks, overtime as "Over Time", (dateformat(date(clock-in) + dur(8 hours 30 minutes) - dur(overtime) + dur(breaks), "hh:mm")) as "Expected Clock-out"
from "daily"
where clock-in
```



# Clock in-plus 8h30m     JUST display overtime and breaks - Bonus where date filtering
```dataview
table clock-in as "Clock-In", breaks as Breaks, overtime as "Over Time", (dateformat(date(clock-in) + dur(8 hours 30 minutes), "hh:mm")) as "Expected Clock-out"
from "daily"
where clock-in
where clock-in and file.day = date(today)
```


Banked Hours by last 7 days
```dataview
TABLE WITHOUT ID
	sum(rows.overtime) as "Banked Hours"
FROM "daily"
GROUP BY ""
where date(today) - date(file.ctime) <= 7
```


Banked and break Hours by last 30 days
```dataview
TABLE WITHOUT ID
	sum(rows.overtime) as "Banked Hours",
	sum(rows.breaks) as "total breaks"
FROM "daily"
GROUP BY ""
where date(today) - date(file.ctime) <= 30
```


# Trying to get 3 columns, but aggregated values, not multiple rows
```dataview
TABLE WITHOUT ID
	sum(rows.overtime) as "Banked Hours",
	sum(rows.breaks) as "total breaks",
	sum((rows.overtime).hours - sum(rows.breaks).hours) as "Total"
FROM "daily"
WHERE date(today) - date(file.ctime) <= 30
```


```dataview
TABLE WITHOUT ID
	sum(dur(overtime).hours - dur(reclaimed).hours) as "Net Banked Hours"
FROM "daily"
WHERE date(today) - date(file.ctime) <= 7
```


```dataview
TABLE greenStyle + author + styleEnd AS Author, 
    genres, 
    rightAlignStyle + totalPages + styleEnd AS "Total Pages"
FROM "10 Example Data/books"
FLATTEN "<span style='color: white;'>" AS greenStyle
FLATTEN "<span style='display:flex; justify-content: right;'>" AS rightAlignStyle
FLATTEN "</span>" AS styleEnd
```




```tasks
not done
has due date
sort by priority
sort by due
sort by scheduled
group by function task.happens.moment? (task.due.moment?.isBefore(moment(), 'day') ? task.due.format("[%%0%% <span style='color:red;'>!!Overdue</span>]") : \
(task.due.moment?.isBetween(moment(), moment(), 'day', '[]') ? task.due.format("[%%1%% <span style='color:orange; font-size:42px;'>Due Today</span>]") : \
(task.due.moment?.isSame(moment().add(1, 'day'), 'day') ? task.due.format("[%%2%%📆 <span style='color:yellow; font-size:32px;'>Due Tomorrow</span>]") : \
(task.happens.moment?.isBefore(moment().add(7, 'days'), 'day') ? task.happens.format("[%%3%%⬆️ Upcoming]") : \
task.happens.format("[%%4%%💤Beyond Seven Days]"))))) : "No Date"

limit groups 15
full mode
hide start date
hide created date
hide scheduled date
hide start date
heading does not include Backlog
```



```tasks
done after 7 days ago
group by function task.done.moment?.isAfter(moment().subtract(7, 'days'), 'day') ? task.done.format("[%%0%% <span style='color:green;'>Completed Last week</span>]") : null
group by path
sort by path
full mode
```

# Tasks Lost and Found
```tasks
not done
no due date
limit 10
```





```dataview
CALENDAR file.day
FROM "daily"
```