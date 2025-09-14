
## ToDo

```dataview
TASK
WHERE
	!completed
	OR (this.file.day AND date(completion) >= date(this.file.day))
GROUP BY file.link
```

## Journal

## Today's New Notes

```dataview
LIST WITHOUT ID file.folder + ": " + file.link
WHERE file.path != this.file.path
  AND (
    (file.day = null AND date(file.cday) = this.file.day)
    OR (file.day != null AND file.day = this.file.day)
  )
SORT file.path ASC
```

## Today's Updated Notes

```dataview
LIST WITHOUT ID file.folder + ": " + file.link
WHERE file.path != this.file.path
  AND date(file.mday) = this.file.day
  AND date(file.cday) != this.file.day
  AND file.day != this.file.day
SORT file.path ASC
```
