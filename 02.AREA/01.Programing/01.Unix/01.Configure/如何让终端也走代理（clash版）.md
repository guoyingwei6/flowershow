---
created: 2024-07-29 17:05
updated: 2025-06-10 09:55
tags:
  - garden/🌳Evergreen
dg-publish: true
---


在`~/.zshrc`文件中添加下面两行命令：

```shell
# proxy
alias proxy='export all_proxy=socks5://127.0.0.1:7890'
alias unproxy='unset all_proxy'
# 通过 alias 定义命令 `ip`
alias ip="curl ipinfo.io"
```

在命令行中输入 proxy 并按回车后，执行 `export all_proxy=socks5://127.0.0.1:7890`。这条命令设置了一个名为 `all_proxy` 的环境变量，它的值是 `socks5://127.0.0.1:7890`，指向本地机器上的 SOCKS5 代理服务器，监听在 `7890` 端口。这意味着所有支持此环境变量的程序会通过这个代理服务器路由它们的网络请求。

这条命令设置了一个名为 `all_proxy` 的环境变量，它的值是 `socks5://127.0.0.1:7890`，指向本地机器上的 `SOCKS5` 代理服务器，监听在 `7890` 端口。这意味着所有支持此环境变量的程序会通过这个代理服务器路由它们的网络请求。