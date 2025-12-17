# 🚀 我的 K3s 实验室

这里是我 K3s 集群的“大本营”。我把所有的 YAML 配置文件都丢在这儿，用 GitOps 的思路来折腾我的基础设施和各种好玩的应用。

## 目录里都有啥？

```bash
.
├── app/                    # 业务应用（我的各种玩具）
│   ├── bitwarden/          # 密码管理隧道
│   ├── komari/             # Komari 应用
│   ├── nginx/              # 个人站点 (groovydeng.eu.org)
│   ├── nodepass/           # 节点管理
│   ├── realm/              # 端口转发神器
│   ├── singbox/            # 网络工具
│   ├── uptime-kuma/        # 看看服务挂没挂
│   └── whoami/             # 简单的 HTTP 测试
├── doc/                    # 备忘录
│   ├── k9s.md              # k9s 怎么用
│   └── ops.md              # 常用运维命令（防忘）
├── infra/                  # 基础设施（集群的底座）
│   ├── kube-system/        # 系统组件 (Traefik 之类的)
│   └── sealed-secrets/     # 秘密加密工具
├── .gitignore
└── README.md
```

## 核心组件

### 基础设施 (infra)
- **Traefik**: 集群的大门，管流量的。
- **Sealed Secrets**: 专门用来给密码加密，这样我就能放心地把配置传到 GitHub 上了。

### 业务应用 (app)
- **监控**: 用 `uptime-kuma` 盯着我的服务，挂了能及时发现。
- **网络与转发**: `realm`、`singbox` 还有 `bitwarden` 隧道，搞定各种奇奇怪怪的网络需求。
- **管理工具**: `nodepass` 相关的东西，用于节点和密码管理。

## 怎么玩？

### 部署应用
直接用 kubectl 梭哈：
```bash
kubectl apply -f app/<应用名>/
```

### 看看状态
强烈推荐用 `k9s`，用过都说好。具体怎么操作看这里：[doc/k9s.md](doc/k9s.md)。

## 运维小贴士
网络测试、容器调试之类的命令我都记在 [doc/ops.md](doc/ops.md) 里了，省得每次都要去搜。

## 安全守则
- **千万别** 把明文密码（Secret）直接传上来！
- 所有的敏感信息都要先用 `kubeseal` 加密成 `SealedSecret`。
- `.gitignore` 已经帮我拦住了一些敏感文件，但自己提交前还是要留个心眼。
