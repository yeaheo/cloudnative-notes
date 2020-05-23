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

#### `kubectl delete`

使用 pod.yaml 文件中指定的类型和名称删除 pod：

```bash
$ kubectl delete -f pod.yaml
```

删除标签名= <label-name> 的所有 pod 和服务:

```bash
$ kubectl delete pods,service -l name=<label-name>
```

删除所有 pod，包括未初始化的 pod:

```bash
$ kubectl delete pods --all
```

#### ```kubectl exec```

从 pod <pod-name> 中获取运行 'date' 的输出。默认情况下，输出来自第一个容器:

```bash
$ kubectl exec <pod-name> -- date
```

运行命令时指定容器 <container_name>

```bash
$ kubectl exec <pod-name> -c <container_name> -- date
```

进入 pod 内的容器：

```bash
$ kubectl exec -it <pod_name> -- bash/sh
```

