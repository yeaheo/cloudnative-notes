## kubectl 备忘单

kubectl 常用操作梳理：

`kubectl apply`

使用 example-service.yaml 文件创建或者更新服务对象：

```bash
$ kubectl apply -f example-service.yaml
```

使用 <directory> 路径下的任意 `.yaml` 、`.yml` 或  `.json`  文件创建资源对象:

```bash
$ kubectl apply -f <directory>
```

`kubectl get`