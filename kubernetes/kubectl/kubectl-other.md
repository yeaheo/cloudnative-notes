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

获取一个没有集群特定信息的 YAML:

```bash
$ kubectl get pods <pod_name> -o yaml --export
```

查看 pod 按名称排序：

```bash
$ kubectl get pods --sort-by='.metadata.name'
```

列出 pods 按照重启次数进行排序:

```bash
$ kubectl get pods --sort-by='.status.containerStatuses[0].restartCount'
```

获取所有工作节点(使用选择器以排除标签名称为 'node-role.kubernetes.io/master' 的结果):

```bash
$ kubectl get nodes --selector='!node-role.kubernetes.io/master'
```

pod 按照创建时间排序：

```bash
$ kubectl -n <namespace> get pods --sort-by='.metadata.creationTimestamp'
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

#### `与运行中的容器进行交互`

```bash
kubectl logs my-pod                                 # 获取 pod 日志(标准输出)
kubectl logs -l name=myLabel                        # 获取 pod label name=myLabel 日志(标准输出)
kubectl logs my-pod --previous                      # 获取上个容器实例的 pod 日志(标准输出)
kubectl logs my-pod -c my-container                 # 获取 pod 的容器日志 (标准输出, 多容器的场景)
kubectl logs -l name=myLabel -c my-container        # 获取 label name=myLabel pod 的容器日志 (标准输出, 多容器的场景)
kubectl logs my-pod -c my-container --previous      # 获取 pod 的上个容器实例日志 (标准输出, 多容器的场景)
kubectl logs -f my-pod                              # 流式输出 pod 的日志 (标准输出)
kubectl logs -f my-pod -c my-container              # 流式输出 pod 容器的日志 (标准输出, 多容器的场景)
kubectl logs -f -l name=myLabel --all-containers    # 流式输出 label name=myLabel pod 的日志 (标准输出)
kubectl run -i --tty busybox --image=busybox -- sh  # 以交互式 shell 运行 pod
kubectl attach my-pod -i                            # 进入到一个运行中的容器中
kubectl port-forward my-pod 5000:6000               # 在本地计算机上侦听端口 5000 并转发到 my-pod 上的端口 6000
kubectl exec my-pod -- ls /                         # 在已有的 pod 中运行命令(单容器的场景)
kubectl exec my-pod -c my-container -- ls /         # 在已有的 pod 中运行命令(多容器的场景)
kubectl top pod POD_NAME --containers               # 显示给定 pod 和容器的监控数据
```

#### ```与节点和集群进行交互```

```bash
kubectl cordon my-node                                                # 设置 my-node 节点为不可调度
kubectl drain my-node                                                 # 对 my-node 节点进行驱逐操作，为节点维护做准备
kubectl uncordon my-node                                              # 设置 my-node 节点为可以调度
kubectl top node my-node                                              # 显示给定 node 的指标
kubectl cluster-info                                                  # 显示 master 和 services 的地址
kubectl cluster-info dump                                             # 将当前集群状态输出到标准输出
kubectl cluster-info dump --output-directory=/path/to/cluster-state   # 将当前集群状态输出到 /path/to/cluster-state

# 如果已存在具有该键和效果的污点，则其值将按指定替换
kubectl taint nodes foo dedicated=special-user:NoSchedule
```

