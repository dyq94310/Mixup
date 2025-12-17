# K3s 集群配置管理

本项目用于管理 K3s 集群的资源清单（Manifests），采用 GitOps 的思路对基础设施和业务应用进行版本控制。

## 目录结构

```bash
.
├── app/                    # 业务应用配置
│   ├── bitwarden/          # Vaultwarden 隧道配置
│   ├── komari/             # Komari 应用及加密密钥
│   ├── nginx/              # Nginx 站点配置 (groovydeng.eu.org)
│   ├── nodepass/           # Nodepass 节点管理 (dash, master)
│   ├── realm/              # Realm 端口转发配置
│   ├── singbox/            # Sing-box 网络工具
│   ├── uptime-kuma/        # 状态监控面板
│   └── whoami/             # HTTP 测试工具
├── doc/                    # 项目文档
│   ├── k9s.md              # k9s 工具使用指南
│   └── ops.md              # 运维手册与常用命令
├── infra/                  # 基础设施组件
│   ├── kube-system/        # 系统级组件配置 (如 Traefik Host)
│   └── sealed-secrets/     # Sealed Secrets 控制器及示例
├── .gitignore
└── README.md
```

## 核心组件说明

### 基础设施 (infra)
- **Traefik**: 作为集群的 Ingress Controller，处理外部流量进入。
- **Sealed Secrets**: 用于将敏感信息（如密码、Token）加密为可安全提交到 Git 的资源。

### 业务应用 (app)
- **监控**: 使用 `uptime-kuma` 进行服务可用性监控。
- **网络与转发**: 包含 `realm` 端口转发、`singbox` 以及 `bitwarden` 的隧道配置。
- **管理工具**: `nodepass` 相关组件用于节点或密码管理。

## 快速开始

### 部署应用
通常可以使用 kubectl 直接应用对应的 yaml 文件：
```bash
kubectl apply -f app/<app-name>/
```

### 查看集群状态
推荐使用 `k9s` 工具进行交互式管理，详细操作指南请参考 [doc/k9s.md](doc/k9s.md)。

## 运维指南
关于网络测试（netshoot）、容器调试（kubectl debug）等常用指令，请参阅 [doc/ops.md](doc/ops.md)。

## 安全规范
- **禁止** 直接提交明文 Secret 到仓库。
- 所有敏感配置必须通过 `kubeseal` 加密生成 `SealedSecret` 后方可提交。
- `.gitignore` 已配置过滤常见的敏感文件后缀（如 `.key`, `.crt`, `.pem`, `kubeconfig` 等），请务必遵守。
