# Traefik 在 k3s + Tailscale 混合组网中的部署与调度说明

本文记录当前集群中 Traefik 的部署方式、调度策略选择，以及在 Tailscale 组网环境下的常见注意点与排查手法。

---

## 1. 背景与现状

- 需求：  
  - **Ingress 只部署在具备公网入口的海外节点**
---

## 2. 当前 Traefik 配置关键点（HelmChartConfig）

[traefik-host.yam配置文件](../infra/kube-system/traefik-host.yaml)

当前使用 k3s 自带 Traefik HelmChart，通过 `HelmChartConfig` 覆盖 values：

- `deployment.kind: DaemonSet`
- 使用 `hostPort` 直接在宿主机监听端口：
  - `web.hostPort: 80`
  - `websecure.hostPort: 443`
- `service.enabled: false`

### 2.1 hostPort 模式的含义（非常重要）

当 `service.enabled: false` 且使用 `hostPort: 80/443` 时：

- **只有运行了 Traefik Pod 的节点**，宿主机才会真正监听 `80/443`
- DNS / 入口流量必须打到“跑了 Traefik 的节点”
- 没跑 Traefik 的节点上，访问 `80/443` 会：
  - 直接连接失败（端口未监听），或
  - 被其他进程占用导致异常

**结论：** Traefik 是否调度到某节点，直接决定该节点是否具备对外 Ingress 入口能力。

---

## 3. 调度策略选型

### 3.1 目前ingress为白名单（nodeSelector）模式

当前 values 中使用：

```yaml
nodeSelector:
  ingress: "true"
```

效果：

* Traefik **只会调度到带 `ingress=true` 标签的节点**
* 新增节点默认没有标签 ⇒ **默认不会跑 Traefik**
* 优点：入口节点可控，默认不暴露 80/443，更安全
* 缺点：新增“应该跑 Ingress 的节点”需要手动打标签（或自动化）

#### 常用操作

给某节点启用 Ingress：

```bash
kubectl label node <node-name> ingress=true --overwrite
```

查看标签：

```bash
kubectl get node <node-name> --show-labels
```

### 3.3 自动化白名单：新节点加入时自动带 ingress=true

如果仍想坚持白名单（更严格的入口控制），可以在节点加入时自动设置 label（避免手动打标签）。

在该节点的 `k3s-agent` 配置中加：

`/etc/rancher/k3s/config.yaml`

```yaml
node-label:
  - "ingress=true"
```

或启动参数添加：

```bash
--node-label ingress=true
```

这样新加入的“入口节点”会自动打上 `ingress=true`。

---


## 4. 运维常用命令清单

### 4.1 查看 Traefik 是否覆盖到目标节点

```bash
kubectl -n kube-system get pods -l app.kubernetes.io/name=traefik -o wide
```

### 4.2 查看 Ingress / Service / Endpoint

```bash
kubectl describe ingress <ingress-name>
kubectl get svc <svc-name> -o wide
kubectl get endpointslice -l kubernetes.io/service-name=<svc-name> -o wide
```

### 4.3 Traefik 日志（看超时/502/route）

```bash
kubectl -n kube-system logs -l app.kubernetes.io/name=traefik --tail=200
```

### 4.4 检查宿主机 80/443 是否监听（hostPort 模式）

在目标节点上：

```bash
sudo ss -lntp | egrep ':(80|443)\b'
```
