# 屏蔽告警页：启动与配置指引（Caddy）

访问被屏蔽网站时，可返回一句告警页（如「好好学习，不要访问不该访问的网站」），由 **Caddy** 在本地提供该页面。

## 前提

- 本机已安装 **Caddy**（通过本脚本添加任意 TLS 协议时会自动安装，或自行安装）。
- 已配置屏蔽域名列表：`/etc/sing-box/blocked_domains.txt` 中至少有一个域名。

## 启动

```bash
# 开启告警页（屏蔽流量将转向 Caddy 并返回告警页）
sing-box block-warn on
```

执行后会：

- 创建并写入 `/etc/sing-box/block_use_warn_page`
- 在 Caddy 中增加 `127.0.0.1:2080` 的告警页站点配置
- 重写 sing-box 的 `config.json`，将屏蔽域名流量转向 `127.0.0.1:2080`
- 重启 Caddy 与 sing-box

## 关闭

```bash
# 关闭告警页（恢复为直接拒绝连接）
sing-box block-warn off
```

## 配置

- **告警页内容**：编辑 `/etc/sing-box/warn-page/warn.html`，按需修改 HTML 文案与样式，保存即可，无需重启。
- **查看状态**：执行 `sing-box block-warn` 可查看当前是「已开启」还是「已关闭」，以及告警页文件路径。

## 说明

- 告警页由 Caddy 在 **127.0.0.1:2080** 提供，仅本机可访问；对外流量经 sing-box 匹配到屏蔽域名后，会被转到该端口并返回告警页内容。
- 与「仅屏蔽、不告警」互斥：开启告警页后为「屏蔽 + 告警页」，关闭后为「屏蔽 + 直接拒绝连接」。
