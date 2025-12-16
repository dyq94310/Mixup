目录结构
```bash
k3s/
├─ README.md
├─ docs/                      # 文档：运维手册、迁移记录、FAQ
│  ├─ ops.md
│  └─ tailscale-k3s.md
├─ infra/                     # 通用基础设施组件（可复用）
│  ├─ ingress-nginx/
│  │  ├─ base/
│  │  └─ overlays/
│  ├─ cert-manager/
│  ├─ sealed-secrets/
│  ├─ metrics-server/
│  └─ kube-dashboard/
├─ apps/                      # 业务应用（可复用）
│  ├─ bitwarden/
│  │  ├─ base/
│  │  └─ overlays/
│  ├─ komari/
│  ├─ sing-box/
│  └─ whoami/
├── .gitignore
└── README.md

```