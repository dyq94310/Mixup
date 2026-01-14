# K9s 使用指南（简明版）

> K9s 是一个 **基于终端的 Kubernetes 集群可视化管理工具**，用于快速查看
> Pod / Node / Service 状态、日志、事件，并支持直接 exec 进入容器排障。
> 不需要在集群中部署任何组件。

---

## 1. 安装

### Linux（推荐）

```bash
curl -sS https://webinstall.dev/k9s | bash
```

或从官方 Release 下载单文件二进制：
[https://github.com/derailed/k9s/releases](https://github.com/derailed/k9s/releases)

### macOS

```bash
brew install k9s
```

### Windows

* 下载 `k9s_Windows_x86_64.zip`
* 解压后放入 `PATH`

---

## 2. 启动

确保本机已配置 `kubeconfig`：

```bash
k9s
```

> 默认使用 `~/.kube/config`
> 支持多集群 / 多 context 切换

---

## 3. 基本操作

### 3.1 切换资源视图（命令模式）

在 K9s 中按 `:` 进入命令模式：

| 命令        | 说明          |
| --------- | ----------- |
| `:po`     | Pods        |
| `:no`     | Nodes       |
| `:svc`    | Services    |
| `:ep`     | Endpoints   |
| `:deploy` | Deployments |
| `:ds`     | DaemonSets  |
| `:ing`    | Ingress     |
| `:ns`     | Namespaces  |
| `:ev`     | Events      |

---

## 4. Pod 常用操作（排障高频）

在 `:po` 视图中选中 Pod：

| 快捷键      | 功能               |
| -------- | ---------------- |
| `Enter`  | 查看 Pod 详情        |
| `l`      | 查看日志             |
| `L`      | 选择容器后查看日志        |
| `s`      | 进入容器 Shell（exec） |
| `d`      | Describe Pod     |
| `r`      | 重启 Pod           |
| `Ctrl+D` | 删除 Pod           |
| `y`      | 查看 YAML          |

---

## 5. Node 排障

进入 Node 视图：

```text
:no
```

| 快捷键     | 功能               |
| ------- | ---------------- |
| `Enter` | Node 详情          |
| `d`     | Describe Node    |
| `p`     | 查看该 Node 上的 Pods |

---

## 6. Service / 网络排查

| 资源        | 命令     |
| --------- | ------ |
| Service   | `:svc` |
| Endpoints | `:ep`  |
| Ingress   | `:ing` |

## 自定义 K9s Plugin (一键 Debug)
你可以通过 K9s 的插件功能，自定义一个快捷键（比如 ``shift-g``），一键启动你最常用的调试环境。

编辑 ``~/.config/k9s/plugins.yaml``：
```yaml
plugins:
  debug:
    shortCut: Shift-D
    confirm: false
    description: "Add Debug Container"
    scopes:
      - pods
    command: kubectl
    background: false
    args:
      - debug
      - -it
      - $NAME
      - -n
      - $NAMESPACE
      - --image=nicolaka/netshoot
      - --profile=general
      # 显式指定拉取策略，防止镜像版本缓存问题
      - --image-pull-policy=IfNotPresent
```


**常见排查顺序**：

1. `:svc` 看端口配置
2. `:ep` 看是否有后端 Pod
3. `:po` 看 Pod 是否 Ready

---

## 7. 常用通用快捷键

| 快捷键      | 说明             |
| -------- | -------------- |
| `/`      | 搜索 / 过滤        |
| `Ctrl+A` | 显示所有 Namespace |
| `Esc`    | 返回上一级          |
| `?`      | 当前视图帮助         |
| `Ctrl+L` | 清屏             |
| `q`      | 退出             |

---

## 8. 常见使用场景

### 查看 Pod 日志

```text
:po → 选中 Pod → l
```

### 进入容器排查

```text
:po → 选中 Pod → s
```

### 查看集群事件

```text
:ev
```

### 查看某节点上运行的 Pod

```text
:no → 选中 Node → p
```

---

## 9. 常见问题

### Q: K9s 是否会在集群中部署组件？

**不会。**
K9s 是本地 CLI 工具，仅通过 Kubernetes API 访问集群。

### Q: 是否支持多集群？

支持，使用 `kubectl config use-context` 或在 K9s 内切换。

---

## 10. 推荐搭配工具

| 工具               | 用途     |
| ---------------- | ------ |
| `kubectl`        | 精细化操作  |
| `kubectl debug`  | 深度排障   |
| `tailscale ping` | 节点网络排查 |
| `netshoot`       | 网络调试容器 |

---

## 一句话总结

记住这 6 个就能 80% 排障：
``:po`` ``l`` ``s`` ``d`` ``:no`` ``:svc``


## 11. 参考链接

* 官方仓库：[https://github.com/derailed/k9s](https://github.com/derailed/k9s)
* 官方文档：[https://k9scli.io/](https://k9scli.io/)
