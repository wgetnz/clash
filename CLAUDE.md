# 项目接班说明

## 用户目的 / 项目背景

该仓库维护 `subweb.fly.dev` / `maxsub.fly.dev` 使用的远程分流规则。Clash 与 sing-box 使用独立入口，目标是在不改变规则优先级和策略目标的前提下生成可由当前 sing-box 内核加载的配置。

## 关键配置和路径

- `Full.ini`：Clash 远程配置，保持原有行为。
- `Full-singbox.ini`：sing-box 专用远程配置，只使用可等价表达的规则和策略组。
- `ai-geosite-singbox.list`：由官方 sing-geosite 规则集展开的 AI 域名规则。
- `ipv6-cn-singbox.list`：中国 IPv6 CIDR 展开规则，由现有工作流自动更新。
- `ssh-singbox.list`：sing-box 端口规则。
- 节点订阅和凭据不允许写入本仓库。

## 重要决策和踩坑

- sing-box 1.14 已移除 `GEOIP` / `GEOSITE` 旧字段，因此使用普通域名、IPv4 CIDR 和 IPv6 CIDR 展开规则保持语义。
- `fallback` / `load-balance` 没有原生等价出站，不在 sing-box 专用配置中伪装保留；主分流策略只使用 `selector` 与 `urltest`。
- 规则源顺序必须与 Clash 保持一致，避免前置规则优先级变化。
- 地区组正则必须兼容小写节点名和 Emoji 重命名结果。

## 常用命令

```powershell
git diff --check
git status --short --branch
```

生成后的 JSON 必须使用官方 sing-box 执行：

```powershell
sing-box check -c config.json
```

## 当前进展和待办

sing-box 规则入口已完成端口、AI geosite、IPv4/IPv6 GEO 展开和地区组适配。后续更新规则时必须继续执行 Clash 与 sing-box 的规则等价性对比。
