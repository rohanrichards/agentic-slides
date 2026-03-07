---
Status: 🟦 In Progress
tags:
  - projects/claude-workflows
deadline:
cssclasses:
  - wide-callout
summary:
---
> **Status:** `INPUT[inlineSelect(option(🟪 Not Started), option(🟦 In Progress), option(🟨 Paused), option(🟥 Blocked), option(🟩 Completed), defaultValue(🟪 Not Started)):Status]`
> **Due Date:** `INPUT[datePicker():deadline]`
> **Summary:** `INPUT[text():summary]`

## Useful Links
-

## ✅ Objectives / Deliverables
- [ ] Objective 1 #ignore

## 🧠 Notes / Context

### Starting
- try out /init to set claude up
- use claude.md committed and collaborate on it, this is a living doc
- use claude.local.md for personal customizations, not checked in

---

> [!multi-column]
>
> > [!info] 📋 Open Tasks
> > ```tasks
> > not done
> > filter by function task.tags.some(t => query.file.tags.includes(t))
> > short mode
> > ```
>
> > [!note] 📝 Meeting Notes
> > ```dataview
> > table file.frontmatter.summary as "Summary", file.ctime as "Created"
> > from "03. Resources/Meeting Notes"
> > where (
> >   any(
> >     filter(this.file.etags, (t) => t != "#ignore"),
> >     (t) => contains(file.etags, t)
> >   )
> >   or contains(file.outlinks, this.file.link)
> > )
> > sort file.ctime desc
> > ```

> [!multi-column]
> > [!tldr] Related Areas
> > ```dataview
> > table join(slice(split(file.folder, "/"), 1), "/") as "Path"
> > from "02. Areas"
> > where (
> >   any(
> >     filter(this.file.etags, (t) => t != "#ignore"),
> >     (t) => contains(file.etags, t)
> >   )
> >   or contains(file.outlinks, this.file.link)
> > )
> > sort file.ctime desc
> > ```
>
> > [!info] 📋 Related Resources
> > ```dataview
> > table join(slice(split(file.folder, "/"), 1), "/") as "Path"
> > from "03. Resources" and -"03. Resources/Meeting Notes"
> > where (
> >   any(
> >     filter(this.file.etags, (t) => t != "#ignore"),
> >     (t) => contains(file.etags, t)
> >   )
> >   or contains(file.outlinks, this.file.link)
> > )
> > sort file.ctime desc
> > ```
