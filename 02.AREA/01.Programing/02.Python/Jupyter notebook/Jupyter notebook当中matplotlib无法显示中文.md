---
created: 2025-06-05 23:38
updated: 2025-06-10 09:54
share_link: https://share.note.sx/5mw99zu4
share_updated: 2025-06-06T10:16:15+08:00
dg-publish: true
tags:
  - garden/🌱Seedling
---
# 下载字体

https://github.com/kaienfr/Font/blob/master/font/simhei.ttf

# 找到matplotlib存放font的路径

```
from matplotlib import font_manager
for font in font_manager.fontManager.ttflist:
    print(font.name, '      ', font.fname)

```

# 将simhei.tff文件转移到路径下

# 重建字体缓存

```
import matplotlib.font_manager as fm
fm._load_fontmanager(try_read_cache=False)  # 强制重建字体缓存
```

# 重启内核即可

