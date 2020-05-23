## kubectl 备忘单

kubectl 常用操作梳理.

#### `kubectl apply`

使用 example-service.yaml 文件创建或者更新服务对象：

```bash
$ kubectl apply -f example-service.yaml
```

使用 <directory> 路径下的任意 `.yaml` 、`.yml` 或  `.json`  文件创建资源对象:

```bash
$ kubectl apply -f <directory>
```

#### `kubectl get`

常用 `kubectl get` 命令查看 pod 信息：

```bash
$ kubectl get pods -n <namespace>
```

> <namespace> 用来指定资源所在命名空间

获取 pod 时包含节点、IP 等附加信息：

```bash
$ kubectl get pods -n <namespace> -o wide
```

获取多个资源，用逗号分割：

```bash
$ kubectl get pods,service -n <namespace>
```

查看某个 node 上的所有 pod：

```bash
$ kubectl get pods --field-selector=spec.nodeName=<node_name>
```

