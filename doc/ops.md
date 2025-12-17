常用命令

开一个网络pod测试
```bash
kubectl run -it --rm netshoot-ccs   --image=nicolaka/netshoot   --overrides='{"spec":{"nodeSelector":{"kubernetes.io/hostname":"ccs"}}}'   --restart=Never -- bash
```

Sidecar 调试模式
请使用 kubectl debug 命令，这会启动一个带有 Shell 的 busybox 容器，并让它共享 Goldpinger 的进程空间。

比如这样看hostname
```bash
kubectl debug -it goldpinger-z7v4k \
  --image=busybox:1.36 \
  --target=goldpinger \
  --profile=sysadmin \
  -- sh

cat /proc/1/environ | tr '\0' '\n' | grep HOSTNAME
```