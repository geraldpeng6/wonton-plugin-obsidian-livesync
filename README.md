# 笔记同步（Obsidian LiveSync）

让家里几台电脑上的 [Obsidian](https://obsidian.md) 笔记实时同步：Obsidian 装上社区插件
[Self-hosted LiveSync](https://github.com/vrtmrz/obsidian-livesync)（MIT），通过局域网连到本插件提供的
[CouchDB](https://couchdb.apache.org)（Apache-2.0）数据库，笔记自动互相同步。数据保存在自己家里，不用订阅
Obsidian 官方的 Sync 服务，也没有第三方服务器经手。

## 怎么用

1. 装好本插件后，在插件详情里打开“在局域网开放”，记下显示的地址、账号和密码。
2. 在电脑版 Obsidian 里安装社区插件 **Self-hosted LiveSync**。
3. 在 LiveSync 的连接设置里填：服务器地址、账号、密码（都来自第 1 步），数据库名填 `obsidiandb`。
4. 按插件引导完成第一次同步，其他电脑重复第 2–3 步即可。

上游文档：<https://github.com/vrtmrz/obsidian-livesync/blob/main/docs/setup_own_server.md>

## 数据存在哪、备份要注意什么

- 全部笔记存在插件的数据目录里（`WONTON_APP_DATA/data`，容器内 `/opt/couchdb/data`），没有别的副本。
- 备份直接拷贝这个目录即可；拷贝前先在插件详情里停止插件（CouchDB 运行中直接拷贝可能得到不完整的文件）。
- LiveSync 本身支持端到端加密（在 Obsidian 的 LiveSync 设置里开“加密”），开了之后服务器上存的是密文。

## 已知限制

- **手机连不上**：上游文档明确要求移动端用 HTTPS 且证书要系统信任（“Obsidian Mobile requires HTTPS”，
  见 setup_own_server.md 与 troubleshooting.md；自签证书也不行，见上游 issue #569）。当前插件底座只提供
  局域网 HTTP 端口，没有局域网 HTTPS 入口，所以手机上的 Obsidian 暂时连不上，先在电脑之间用。
- 没有给人用的网页：CouchDB 的管理页（Fauxton）不对普通用户开放，插件也不声明网页入口。

## 权限

- `network.lan`：在局域网上开放 5984 端口，Obsidian 直连这里同步。
- `owner.identity`：客户端账号用你的用户名，密码由核心生成。

## 许可证和商标

- 插件包：MIT，见 [LICENSE](LICENSE)；素材声明见 [NOTICE](NOTICE)。
- 服务端 Apache CouchDB：Apache License 2.0（官方镜像 `couchdb`）。
- 客户端 Self-hosted LiveSync：MIT。Obsidian 是 Dynalist Inc. 的商标；本插件与上游项目无从属关系。
