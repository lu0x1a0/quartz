


# How many checked

[Help with using Dataview to get task count - Help - Obsidian Forum](https://forum.obsidian.md/t/help-with-using-dataview-to-get-task-count/47525/4)
[Expressions - Dataview (blacksmithgu.github.io)](https://blacksmithgu.github.io/obsidian-dataview/reference/expressions/)

```dataview
TABLE WITHOUT ID 
	key as Tasks, 
	length(filter(rows.tasks, (t)=>!t.parent)) AS Total, 
	length(filter(rows.tasks, (r) => r.completed)) AS Applied 
FROM "Job Search/Job Apply Checklist" FLATTEN file.tasks as tasks 
WHERE !tasks.parent
GROUP BY Applied 
SORT length(rows) DESC
```

*program need to be of format Year/Name*

LastTaskCompleted sort is glitchy, dunno why
# Staging
```dataview
TABLE without ID
	tasks.children[0].firm as "Firm",
	tasks.children[0].program as "Program",
	dateformat(tasks.completion,"yy-MM-dd") as "Applied Date",
	tasks.children[0].salaryEst as "Salary Estimate",
	tasks.children[0].packageEst as "Package Estimate",
	date(max(map(
		filter(
			tasks.children, (child)=>(child.completion)
		),(child)=>(child.completion)
	))) as LastTaskCompleted,
	map(
		filter(tasks.children, (child)=>(child.task)),
		(subtask) => (subtask.text) 
	) as Stages
FROM "Job Search/Job Apply Checklist" FLATTEN file.tasks as tasks
WHERE !tasks.parent 
SORT map(
		filter(tasks.children, (child)=>(child.task)),
		(subtask) => (subtask.due) 
	) DESC,
	date(max(map(
		filter(
			tasks.children, (child)=>(child.completion)
		),(child)=>(child.completion)
	))) DESC,
	tasks.completion ASC
```

## TODO: 
- sort offer (if any) by salary and WLB
