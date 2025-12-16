常用命令

开一个网络pod测试
```bash
kubectl run -it --rm netshoot-ccs   --image=nicolaka/netshoot   --overrides='{"spec":{"nodeSelector":{"kubernetes.io/hostname":"ccs"}}}'   --restart=Never -- bash
```