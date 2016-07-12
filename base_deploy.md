# 基础部署

本章会介绍 CNPM 的基础部署方法。

> 该文章所对应的 cnpm 目标版本为 [v2.12.2](https://github.com/cnpm/cnpmjs.org/tree/2.12.2)，上下浮动一些兼容的版本问题也都不是特别大。

## 准备

想要部署 CNPM，你需要做以下的一些准备。

1. 部署的宿体，如服务器、云主机、自己的电脑等；
2. 数据库，支持 MySQL、PostgreSQL、MariaDB，如果使用 SQLite 则无需准备；
3. Git 客户端（推荐）。

## 开始部署

### 克隆 CNPM

首先在本地选择一个目录，比如我将它选择在 `/usr/app`，然后预想 CNPM 的目录为 `/usr/app/cnpm`，那么需要在终端 `$ cd /usr/app`。

接下去执行 Git 指令将 CNPM 克隆到相应目录。

```sh
$ git clone https://github.com/cnpm/cnpmjs.org.git
```

#### Windows 用户

Windows 用户也可以用类似 [Cygwin](https://www.cygwin.com/)、[MinGW](http://www.mingw.org/)、[Powershell](https://msdn.microsoft.com/en-us/powershell) 甚至直接是 Command 等来运行 [Git](https://git-scm.com/download/win)。

当然也可以直接下载一些 GUI 工具来克隆，如 [SourceTree](https://www.sourcetreeapp.com/)。