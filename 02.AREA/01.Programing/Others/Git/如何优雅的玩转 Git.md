---
title: 如何优雅的玩转 Git
tags:
  - garden/🌿Budding
created: 2024-06-04 20:58
updated: 2025-06-10 09:55
dg-publish: true
---

## Git 简介

### Git 是什么

Git 是一个开源的分布式版本控制系统。

Git 和其它版本控制系统（包括 Subversion 和近似工具）的主要差别在于 Git 对待数据的方式。 从概念上来说，其它大部分系统以文件变更列表的方式存储信息，而 Git 是把数据看作是对小型文件系统的一系列快照。

### 什么是版本控制

版本控制是一种记录一个或若干文件内容变化，以便将来查阅特定版本修订情况的系统。

#### 集中化的版本控制系统

介绍分布式版本控制系统前，有必要先了解一下传统的集中式版本控制系统。

**集中化的版本控制系统**，诸如 CVS，Subversion 等，都有一个单一的集中管理的服务器，保存所有文件的修订版本，而协同工作的人们都通过客户端连到这台服务器，取出最新的文件或者提交更新。

这么做最显而易见的缺点是中央服务器的单点故障。如果宕机一小时，那么在这一小时内，谁都无法提交更新，也就无法协同工作。要是中央服务器的磁盘发生故障，碰巧没做备份，或者备份不够及时，就会有丢失数据的风险。最坏的情况是彻底丢失整个项目的所有历史更改记录。

