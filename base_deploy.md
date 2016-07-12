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

#### 非 Git 用户

跑到 CNPM 的 Release 页面，选择相应的版本下载，比如这里会选择 [v2.12.2](https://github.com/cnpm/cnpmjs.org/releases/tag/2.12.2) 版。

下载完毕后将文件夹解压到相应目录即可。

### 修改配置文件

新建一份 `config/config.js` 文件，并且写入如下的骨架：

```js
'use strict';

module.exports = {
};
```

在这里面输入你需要的键值对。

这里将会列举一些常用的配置项，其余的一些配置项请自行参考 [config/index.js](https://github.com/cnpm/cnpmjs.org/blob/2.12.2/config/index.js) 文件。

#### 配置字段参考

+ `enableCluster`：是否启用 **cluster-worker** 模式启动服务，默认 `false`，生产环节推荐为 `true`;
+ `registryPort`：API 专用的 registry 服务端口，默认 `7001`；
+ `webPort`：Web 服务端口，默认 `7002`；
+ `bindingHost`：监听绑定的 Host，默认为 `127.0.0.1`，如果外面架了一层本地的 **Nginx** 反向代理或者 **Apache** 反向代理的话推荐不用改；
+ `sessionSecret`：**session** 用的盐；
+ `logdir`：日志目录；
+ `uploadDir`：临时上传文件目录；
+ `viewCache`：视图模板缓存是否开启，默认为 `false`；
+ `enableCompress`：是否开启 **gzip** 压缩，默认为 `false`；
+ `admins`：管理员们，这是一个 `JSON Object`，对应各键名为各管理员的用户名，键值为其邮箱，默认为 `{ fengmk2: 'fengmk2@gmail.com', admin: 'admin@cnpmjs.org', dead_horse: 'dead_horse@qq.com' }`；
+ `logoURL`：**Logo** 地址，不过对于我这个已经把 CNPM 前端改得面目全非的人来说已经忽略了这个配置了；
+ `adBanner`：广告 Banner 的地址；
+ `customReadmeFile`：实际上我们看到的 cnpmjs.org