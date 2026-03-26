---
type: dashboard
date: 2026-03-27
status: active
tags: [meta, dashboard]
---
# Second Brain Dashboard

> Live queries powered by Dataview. Refreshes automatically.

---

## Open Threads (all projects)

```dataview
TASK
WHERE !completed
FROM "01-Projects" OR "02-Areas"
SORT file.mtime DESC
```

---

## Active Projects

```dataview
TABLE status, date AS "Updated"
FROM "01-Projects"
WHERE type = "project-note" AND status = "active"
SORT date DESC
```

---

## Recent Session Digests

```dataview
TABLE project, date
FROM "99-Meta/session-digests"
WHERE type = "session-digest"
SORT date DESC
LIMIT 10
```

---

## Files Modified This Week

```dataview
TABLE file.mtime AS "Modified"
FROM ""
WHERE file.mtime >= date(today) - dur(7 days)
AND file.name != "dashboard"
SORT file.mtime DESC
LIMIT 20
```

---

## Competitor Coverage

```dataview
TABLE status, date AS "Last Updated"
FROM "02-Areas/competitors"
WHERE type = "competitor-note"
SORT date DESC
```

---

## SEO Pages

```dataview
TABLE status, date AS "Date"
FROM "02-Areas/seo"
WHERE type = "seo-page" OR type = "seo-note"
SORT date DESC
```
