# 🚀 MIXUP

这里是我的 K3s 集群“大本营”。

### 为什么叫 MIXUP？
1. **告别凌乱**：以前用 Docker 总是乱糟糟的，现在把各种工具全都 **Mix** 进 K3s，统一管理。
2. **蜜雪冰城**：单纯是因为喜欢喝蜜雪冰城（Mixue），所以取了这个名字。🍦

本项目采用 GitOps 的思路来折腾我的基础设施和各种好玩的应用。

## 仓库结构

```text
.
├── app/                      # 业务应用清单（Deployment/DaemonSet/Kustomization）
│   ├── baozap/
│   ├── bitwarden/
│   ├── cloudflared/
│   ├── git-system/
│   ├── komari/
│   ├── nft/
│   ├── nginx/
│   ├── nodepass/
│   ├── realm/
│   ├── singbox/
│   └── smartdns/
├── infra/                    # 基础设施
│   ├── flux-system/          # Flux 引导与 Git 源配置
│   ├── kube-system/          # 系统组件定制（如 Traefik）
│   ├── monitoring/           # Prometheus / Grafana / node-exporter
│   ├── reloader/             # 变更自动重载
│   └── rustfs/               # RustFS 相关资源
├── common/                   # 公共组件
│   └── cronjob/              # 通用定时任务
└── doc/                      # 运维备忘录
```

## 说明

- `app/`：各应用资源与对应的 Flux `Kustomization`。
- `infra/`：集群底座组件与运维基础能力。
- `common/`：可复用的公共任务或配置。
- `doc/`：常用命令与操作记录。

### 看看状态
强烈推荐用 `k9s`。具体操作指南：[doc/k9s.md](doc/k9s.md)。

## 运维小贴士
网络测试、容器调试之类的命令我都记在 [doc/ops.md](doc/ops.md) 里了，省得每次都要去搜。