![img](https://git-scm.com/figures/18333fig0102-tn.png)

#### 分布式版本控制系统

**分布式版本控制系统**的客户端并不只提取最新版本的文件快照，而是把代码仓库完整地镜像下来。这么一来，任何一处协同工作用的服务器发生故障，事后都可以用任何一个镜像出来的本地仓库恢复。因为每一次的提取操作，实际上都是一次对代码仓库的完整备份。

![img](https://git-scm.com/figures/18333fig0103-tn.png)

### 为什么使用 Git

Git 是分布式的。这是 Git 和其它非分布式的版本控制系统（例如 svn，cvs 等），最核心的区别。分布式带来以下好处：

- **工作时不需要联网** - 首先，分布式版本控制系统根本没有“中央服务器”，每个人的电脑上都是一个完整的版本库，这样，你工作的时候，就不需要联网了，因为版本库就在你自己的电脑上。既然每个人电脑上都有一个完整的版本库，那多个人如何协作呢？比方说你在自己电脑上改了文件 A，你的同事也在他的电脑上改了文件 A，这时，你们俩之间只需把各自的修改推送给对方，就可以互相看到对方的修改了。
- **更加安全**
  - 集中式版本控制系统，一旦中央服务器出了问题，所有人都无法工作。
  - 分布式版本控制系统，每个人电脑中都有完整的版本库，所以某人的机器挂了，并不影响其它人。

### Git 的工作原理

个人认为，对于 Git 这个版本工具，再不了解原理的情况下，直接去学习命令行，可能会一头雾水。所以，本文特意将原理放在命令使用章节之前讲解。

#### 版本库

当你一个项目到本地或创建一个 git 项目，项目目录下会有一个隐藏的 `.git` 子目录。这个目录是 git 用来跟踪管理版本库的，如果不熟悉其工作机制，千万不要手动修改。

![img](https://raw.githubusercontent.com/dunwu/images/master/snap/20210419093855.png)

- `hooks` 目录：包含客户端或服务端的钩子脚本（hook scripts）
- `info` 目录：包含一个全局性排除（global exclude）文件， 用以放置那些不希望被记录在 `.gitignore` 文件中的忽略模式（ignored patterns）。
- `objects` 目录：存储所有数据内容。
- `refs` 目录：存储指向数据（分支、远程仓库和标签等）的提交对象的指针
- `HEAD` 文件：指向目前被检出的分支。
- `index` 文件保存暂存区信息。
- `config` 文件：包含项目特有的配置选项。
- `description` 文件：`description` 文件仅供 GitWeb 程序使用，我们无需关心。

#### 哈希值

Git 中所有数据在存储前都计算校验和，然后以校验和来引用。 这意味着不可能在 Git 不知情时更改任何文件内容或目录内容。 这个功能构筑在 Git 底层，是 Git 的关键组件。 若你在传送过程中丢失信息或损坏文件，Git 就能发现。

Git 计算校验和的使用 SHA-1 哈希算法。 这是一个由 40 个十六进制字符（0-9 和 a-f）组成字符串，基于 Git 中文件的内容或目录结构计算出来。 SHA-1 哈希值看起来是这样：

```shell
24b9da6552252987aa493b52f8696cd6d3b00373
```

Git 中使用这种哈希值的情况很多，你将经常看到这种哈希值。 实际上，Git 数据库中保存的信息都是以文件内容的哈希值来索引，而不是文件名。

#### 文件状态

在 GIt 中，你的文件可能会处于三种状态之一：

- **已修改（modified）** - 已修改表示修改了文件，但还没保存到数据库中。
- **已暂存（staged）** - 已暂存表示对一个已修改文件的当前版本做了标记，使之包含在下次提交的快照中。
- **已提交（committed）** - 已提交表示数据已经安全的保存在本地数据库中。

#### 工作区域

与文件状态对应的，不同状态的文件在 Git 中处于不同的工作区域。

- **工作区（working）** - 当你 `git clone` 一个项目到本地，相当于在本地克隆了项目的一个副本。工作区是对项目的某个版本独立提取出来的内容。 这些从 Git 仓库的压缩数据库中提取出来的文件，放在磁盘上供你使用或修改。
- **暂存区（staging）** - 暂存区是一个文件，保存了下次将提交的文件列表信息，一般在 Git 仓库目录中。 有时候也被称作`‘索引’'，不过一般说法还是叫暂存区。
- **本地仓库（local）** - 提交更新，找到暂存区域的文件，将快照永久性存储到 Git 本地仓库。
- **远程仓库（remote）** - 以上几个工作区都是在本地。为了让别人可以看到你的修改，你需要将你的更新推送到远程仓库。同理，如果你想同步别人的修改，你需要从远程仓库拉取更新。

![img](https://raw.githubusercontent.com/dunwu/images/master/cs/web/git/git-theory.png)

## 分支管理

### Git Flow

Git Flow 应该是目前流传最广的 Git 分支管理策略。Git Flow 围绕的核心点是版本发布（release），它适用于迭代版本较长的项目。

> ![img](https://raw.githubusercontent.com/dunwu/images/master/snap/20210419110634.png)

> 详细内容，可以参考这篇文章：[Git 在团队中的最佳实践--如何正确使用 Git Flow](http://www.cnblogs.com/cnblogsfans/p/5075073.html)

Git Flow 常用分支：

- **`master`** - 主线分支
- **`develop`** - 开发分支
- **`feature`** - 特性分支
- **`release`** - 发布分支
- **`hotfix`** - 问题修复分支

Git Flow 工作流程

#### 2.1. 主干分支

![img](https://raw.githubusercontent.com/dunwu/images/master/snap/20210419113532.png)

主干分支有两个，它们是伴随着项目生命周期长期存在的分支。

- `master` - 这个分支对应发布到生产环境的代码。这个分支**只允许从其他分支合入代码**，不能在这个分支直接修改。所有在 master 分支上的 Commit 都应该打 Tag。
- `develop` - 这个分支包含所有要发布到下一个 release 的代码，这个分支**主要是从其他分支合入代码**，比如 feature 分支。

#### 2.2. `feature` 分支

这个分支主要是用来开发一个新的功能，一旦开发完成，我们合并回 develop 分支进入下一个 release。feature 分支开发结束后，必须合并回 develop 分支, 合并完分支后一般会删点这个 feature 分支，但是我们也可以保留。

![img](https://raw.githubusercontent.com/dunwu/images/master/snap/20210419114042.png)

#### 2.3. **`release` 分支**

release 分支基于 develop 分支创建，创建后，我们可以在这个 release 分支上进行测试，修复 Bug 等工作。同时，其它开发人员可以基于它开发新的 feature (记住：一旦创建了 release 分支之后不要从 develop 分支上合并新的改动到 release 分支)。

发布 release 分支时，合并 release 到 master 和 develop， 同时在 master 分支上打个 Tag 记住 release 版本号，然后可以删除 release 分支了。

#### 2.4. `hotfix` 分支

当出现线上 bug 时，也意味着 master 存在 Bug。这时，我们需要基于 master 创建一个 hotfix 分支，在此分支上完成 bug 修复。修复后，我们应该将此分支合并回 master 和 develop 分支，同时在 master 上打一个 tag。所以，hotfix 的改动会进入下一个 release。

![img](https://nvie.com/img/hotfix-branches@2x.png)

#### 2.5. 如何应用 Git Flow

在实际开发中，如何具体落地 Git Flow 流程呢？

git 提供了 `git flow` 命令来手动管理，但是比较麻烦，所以还是建议使用 Git Flow 的 GUI 工具。比如：[SourceTree](https://www.sourcetreeapp.com/)、[VScode 的 GitFlow 插件](https://marketplace.visualstudio.com/items?itemName=vs-publisher-57624.GitFlowforVisualStudio)、Intellij 的 GitFlow 插件等。

> 想了解更详细的 Git Flow 介绍，可以参考：
>
> [A Successful Git Branching Model](http://nvie.com/posts/a-successful-git-branching-model/)
>
> [Git 在团队中的最佳实践--如何正确使用 Git Flow](http://www.cnblogs.com/cnblogsfans/p/5075073.html)

### Github Flow

对于简单且迭代频繁的项目来说，Git Flow 可能有些太复杂了。这时，可以考虑 Github Flow。

在 Github Flow 策略中，所有分支都是基于 master 创建。在 Feature 或 Bugfix 分支中完成工作后，将其合入 master，然后继续迭代。

![img](https://raw.githubusercontent.com/dunwu/images/master/snap/20210420194518.png)

> 想了解更详细的 Github Flow 介绍，可以参考：[GitHub Flow](http://scottchacon.com/2011/08/31/github-flow.html)

## Git Commit 规范

Git 每次提交代码，都要写 Commit message（提交说明），否则就不允许提交。

好的 Commit message 可以让人一眼就明白提交者修改了什么内容，有什么影响；而不好的 Commit message 写了和没写一样，甚至还可能误导别人。

先来看下图中不好的 Commit message 范例，从提交信息完全看不出来修改了什么。

![img](https://raw.githubusercontent.com/dunwu/images/master/snap/20210420152215.png)

再来一张较好的 Commit message 范例，每次提交的是什么内容，做了什么一目了然。

![img](https://raw.githubusercontent.com/dunwu/images/master/snap/20210420151352.png)

### Commit message 的作用

从前面，我们不难看出，完善的 Commit message 非常有利于项目维护。即时是个人维护的项目，时间久了，可能也会忘记当初自己改了什么。

Commit message 的作用还不仅仅是理解历史信息，它的主要作用如下：

（1）提供易于理解的历史信息，方便检索

（2）可以过滤某些 commit（比如文档改动），便于快速查找信息。

（3）可以直接从 commit 生成 Change log。

### Commit message 的规范

开源社区有很多 Commit message 的规范，个人推荐使用 [Angular Git Commit 规范](https://docs.google.com/document/d/1QrDFcIiPjSLDn3EL15IJygNPiHORgU1_OOAqWjiDU5Y/edit#heading=h.greljkmo14y0)，这是目前使用最广的写法，比较合理和系统化，并且有配套的工具。

它主要有以下组成部分：

- 标题行：必填, 描述主要修改类型和内容
- 主题内容：描述为什么修改, 做了什么样的修改, 以及开发的思路等等
- 页脚注释：放 Breaking Changes 或 Closed Issues

常用的修改项

- `type`：commit 的类型
- `feat`：新特性
- `fix`：修改问题
- `refactor`：代码重构
- `docs`：文档修改
- `style`：代码格式修改, 注意不是 css 修改
- `test`：测试用例修改
- `chore`：其他修改, 比如构建流程, 依赖管理.
- `scope`：commit 影响的范围, 比如：route, component, utils, build...
- `subject`：commit 的概述
- `body`：commit 具体修改内容, 可以分为多行
- `footer`：一些备注, 通常是 BREAKING CHANGE 或修复的 bug 的链接

### Git Commit Template

Intellij 中有集成 [Angular Git Commit 规范](https://docs.google.com/document/d/1QrDFcIiPjSLDn3EL15IJygNPiHORgU1_OOAqWjiDU5Y/edit#heading=h.greljkmo14y0) 的插件，可以帮助我们快速创建符合 [Angular Git Commit 规范](https://docs.google.com/document/d/1QrDFcIiPjSLDn3EL15IJygNPiHORgU1_OOAqWjiDU5Y/edit#heading=h.greljkmo14y0) 的 Git Commit Message。

其使用步骤如下：

第一步，安装插件

![img](https://raw.githubusercontent.com/dunwu/images/master/snap/20210419145223.png)

第二步，提交代码时，按照模板填写 commit message

![img](https://raw.githubusercontent.com/dunwu/images/master/snap/20210419145327.png)

### 生成 Change log

如果你的所有 Commit 都符合 [Angular Git Commit 规范](https://docs.google.com/document/d/1QrDFcIiPjSLDn3EL15IJygNPiHORgU1_OOAqWjiDU5Y/edit#heading=h.greljkmo14y0)，那么发布新版本时，就可以用脚本自动生成 Change log。

生成的文档包括以下三个部分。

- New features
- Bug fixes
- Breaking changes.

每个部分都会罗列相关的 commit ，并且有指向这些 commit 的链接。当然，生成的文档允许手动修改，所以发布前，你还可以添加其他内容。

[conventional-changelog](https://github.com/ajoslin/conventional-changelog) 就是生成 Change log 的工具，运行下面的命令即可。

```bash
$ npm install -g conventional-changelog
$ cd my-project
$ conventional-changelog -p angular -i CHANGELOG.md -w
```

上面命令不会覆盖以前的 Change log，只会在`CHANGELOG.md`的头部加上自从上次发布以来的变动。

如果你想生成所有发布的 Change log，要改为运行下面的命令。

```bash
$ conventional-changelog -p angular -i CHANGELOG.md -w -r 0
```

为了方便使用，可以将其写入`package.json`的`scripts`字段。

```javascript
{
  "scripts": {
    "changelog": "conventional-changelog -p angular -i CHANGELOG.md -w -r 0"
  }
}
```

以后，直接运行下面的命令即可。

```bash
$ npm run changelog
```

## Git 奇技淫巧

### 生成 SSH 公钥

许多 Git 服务器都使用 SSH 公钥进行认证。 为了向 Git 服务器提供 SSH 公钥，如果某系统用户尚未拥有密钥，必须事先为其生成一份。 这个过程在所有操作系统上都是相似的。 首先，你需要确认自己是否已经拥有密钥。 默认情况下，用户的 SSH 密钥存储在其 `~/.ssh` 目录下。 进入该目录并列出其中内容，你便可以快速确认自己是否已拥有密钥：

```shell
$ cd ~/.ssh
$ ls
authorized_keys2  id_dsa       known_hosts
config            id_dsa.pub
```

我们需要寻找一对以 `id_dsa` 或 `id_rsa` 命名的文件，其中一个带有 `.pub` 扩展名。 `.pub` 文件是你的公钥，另一个则是私钥。 如果找不到这样的文件（或者根本没有 `.ssh` 目录），你可以通过运行 `ssh-keygen` 程序来创建它们。在 Linux/Mac 系统中，`ssh-keygen` 随 SSH 软件包提供；在 Windows 上，该程序包含于 MSysGit 软件包中。

```shell
$ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/schacon/.ssh/id_rsa):
Created directory '/home/schacon/.ssh'.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/schacon/.ssh/id_rsa.
Your public key has been saved in /home/schacon/.ssh/id_rsa.pub.
The key fingerprint is:
d0:82:24:8e:d7:f1:bb:9b:33:53:96:93:49:da:9b:e3 schacon@mylaptop.local
```

首先 `ssh-keygen` 会确认密钥的存储位置（默认是 `.ssh/id_rsa`），然后它会要求你输入两次密钥口令。如果你不想在使用密钥时输入口令，将其留空即可。

现在，进行了上述操作的用户需要将各自的公钥发送给任意一个 Git 服务器管理员（假设服务器正在使用基于公钥的 SSH 验证设置）。 他们所要做的就是复制各自的 `.pub` 文件内容，并将其通过邮件发送。 公钥看起来是这样的：

```shell
$ cat ~/.ssh/id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEAklOUpkDHrfHY17SbrmTIpNLTGK9Tjom/BWDSU
GPl+nafzlHDTYW7hdI4yZ5ew18JH4JW9jbhUFrviQzM7xlELEVf4h9lFX5QVkbPppSwg0cda3
Pbv7kOdJ/MTyBlWXFCR+HAo3FXRitBqxiX1nKhXpHAZsMciLq8V6RjsNAQwdsdMFvSlVK/7XA
t3FaoJoAsncM1Q9x5+3V0Ww68/eIFmb1zuUFljQJKprrX88XypNDvjYNby6vw/Pb0rwert/En
mZ+AW4OZPnTPI89ZPmVMLuayrD2cE86Z/il8b+gw3r3+1nKatmIkjn2so1d01QraTlMqVSsbx
NrRFi9wrf+M7Q== schacon@mylaptop.local
```

在你的 Github 账户中，依次点击 **Settings** > **SSH and GPG keys** > **New SSH key**

然后，将上面生成的公钥内容粘贴到 `Key` 编辑框并保存。至此大功告成。

后面，你在克隆你的 Github 项目时使用 SSH 方式即可。

如果觉得我的讲解还不够细致，可以参考：[adding-a-new-ssh-key-to-your-github-account](https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/)

### 使用 .gitignore 忽略不必提交内容

`.gitignore` 文件可能从字面含义也不难猜出：这个文件里配置的文件或目录，会自动被 git 所忽略，不纳入版本控制。

在日常开发中，我们的项目经常会产生一些临时文件，如编译 Java 产生的 `*.class` 文件，又或是 IDE 自动生成的隐藏目录（Intellij 的 `.idea` 目录、Eclipse 的 `.settings` 目录等）等等。这些文件或目录实在没必要纳入版本管理。在这种场景下，你就需要用到 `.gitignore` 配置来过滤这些文件或目录。

`.gitignore` 配置的规则很简单，也没什么可说的，看几个例子，自然就明白了。

【示例】一份 Java 的 .gitignore

```
# Compiled class file
*.class

# Log file
*.log

# BlueJ files
*.ctxt

# Mobile Tools for Java (J2ME)
.mtj.tmp/

# Package Files #
*.jar
*.war
*.nar
*.ear
*.zip
*.tar.gz
*.rar

# virtual machine crash logs, see http://www.java.com/en/download/help/error_hotspot.xml
hs_err_pid*
```

> 【推荐】这里推荐一个 Github 的开源项目：[gitignore](https://github.com/github/gitignore)，在这里，你可以找到很多常用的 .gitignore 模板，如：Java、Nodejs、C++ 的 `.gitignore` 模板等等。

### 使用 .gitattributes 解决 LF 和 CRLF 问题

你有没有在和多人协同开发时遇到过以下烦恼？

开发者们分别使用不同的操作系统进行开发，有的人用 Windows，有的人用 Linux/MacOS。众所周知，不同操作系统默认的文件结尾行是不同的：在 Windows 上默认的是回车换行（Carriage Return Line Feed, CRLF），然而，在 Linux/MacOS 上则是换行（Line Feed, LF）。这就可能导致这种情况：明明文件内容一模一样，但是版本比对时仍然存在版本差异。

那么如何解决这个问题呢？Git 提供了 `.gitattributes` 配置文件，它允许使用者指定由 git 使用的文件和路径的属性。

在 Git 库中，一个普通文本文件的行尾默认是 `LF`。对于工作目录，除了 `text` 属性之外，还可以设置 `eol` 属性或 `core.eol` 配置变量。

`.gitattributes` 文件中，可以用 text 属性指定某类文件或目录下的文件，控制它的行结束标准化。当一个文本文件被标准化时，它的行尾将在存储库中转换为 `LF`。要控制工作目录中使用的行结束风格，请使用单个文件的`eol`属性和所有文本文件的 `core.eol` 配置变量。

【示例】一份 .gitattributes 示例

```
* text=auto eol=lf

*.txt text
*.java text
*.scala text
*.groovy text
*.gradle text
*.properties text

# unix style
*.sh text eol=lf

# win style
*.bat text eol=crlf

# binary
*.jar binary
*.war binary
*.zip binary
*.tar binary
*.tar.gz binary
*.gz binary
*.apk binary
*.bin binary
*.exe binary
```

> 【推荐】这里推荐一个 Github 的开源项目：[gitignore](https://github.com/alexkaratarakis/gitattributes)，在这里，你可以找到很多常用的 .gitignore 模板，如：Java、Nodejs、C++ 的 `.gitignore` 模板等等。

### 同时提交代码到不同的远程仓库

如果，你在不同的 Git 远程仓库中维护同一个项目，你可能会有这样的需求：能不能一次提交，同时 push 到多个远程仓库中呢？

这个可以有，解决方案如下：

比如，我有一个 blog 项目，同时维护在 Github 和 Gitee 上。

（1）首先，在 Github 和 Gitee 上配置本地的 ssh 公钥（如果是 Gitlab，也同样如此），这样中央仓库就能识别本地。

生成 SSH 公钥的方法，请参考上文的 “生成 SSH 公钥” 章节。

（2）进入 git 项目的隐藏目录 `.git`，打开 config 文件，参考下面配置进行编辑：

```ini
[core]
	repositoryformatversion = 0
	filemode = false
	bare = false
	logallrefupdates = true
	symlinks = false
	ignorecase = true
[remote "origin"]
	url = git@github.com:dunwu/blog.git
	url = git@gitee.com:turnon/blog.git
	fetch = +refs/heads/*:refs/remotes/origin/*
[branch "master"]
	remote = origin
	merge = refs/heads/master
[user]
	name = dunwu
	email = forbreak@163.com
```

重点在于 `remote "origin"`，同时配置了两个 url。配置后，一旦触发 push 远程仓库的动作，就会同时推送提交记录到配置的远程仓库。

### Github Issue 和 Gitlab Issue

开发软件，Bug 再所难免，出现问题不可怕，可怕的是放任不管；所以，优秀的软件项目，都应该管理好问题追踪。软件的使用者，在使用中，可能会遇到形形色色的问题，难以解决，需要向维护者寻求帮助。

问题追踪如此重要，所以各种代码托管平台都会提供 Issue 维护机制，如 Github Issue 和 Gitlab Issue。

如果一个项目的开源社区很活跃，在没有任何约束的前提下，提问肯定是五花八门的，让维护者难以招架。

其实，提问也是一门艺术，如果提问者的问题长篇大幅，言不达意，让别人难以理解，就很难得到有效帮助。关于如何高效的提问，推荐参考 [提问的智慧](https://github.com/ryanhanwu/How-To-Ask-Questions-The-Smart-Way) 这篇文章，作者整理的非常好。

作为开发者，你不能期望所有提问者都是训练有素的提问者。所以，使用规范化的 Issue 模板来引导提问者提问，可以大大减轻开发者的负担。

#### Github Issue 模板

如何在 Github Issue 平台上创建 Issue 模板呢？方法如下：

（1）在仓库根目录创建新目录 `.github`

（2）在 `.github` 目录中添加 `ISSUE_TEMPLATE` 目录，在其中添加的 md 文件都会被 Github 自动识，并将其作为 issue 的默认模板。

示例，下面是携程 [apollo](https://github.com/ctripcorp/apollo) 的一个 Issue 模板，要求提问者填充 bug 描述、复现步骤、期望、截图、日志等细节。

![img](https://raw.githubusercontent.com/dunwu/images/master/snap/20210420114644.png)

> 更多模板：[Github issue_templates 模板](https://github.com/stevemao/github-issue-templates)

#### Gitlab Issue 模板

如何在 Gitlab Issue 平台上创建 Issue 模板呢？方法如下：

（1）在仓库根目录创建新目录 `.gitlab`

（2）在 `.gitlab` 目录中添加 `issue_templates` 目录，在其中添加的 md 文件都会被 Gitlab 自动识，并将其作为 issue 的默认模板。

![img](https://raw.githubusercontent.com/dunwu/images/master/snap/20210420141838.png)

> 更多模板：[Gitlab 官方 issue_templates 模板](https://gitlab.com/gitlab-org/gitlab/-/tree/master/.gitlab/issue_templates)

### Git Hook

在执行提交代码（git commit），推送代码（git push）等行为时，我们可能希望做一些代码检查性工作，例如：代码 lint 检查、代码格式化等。当检查发现代码存在问题时，就拒绝代码提交，从而保证项目质量。

Git 提供了 Git Hook 机制，允许使用者在特定的重要动作发生时触发自定义脚本。有两类钩子：客户端钩子和服务器端钩子。客户端钩子由诸如提交和合并等操作所触发调用，而服务器端钩子作用于诸如接收被推送的提交这样的联网操作。钩子都被存储在 Git 项目目录下的 `.git/hooks` 子目录中。Git 在这个目录下放置了一些示例，这些示例的名字都是以 `.sample` 结尾，如果想启用它们，得先移除这个后缀。

常用的客户端钩子：

- `pre-commit` 钩子：在提交信息前运行。 它用于检查即将提交的快照，例如，检查是否有所遗漏，确保测试运行，以及核查代码。 如果该钩子以非零值退出，Git 将放弃此次提交，不过你可以用 `git commit --no-verify` 来绕过这个环节。 你可以利用该钩子，来检查代码风格是否一致（运行类似 `lint` 的程序）、尾随空白字符是否存在（自带的钩子就是这么做的），或新方法的文档是否适当。
- `prepare-commit-msg` 钩子：在启动提交信息编辑器之前，默认信息被创建之后运行。 它允许你编辑提交者所看到的默认信息。 该钩子接收一些选项：存有当前提交信息的文件的路径、提交类型和修补提交的提交的 SHA-1 校验。 它对一般的提交来说并没有什么用；然而对那些会自动产生默认信息的提交，如提交信息模板、合并提交、压缩提交和修订提交等非常实用。 你可以结合提交模板来使用它，动态地插入信息。
- `commit-msg` 钩子：接收一个参数，此参数即上文提到的，存有当前提交信息的临时文件的路径。 如果该钩子脚本以非零值退出，Git 将放弃提交，因此，可以用来在提交通过前验证项目状态或提交信息。 在本章的最后一节，我们将展示如何使用该钩子来核对提交信息是否遵循指定的模板。
- `post-commit` 钩子：在整个提交过程完成后运行。它不接收任何参数，但你可以很容易地通过运行 `git log -1 HEAD` 来获得最后一次的提交信息。 该钩子一般用于通知之类的事情。
- `pre-push` 钩子：会在 `git push` 运行期间， 更新了远程引用但尚未传送对象时被调用。 它接受远程分支的名字和位置作为参数，同时从标准输入中读取一系列待更新的引用。 你可以在推送开始之前，用它验证对引用的更新操作（一个非零的退出码将终止推送过程）。

#### Javascript 应用 Git Hook

想在 JavaScript 应用中使用 Git Hook，推荐使用 [husky](https://github.com/typicode/husky) ，可以很方便的编写钩子处理命令。

使用方法很简单，先安装 husky

```
npm i -D husky
```

然后，在 package.json 中添加配置：

```json
"husky": {
  "hooks": {
    "pre-commit": "lint-staged"
  }
},
"lint-staged": {
  "src/**/*.{js,vue}": [
    "eslint --fix",
    "git add"
  ]
},
```

以上配置的作用是，当提交代码前（ `pre-commit` ），先执行 `lint-staged`；

`lint-staged` 中执行的动作是，对 src 目录的所有 js、vue 文件进行 eslint 检查，并尝试修复。如果修复后没有问题，就 git add 添加修改后的文件；如果修复失败，则拒绝提交代码。

## 参考资料

- **官方资源**
  - [Git 官网](https://git-scm.com/)
  - [Git Github](https://github.com/git/git)
  - [Github 官方教程](https://guides.github.com/)
- **模板**
  - [gitignore 模板](https://github.com/github/gitignore) - .gitignore 文件模板
  - [gitattributes 模板](https://github.com/alexkaratarakis/gitattributes) - .gitattributes 文件模板
  - [github-cheat-sheet](https://github.com/tiimgreen/github-cheat-sheet) - git 命令简略图表
- **Git 教程**
  - [Learn Git branching](https://learngitbranching.js.org/) - 交互式教程
  - [Git 官方推荐教程](https://git-scm.com/book/zh/v2) - Scott Chacon 的 Git 书。
    - [git-flight-rules](https://github.com/k88hudson/git-flight-rules)
  - [git-tips](https://github.com/521xueweihan/git-tips)
  - [Git 中文教程](https://github.com/geeeeeeeeek/git-recipes)
  - [廖雪峰的 Git 教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)
  - [有关 git 的学习资源](https://github.com/xirong/my-git)
- **文章**
  - [Git Cookbook](https://github.com/k88hudson/git-flight-rules/blob/master/README_zh-CN.md)
  - [Git 奇技淫巧](https://github.com/521xueweihan/git-tips)
  - [Git 风格指南](https://github.com/aseaday/git-style-guide)
  - [Git 在团队中的最佳实践--如何正确使用 Git Flow](http://www.cnblogs.com/cnblogsfans/p/5075073.html)
  - [Commit message 和 Change log 编写指南](http://www.ruanyifeng.com/blog/2016/01/commit_message_change_log.html)
- **Git 工具**
  - [guis](https://git-scm.com/downloads/guis) - Git 官网展示的客户端工具列表。
  - [gogs](https://github.com/gogits/gogs) - 极易搭建的自助 Git 服务。
  - [gitflow](https://github.com/nvie/gitflow) - 应用 [fit-flow](http://nvie.com/posts/a-successful-git-branching-model/) 模型的工具。
  - [firstaidgit.io](http://firstaidgit.io/) 一个可搜索的最常被问到的 Git 的问题
  - [git-extra-commands](https://github.com/unixorn/git-extra-commands) - 一堆有用的额外的 Git 脚本
  - [git-extras](https://github.com/tj/git-extras) - GIT 工具集 -- repo summary, repl, changelog population, author commit percentages and more
  - [git-fire](https://github.com/qw3rtman/git-fire) - git-fire 是一个 Git 插件，用于帮助在紧急情况下添加所有当前文件, 做提交(committing), 和推(push)到一个新分支(阻止合并冲突)。
  - [git-tips](https://github.com/git-tips/tips) - Git 小提示
  - [git-town](https://github.com/Originate/git-town) - 通用，高级 Git 工作流支持！
- **GUI 客户端**
  - [GitKraken](https://www.gitkraken.com/) - 豪华的 Git 客户端 Windows, Mac & Linux
  - [git-cola](https://git-cola.github.io/) - 另外一个 Git 客户端 Windows & OS X
  - [GitUp](https://github.com/git-up/GitUp) - 一个新的 Git 客户端，在处理 Git 的复杂性上有自己的特点
  - [gitx-dev](https://rowanj.github.io/gitx/) - 图形化的 Git 客户端 OS X
  - [Source Tree](https://www.sourcetreeapp.com/) - 免费的图形化 Git 客户端 Windows & OS X
  - [Tower](http://www.git-tower.com/) - 图形化 Git 客户端 OS X(付费)
- **git cheat sheet**
  - [github-git-cheat-sheet](https://services.github.com/on-demand/downloads/github-git-cheat-sheet.pdf)