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
+ `customReadmeFile`：实际上我们看到的 [cnpmjs.org](http://cnpmjs.org) 首页中间一大堆冗长的介绍是一个 Markdown 文件转化而成的，你可以设置该项来自行替换这个文件；
+ `customFooter`：自定义页脚模板；
+ `npmClientName`：默认为 `cnpm`，如果你有自己开发或者 fork 的 npm 客户端的话请改成自己的 CLI 命令，这个应该会在一些页面的说明处替换成你所写的；
+ `backupFilePrefix`：备份目录；
+ `database`：数据库相关配置，为一个对象，默认如果不配置将会是一个 `~/.cnpmjs.org/data.sqlite` 的 SQLite；
  - `db`：数据的库名；
  - `username`：数据库用户名；
  - `password`：数据库密码；
  - `dialect`：数据库适配器，可选 `"mysql"`、`"sqlite"`、`"postgres"`、`"mariadb"`，默认为 `"sqlite"`；
  - `hsot`：数据库地址；
  - `port`：数据库端口；
  - `pool`：数据库连接池相关配置，为一个对象；
    * `maxConnections`：最大连接数，默认为 `10`；
    * `minConnections`：最小连接数，默认为 `0`；
    * `maxIdleTime`：单条链接最大空闲时间，默认为 `30000` 毫秒；
  - `storege`：仅对 SQLite 配置有效，数据库地址，默认为 `~/.cnpmjs/data.sqlite`；
+ `nfs`：包文件系统处理对象，为一个 Node.js 对象，默认是 [fs-cnpm]() 这个包，并且配置在 `~/.cnpmjs/nfs` 目录下，也就是说默认所有同步的包都会被放在这个目录下；开发者可以使用别的一些文件系统插件（如上传到又拍云等）,又或者自己去按接口开发一个逻辑层，这些都是后话了；
+ `registryHost`：暂时还未试过，我猜是用于 Web 页面显示用的，默认为 `r.cnpmjs.org`；
+ `enablePrivate`：是否开启私有模式，默认为 `false`；
  - 如果是私有模式则只有管理员能发布包，其它人只能从源站同步包；
  - 如果是非私有模式则所有登录用户都能发布包；
+ `scopes`：非管理员发布包的时候只能用以 `scopes` 里面列举的命名空间为前缀来发布，如果没设置则无法发布，也就是说这是一个必填项，默认为 `[ '@cnpm', '@cnpmtest', '@cnpm-test' ]`，据苏千大大解释是为了便于管理以及让公司的员工自觉按需发布；更多关于 NPM scope 的说明请参见 [npm-scope](https://docs.npmjs.com/misc/scope)；
+ `privatePackages`：就如该配置项的注释所述，出于历史包袱的原因，有些已经存在的私有包（可能之前是用 Git 的方式安装的）并没有以命名空间的形式来命名，而这种包本来是无法上传到 CNPM 的，这个配置项数组就是用来加这些例外白名单的，默认为一个空数组；
+ `sourceNpmRegistry`：更新源 NPM 的 registry 地址，默认为 `https://registry.npm.taobao.org`；
+ `sourceNpmRegistryIsCNpm`：源 registry 是否为 CNPM，默认为 `true`，如果你使用的源是官方 NPM 源，请将其设为 `false`；
+ `syncByInstall`：如果安装包的时候发现包不存在，则尝试从更新源同步，默认为 `true`；
+ `syncModel`：更新模式（不过我觉得是个 `typo`），有下面几种模式可以选择，默认为 `"none"`;
  - `"none"`：永不同步，只管理私有用户上传的包，其它源包会直接从源站获取；
  - `"exist"`：定时同步已经存在于数据库的包；
  - `"all"`：定时同步所有源站的包；
+ `syncInterval`：同步间隔，默认为 `"10m"` 即十分钟；
+ `syncDevDependencies`：是否同步每个包里面的 `devDependencies` 包们，默认为 `false`；
+ `badgeSubject`：包的 **badge** 显示的名字，默认为 `cnpm`；
+ `userService`：用户验证接口，默认为 `null`，即无用户相关功能也就是无法有用户去上传包，该部分需要自己实现接口功能并配置，如与公司的 **Gitlab** 相对接，这也是后话了；
+ `alwaysAuth`：是否始终需要用户验证，即便是 `$ cnpm install` 等命令；
+ `httpProxy`：代理地址设置，用于你在墙内源站在墙外的情况。

#### 一个可能的配置

下面给出一个样例配置：

```js
module.exports = {
    enableCluster: true,
    database: {
        db: "snpm",
        username: "username",
        password: "password",

        dialect: "mysql",
        host: "127.0.0.1",
        port: 3306
    },
    enablePrivate: false,
    admins: {
        xadillax: "i@2333.moe"
    },
    syncModel: "exist",
    nfs: require('upyun-cnpm').create({
        bucket: "your bucket",
        oprator: "your id",
        password: "your secret"
    }),
    scopes: [ '@cheniu', '@souche', '@souche-f2e' ],
    badgeSubject: 'snpm',
    privatePackages: [ 'snpm' ]
};
```

> 上面的配置包文件系统层用的是 [upyun-cnpm](https://github.com/cnpm/upyun-cnpm) 插件，需要在 CNPM 源码根目录执行
> ```sh
> $ npm install --save -d upyun-cnpm
> ```
>
> 这个时候你的 `package.json` 就有更改与源 Repo 不一致了，如果是 Git 克隆的用户在以后升级更新系统的时候稍稍注意一下可能的冲突即可。

