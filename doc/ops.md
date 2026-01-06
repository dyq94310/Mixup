常用命令

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
  --target=goldpinger \
  --profile=sysadmin \
  -- sh

cat /proc/1/environ | tr '\0' '\n' | grep HOSTNAME
```

或者使用netshoot
```bash
kubectl debug -it goldpinger-z7v4k \
  --image=nicolaka/netshoot \
  --target=goldpinger \
  --profile=sysadmin \
  -- sh

cat /proc/1/environ | tr '\0' '\n' | grep HOSTNAME
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