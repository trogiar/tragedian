---
created_at:
modified_at:
---

```dataview
LIST rows.file.link
FROM "inbox/daily_notes"
WHERE
	file.name != "recent"
	AND file.day > (date(now) - dur(3 mo))
GROUP BY dateformat(file.day,"yyyy-MM")
SORT rows.file.day DESC
```
