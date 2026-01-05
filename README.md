# 🚀 MIXUP

这里是我的 K3s 集群“大本营”。

### 为什么叫 MIXUP？
1. **告别凌乱**：以前用 Docker 总是乱糟糟的，现在把各种工具全都 **Mix** 进 K3s，统一管理。
2. **蜜雪冰城**：单纯是因为喜欢喝蜜雪冰城（Mixue），所以取了这个名字。🍦

本项目采用 GitOps 的思路来折腾我的基础设施和各种好玩的应用。

## 目录里都有啥？

```bash
.
├── app/                    # 业务应用（我的各种玩具）
│   ├── bitwarden/          # 密码管理隧道
│   ├── komari/             # Komari 应用
│   ├── nginx/              # 个人站点 
│   ├── nodepass/           # 端口转发神器
│   ├── realm/              # 端口转发神器
│   ├── singbox/            # 端口转发神器
│   ├── uptime-kuma/        # 看看服务挂没挂
│   └── whoami/             # 简单的 HTTP 测试
│   └── git-system/         # git
├── doc/                     # 备忘录
│   ├── k9s.md              # k9s 怎么用
│   └── ops.md              # 常用运维命令（防忘）
├── infra/                   # 基础设施（集群的底座）
│   ├── kube-system/        # 系统组件 (Traefik 之类的)
│   └── flux-system/        # 流水线
│   └── rustfs/             # s3文服
├── common/                  # 公共设施
│   ├── cronjob/            # 定时任务
├── .gitignore
└── README.md
```

## 核心组件

### 基础设施 (infra)

### 业务应用 (app)

### 公共设施 (common)

### 看看状态
强烈推荐用 `k9s`。具体操作指南：[doc/k9s.md](doc/k9s.md)。

## 运维小贴士
网络测试、容器调试之类的命令我都记在 [doc/ops.md](doc/ops.md) 里了，省得每次都要去搜。
