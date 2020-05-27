## Kubernetes 常用操作指南

### 检查 kubelet 证书有效期

```bash
$ openssl x509 -in <kubelet_crt> -noout -text | grep -A 2 "Validity"
Validity
            Not Before: May 12 06:21:00 2020 GMT
            Not After : Apr  2 18:21:00 2030 GMT
```

输出上述样式，可以看到该证书有效期为 10 年