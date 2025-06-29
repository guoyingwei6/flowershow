---
created: 2025-06-03 09:33
updated: 2025-06-10 15:00
---


```dataview
TABLE WITHOUT ID
  link(file.path, truncate(file.name, 28)) as Note,
  dateformat(share_updated, "yyyy-MM-dd") as "Shared on", 
  elink(share_link, regexreplace(share_link, "^.*?(\w+)(#.+?|)$", "$1")) as Link,
  choice(regextest("#", share_link), "🔒", "") as ""
WHERE share_link
```

