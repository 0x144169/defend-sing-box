# 介绍

最好用的 sing-box 一键安装脚本 & 管理脚本

# 安装

**方式一：一键安装（服务器需能访问 GitHub）**

```bash
bash <(wget -qO- -o- https://github.com/0x144169/defend-sing-box/raw/main/install.sh)
```

**方式二：仅上传、本地安装（如 luns.host 等不能随意装软件的环境）**

1. 在本地把整个项目打成 zip 或 tar.gz（必须包含 `install.sh`、`sing-box.sh`、`src/` 目录）。
2. 上传到服务器（如 `/root/` 或 `/tmp/`）。
3. 解压并进入目录后，用 **root** 执行：

```bash
# 若用 zip（需有 unzip）
unzip defend-sing-box-main.zip
cd defend-sing-box-main

# 若用 tar.gz（一般都有 tar）
tar xzf defend-sing-box-main.tar.gz
cd defend-sing-box-main

# 使用当前目录的脚本进行安装（不再从 GitHub 拉脚本）
bash install.sh -l
```

- 安装过程仍会从 GitHub 下载 sing-box 核心和 jq（需服务器能联网）。若**完全不能联网**，需在本地先下载 [sing-box 对应架构的 linux 包](https://github.com/SagerNet/sing-box/releases) 和 [jq](https://github.com/jqlang/jq/releases)，上传后执行：`bash install.sh -l -f /path/to/sing-box-xxx-linux-amd64.tar.gz`，并确保系统里已有可用的 `jq`（或放到 PATH）。

# 特点

- 快速安装
- 无敌好用
- 零学习成本
- 自动化 TLS
- 简化所有流程
- 兼容 sing-box 命令
- 强大的快捷参数
- 支持所有常用协议
- 一键添加 VLESS-REALITY (默认)
- 一键添加 TUIC
- 一键添加 Trojan
- 一键添加 Hysteria2
- 一键添加 Shadowsocks 2022
- 一键添加 VMess-(TCP/HTTP/QUIC)
- 一键添加 VMess-(WS/H2/HTTPUpgrade)-TLS
- 一键添加 VLESS-(WS/H2/HTTPUpgrade)-TLS
- 一键添加 Trojan-(WS/H2/HTTPUpgrade)-TLS
- 一键启用 BBR
- 一键更改伪装网站
- 一键更改 (端口/UUID/密码/域名/路径/加密方式/SNI/等...)
- **可配置网站屏蔽**：通过域名列表限制访问指定网站（如未成年人保护场景）
- 还有更多...

# 屏蔽指定网站（限制访问）

本脚本支持通过**域名列表**屏蔽指定网站，适用于需要限制访问敏感内容的场景（如未成年人保护）。

- **配置文件**：`/etc/sing-box/blocked_domains.txt`（首次生成 config 或执行 fix-config.json 时自动创建）
- **格式**：每行一个域名，支持以 `#` 开头的注释行。填写 `example.com` 会屏蔽该域名及其所有子域名。
- **生效**：编辑保存后执行 `sing-box fix-config.json`，然后服务会自动重启使配置生效。
- **查看配置**：执行 `sing-box block-list` 可查看配置文件路径与当前内容。

本功能为**可选扩展**，不修改一键安装流程；未配置或列表为空时行为与原有脚本完全一致。

### 访问日志（便于审查与更新 block-list）

- **日志路径**：`/var/log/sing-box/access.log`
- **实时查看**：`sing-box log`（等价于 `tail -f` 该文件）
- **调整级别**：`sing-box log [trace|debug|info|warn|error]`，默认 `info`；审查时可临时改为 `debug` 获取更细的访问记录。
- 定期查看该日志可发现访问行为，便于及时更新 `blocked_domains.txt` 后执行 `sing-box fix-config.json` 生效。

### 屏蔽时显示告警页（Caddy）

访问被屏蔽网站时，可改为返回一句告警页（如「好好学习，不要访问不该访问的网站」），而不是直接断连。由 **Caddy** 在本地提供该页面，sing-box 将屏蔽流量转向 Caddy。

**前提**：本机已安装 Caddy（通过本脚本添加任意 TLS 协议时会自动安装，或自行安装）。

**启动与配置：**

1. **开启告警页**
   ```bash
   sing-box block-warn on
   ```
   会创建 `/etc/sing-box/block_use_warn_page`、配置 Caddy 监听 `127.0.0.1:2080` 并重写 config.json，之后访问屏蔽域名会看到告警页。

2. **关闭告警页（恢复为直接拒绝连接）**
   ```bash
   sing-box block-warn off
   ```

3. **自定义告警文案**
   - 编辑 `/etc/sing-box/warn-page/warn.html`，按需修改 HTML 内容。
   - 无需重启；Caddy 每次请求都会读该文件。

4. **查看状态**
   ```bash
   sing-box block-warn
   ```
   可查看当前是「已开启」还是「已关闭」，以及告警页文件路径。

**说明**：告警页由 Caddy 在 `127.0.0.1:2080` 提供，仅本机可访问；对外流量经 sing-box 匹配到屏蔽域名后，会被转到该端口并返回告警页内容。

# 设计理念

设计理念为：**高效率，超快速，极易用**

脚本基于作者的自身使用需求，以 **多配置同时运行** 为核心设计

并且专门优化了，添加、更改、查看、删除、这四项常用功能

你只需要一条命令即可完成 添加、更改、查看、删除、等操作

例如，添加一个配置仅需不到 1 秒！瞬间完成添加！其他操作亦是如此！

脚本的参数非常高效率并且超级易用，请掌握参数的使用

# 文档

安装及使用：https://233boy.com/sing-box/sing-box-script/

# 帮助

使用：`sing-box help`

```
sing-box script v1.0 by 233boy
Usage: sing-box [options]... [args]...

基本:
   v, version                                      显示当前版本
   ip                                              返回当前主机的 IP
   pbk                                             同等于 sing-box generate reality-keypair
   get-port                                        返回一个可用的端口
   ss2022                                          返回一个可用于 Shadowsocks 2022 的密码

一般:
   a, add [protocol] [args... | auto]              添加配置
   c, change [name] [option] [args... | auto]      更改配置
   d, del [name]                                   删除配置**
   i, info [name]                                  查看配置
   qr [name]                                       二维码信息
   url [name]                                      URL 信息
   log                                             查看日志
更改:
   full [name] [...]                               更改多个参数
   id [name] [uuid | auto]                         更改 UUID
   host [name] [domain]                            更改域名
   port [name] [port | auto]                       更改端口
   path [name] [path | auto]                       更改路径
   passwd [name] [password | auto]                 更改密码
   key [name] [Private key | atuo] [Public key]    更改密钥
   method [name] [method | auto]                   更改加密方式
   sni [name] [ ip | domain]                       更改 serverName
   new [name] [...]                                更改协议
   web [name] [domain]                             更改伪装网站

进阶:
   dns [...]                                       设置 DNS
   dd, ddel [name...]                              删除多个配置**
   fix [name]                                      修复一个配置
   fix-all                                         修复全部配置
   fix-caddyfile                                   修复 Caddyfile
   fix-config.json                                 修复 config.json
   block-list                                      查看/说明屏蔽域名列表配置
   import                                          导入 sing-box/v2ray 脚本配置

管理:
   un, uninstall                                   卸载
   u, update [core | sh | caddy] [ver]             更新
   U, update.sh                                    更新脚本
   s, status                                       运行状态
   start, stop, restart [caddy]                    启动, 停止, 重启
   t, test                                         测试运行
   reinstall                                       重装脚本

测试:
   debug [name]                                    显示一些 debug 信息, 仅供参考
   gen [...]                                       同等于 add, 但只显示 JSON 内容, 不创建文件, 测试使用
   no-auto-tls [...]                               同等于 add, 但禁止自动配置 TLS, 可用于 *TLS 相关协议
其他:
   bbr                                             启用 BBR, 如果支持
   bin [...]                                       运行 sing-box 命令, 例如: sing-box bin help
   [...] [...]                                     兼容绝大多数的 sing-box 命令, 例如: sing-box generate uuid
   h, help                                         显示此帮助界面

谨慎使用 del, ddel, 此选项会直接删除配置; 无需确认
反馈问题) https://github.com/233boy/sing-box/issues
文档(doc) https://233boy.com/sing-box/sing-box-script/
```