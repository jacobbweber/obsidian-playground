---
clock-in: 
breaks: 
overtime: 
reclaimed:
---

# Time Tracking

```dataview
TABLE WITHOUT ID
	sum(rows.overtime) as "Banked Hours"
FROM "daily"
GROUP BY ""
where date(today) - date(file.ctime) <= 7
```

```dataview
table clock-in as "Clock-In", breaks as Breaks, overtime as "Over Time", (dateformat(date(clock-in) + dur(8 hours 30 minutes), "hh:mm")) as "Expected Clock-out"
WHERE file.name = this.file.name
```




# **_Daily Work Logs_**


## Admin:

- Example
- [ ] willy doody



## Engineer:

- Example


## On-Call:

- Example


# Carry over
