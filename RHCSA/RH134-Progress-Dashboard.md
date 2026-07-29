---
title: RH134 Progress Dashboard
tags:
  - rh134
  - dashboard
  - progress
---

# 📊 RH134 Progress Dashboard

> Use this page as your central hub. Works great with **Dataview** plugin.

---

## Overall Progress

```dataview
TABLE WITHOUT ID
  file.link as Chapter,
  status as "Status",
  theory as "Theory",
  guided as "Guided",
  quiz as "Quiz/Lab",
  round(100 * (theory + guided + quiz) / 3) as "%"
FROM "RH134-Red-Hat-System-Administration-II-Checklist"
WHERE file.name = "RH134-Red-Hat-System-Administration-II-Checklist"
```

**Manual Quick View** (update the main checklist instead):

| Metric              | Value     |
|---------------------|-----------|
| Chapters Completed  | 0 / 19    |
| Labs Completed      | 0 / 8+    |
| Quizzes Done        | 0 / ~8    |
| Overall Completion  | **0%**    |

---

## Chapter Status Overview

```dataview
TABLE 
  chapter as Chapter,
  status as Status,
  notes as Notes
FROM "RH134-Red-Hat-System-Administration-II-Checklist"
FLATTEN file.lists
WHERE contains(item, "- [ ] **")
```

*(Alternative: just update the table in the main checklist file regularly)*

---

## Recent Activity

```dataview
LIST
FROM "Daily Logs"
SORT file.name DESC
LIMIT 7
```

---

## Critical Chapters (Revisit These)

- [[Chapter 6 - SELinux]]
- [[Chapter 11 - LVM]]
- [[Chapter 12 - Boot Process]]
- [[Chapter 19 - Comprehensive Review]]

---

## Efficiency Stats (fill manually or with Tasks plugin)

- Total study hours this week: 
- Labs attempted independently: 
- Errors logged & resolved: 
- Commands added to cheatsheet this week: 

---

## Quick Links

- [[RH134-Red-Hat-System-Administration-II-Checklist|Full Checklist]]
- [[RH134-Command-Cheatsheet|Command Cheatsheet]]
- [[RH134-Troubleshooting-Log|Troubleshooting Log]]
- [[RH134-Daily-Study-Log-Template|Daily Log Template]]

---

**Last Updated:** 
*Tip: Pin this page or add it to your daily note.*