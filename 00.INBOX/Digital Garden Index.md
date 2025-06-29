---
created: 2024-07-25 16:09
updated: 2025-06-10 09:55
dg-home: true
dg-publish: true
---

## 📌Welcome Here

> [!INFO]
>**本网站用于展示和分享积累在Obsidian仓库中的笔记，可以点击左侧导航栏查看笔记索引，也可以进行关键词搜索。**

---
<br>
<br>


## 📝最新编辑

```dataview
table WITHOUT ID file.link AS "笔记名称", file.mtime as "时间"
from ""
where dg-publish = true and !contains(file.path, "00.Inbox") 
sort file.mtime desc
limit 10
```

---
<br>
<br>


## 🗓️最近创建

```dataview
table WITHOUT ID file.link AS "笔记名称", file.ctime as "时间"
from ""
where dg-publish = true and !contains(file.path, "00.Inbox")
sort file.ctime desc
limit 10
```

---

<br>
<br>

## 🗂️所有笔记

```dataviewjs
for(let i of dv.pagePaths(`"/"`).groupBy(p => p.split("/")[0])){
    let pages = dv.pages(`"${i.key}"`)
                  .where(p => p["dg-publish"] === true && p.file.path !== dv.current().file.path); // 确保不包括当前笔记); // 筛选属性为 true 的笔记
    let count = pages.length;
    
    // 只有当存在符合条件的笔记时，才显示目录和笔记列表
    if(count > 0) {
        dv.paragraph(`### ${i.key}`);
        dv.paragraph(`共有==${count}==篇`);
        dv.list(
            pages
            .map(p => moment(Number(p.file.cday)).format('yyyy-MM-DD')+' >> '+ p.file.link )
        );
    }
}

```

---
<br>
<br>

## 🏷️所有标签

```dataviewjs
dv.paragraph(`
  <div style="display: flex; flex-wrap: wrap; gap: 10px;">
    ${dv.pages()
      .where(p => p["dg-publish"] === true)
      .file.etags.distinct()
      .sort(t => dv.pages()
        .where(p => p["dg-publish"] === true && p.file.etags.includes(t))
        .length, 'desc')
      .map(t => {
        let count = dv.pages()
          .where(p => p["dg-publish"] === true && p.file.etags.includes(t))
          .length;
        return `<div style="padding: 5px; background-color: #f0f0f0; border-radius: 10px; box-shadow: 0 2px 5px rgba(0,0,0,0.1);">
                  <a href="${t}" style="text-decoration: none; color: #333;">
                    <strong>${t}</strong> (${count})
                  </a>
                </div>`;
      })
      .array()
      .join("")}
  </div>
`);
```

---


