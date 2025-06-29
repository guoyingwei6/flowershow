---
created: 2024-06-04 20:58
updated: 2025-06-10 09:55
dg-publish: true
tags:
  - garden/🌱Seedling
---

在Linux系统中，`/etc/shadow` 文件是存储用户密码哈希及相关安全信息的关键文件，比传统的 `/etc/passwd` 更安全。以下是详细解释：

---

### **1. 文件结构**

每行对应一个用户账户，格式为（用 `:` 分隔的9个字段）：

```
username:password_hash:last_changed:min_age:max_age:warn:inactive:expire:reserved
```

示例：

```
alice:$6$salt$hashed_password:18627:0:99999:7:::
```

---

### **2. 字段详解**

|字段|说明|示例值含义|
|---|---|---|
|**username**|用户名|`alice`|
|**password_hash**|密码哈希值|`$6$salt$hashed_password`|
|**last_changed**|上次修改密码的天数（从1970-1-1起）|`18627`（2021年1月1日）|
|**min_age**|密码最短使用天数（0=可随时修改）|`0`|
|**max_age**|密码最长有效期（99999=永不过期）|`99999`|
|**warn**|密码到期前警告天数|`7`（提前7天警告）|
|**inactive**|密码过期后宽限天数|空=立即失效|
|**expire**|账户绝对过期日期（1970-1-1起）|空=永不过期|
|**reserved**|保留字段|未使用|

---

### **3. 密码哈希格式**

常见格式（以 `$` 分隔）：

```
$id$salt$hashed_password
```

- **`id`** 算法标识：
    - `1` = MD5
    - `5` = SHA-256
    - `6` = SHA-512（最安全）

---

### **4. 安全特性**

- **仅root可读**：防止普通用户获取哈希值进行破解。
- **密码锁定**：用 `!` 或 `*` 开头表示账户被锁定（如 `!$6$...`）。
- **空密码**：字段为 `!!` 或 `*` 时禁止登录。

---

### **5. 相关命令**

```bash
# 修改密码（会更新shadow文件）
sudo passwd username

# 查看用户密码状态
sudo chage -l username
```