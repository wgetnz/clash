# sing-box 分流规则迁移

## 完成内容

- 保留 Clash 规则源顺序和策略目标，新增 sing-box 专用远程配置。
- 将 AI `GEOSITE` 分类展开为 182 条普通域名规则。
- 使用完整 IPv4/IPv6 CIDR 集合替代已移除的 GEO 规则。
- 将 SSH 端口规则改为 sing-box `PORT` 语义。
- 删除无法原生等价的 fallback/load-balance 可选组，修复地区节点匹配。

## 修改文件

- `Full-singbox.ini`
- `ai-geosite-singbox.list`
- `ssh-singbox.list`
- `CLAUDE.md`

## 验证

- 原 Clash 9097 条可直接映射规则：零缺失。
- AI geosite 182 条展开规则：零缺失。
- 中国 IPv6 3411 条展开规则：零缺失。
- 官方 sing-box 1.14.1 `check`：通过。

## 上线（2026-09-22）

- main 已推送；`maxsub` v21 与 `subweb` 前端已部署，生产 URL 生成的配置通过官方 sing-box 1.14.1 `check`。
- `ipv6-cn-singbox.list` 的自动同步工作流会持续改动扁平规则数（本次 3411 → 3406，CIDR 聚合），对比时以当前 main 为准。

## 后续待办

- 上游 sing-geosite 分类变更时重新生成 `ai-geosite-singbox.list` 并复跑等价性检查。
