
# Tasks List
## Jira - Issues Assigned to me
```jira-search
resolution = Unresolved AND assignee = currentUser() AND status = 'In Progress' order by priority DESC
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
done after 5 days ago
group by function task.done.moment?.isAfter(moment().subtract(7, 'days'), 'day') ? task.done.format("[%%0%% <span style='color:green;'>Completed Last week</span>]") : null
group by path
sort by path
full mode
```


# Projects
```dataview
table WITHOUT ID file.link as "Project", Status, Start_Date as "Start Date", Due_Date as "End Date", Priority, Phase, Active_Milestone as "Active Milestone", Project_Type as "Project Type", Owner/Lead
from "projects"
```


# Jira Queries
## Jira - Corporate Project Time Tracking
- JIRA:SC2S-1 
- JIRA:GU-44

## Jira Infrastructure 'IDP' Issues
```jira-search
# status = 'In Progress' AND project = GU ORDER BY Rank
query: status = 'In Progress' AND project = 'IDP'
# columns: key, summary, status, notes
columns: KEY, SUMMARY, TYPE, CREATED, UPDATED, REPORTER, ASSIGNEE, PRIORITY, STATUS, DUE_DATE, LAST_VIEWED, NOTES, 
limit: 10
```

# Tasks Lost and Found - Not done and no due dates
```tasks
not done
no due date
limit 10
```