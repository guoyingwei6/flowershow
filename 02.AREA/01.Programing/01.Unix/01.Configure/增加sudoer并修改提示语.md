---
created: 2024-11-14 16:28
updated: 2025-06-10 09:55
dg-publish: true
---


## 一、增加新的超级用户

```shell
sudo visudo
```

下面增加一行：
```shell

# User alias specification

# Cmnd alias specification

# User privilege specification
root    ALL=(ALL:ALL) ALL
guoyingwei      ALL=(ALL:ALL) ALL

```

## 二、修改sudo提示语

### 1. 修改提示语内容

假设我们想要输入的提示语内容为：

> We trust you have received the usual lecture from the local system administrator. It usually boils down to these three things:  
> #1) Respect the privacy of others.  
> #2) Think before you type.  
> #3) With great power comes great responsibility.

我们将内容保存到了目录：`/etc/sudoers.lecture`文件中。

现在我们只需要在`/etc/sudoers`文件中，使用命令`visudo`添加下面的这行

```bash
Defaults        lecture_file = /etc/sudoers.lecture
```

### 2. 修改提示语出现次数

同样是修改`/etc/sudoers`文件，使用命令`visudo`添加下面的这行

```bash
Defaults        lecture = always
```

其中always也可以分别被替换成：

1. always（总是，每次输入sudo都会出现提示语）
2. once（只有用户第一次使用sudo才会显示）
3. never（永远不显示提示语）


