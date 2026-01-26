常用命令

在某个节点上搞事，加污点
```bash
kubectl taint nodes unode key=value:NoExecute
NoSchedule ：表示k8s将不会将Pod调度到具有该污点的Node上
PreferNoSchedule ：表示k8s将尽量避免将Pod调度到具有该污点的Node上
NoExecute ：表示k8s将不会将Pod调度到具有该污点的Node上，同时会将Node上已经存在的Pod驱逐出去

搞完事情去污点
kubectl taint nodes unode key:NoExecute-
kubectl taint nodes unode key-
```

开一个网络pod测试
```bash
kubectl run -it --rm netshoot-ccs   --image=nicolaka/netshoot   --overrides='{"spec":{"nodeSelector":{"kubernetes.io/hostname":"ccs"}}}'   --restart=Never -- bash
```

调试模式
请使用 kubectl debug 命令，这会启动一个带有 Shell 的 busybox 容器，并让它共享 Goldpinger 的进程空间。

比如这样看hostname
```bash
kubectl debug -it goldpinger-z7v4k \
  --image=busybox:1.36 \
  --profile=sysadmin \
  -- sh

cat /proc/1/environ | tr '\0' '\n' | grep HOSTNAME
```

或者使用netshoot
```bash
kubectl debug -it goldpinger-z7v4k \
  --image=nicolaka/netshoot \
  --profile=sysadmin \
  -- sh

cat /proc/1/environ | tr '\0' '\n' | grep HOSTNAME
```


控制busybox在哪个机器
```
kubectl -n default run curltest --rm -it --image=busybox:1.36 \
  --overrides='{"spec":{"nodeName":"ccs"}}' -- sh
```

## 快速查看日志

使用 awk 设定“终点线”（最推荐）

这种方法最优雅，因为一旦匹配到结束时间，awk 会立即退出，不会继续读取后续没用的日志，节省资源。
```bash
kubectl logs <pod-name> -c <container-name> --since-time="2026-01-25T15:30:00Z" \
| awk '$1 > "2026-01-25T15:40:00" {exit} {print}'


kubectl logs sing-box-rear-5c86df9bb6-gzj65 -c sing-box --since-time="2026-01-25T15:30:00+08:00"| awk '$1 > "2026-01-25T15:40:00" {exit} {print}'
```

# 应急命令

## 强制覆盖敏感文件
```bash
# 安装工具（以 Debian/Ubuntu 为例）：
apt install git-filter-repo

# 在本地仓库运行（替换 YOUR_TOKEN_STRING 为你暴露的字符串）
git filter-repo --replace-text <(echo "YOUR_TOKEN_STRING==>REMOVED") --force

# 或者直接删除包含敏感信息的文件：
git filter-repo --path path/to/your/secret_file --invert-paths

# 重新再强制推送
git remote add origin git@github.com:dyq94310/Mixup.git
git push origin --force --all
git push origin --force --tags
```