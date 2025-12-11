
**1 填空 (编辑文件)**

每次有新项目，复制这个模版，填入你的真密码。比如 `plain.yaml`：

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: komari-secret
  namespace: default
type: Opaque
stringData:
  ADMIN_PASSWORD: "MySuperStrongPassword123"
  KOMARI_CLOUDFLARED_TOKEN: "eyJhbGciOi..."
```

**3. 一键加密**

直接把这个文件喂给 `kubeseal`：

```bash
kubeseal --format=yaml < plain.yaml > sealed-secret.yaml
```

**4. 删掉明文**

```bash
rm plain.yaml
```

**5. apply密文文件**

```bash
apply -f sealed-secret.yaml

```

**6.修改项目yaml,引用密文**

```yaml
env:
        - name: ADMIN_PASSWORD
          valueFrom:
            secretKeyRef:
              name: komari-secret
              key: ADMIN_PASSWORD  # 引用同一个 Secret 里的不同 Key
        - name: KOMARI_CLOUDFLARED_TOKEN
          valueFrom:
            secretKeyRef:
              name: komari-secret
              key: KOMARI_CLOUDFLARED_TOKEN
```

