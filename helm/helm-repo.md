### 官方 helm 仓库

helm 官方的仓库如下所示:

```yaml
https://kubernetes-charts.storage.googleapis.com/

stable: https://hub.kubeapps.com/charts/stable
incubator: https://hub.kubeapps.com/charts/incubator
```

### 微软 helm 仓库镜像

```yaml
stable: https://mirror.azure.cn/kubernetes/charts/
incubator: https://mirror.azure.cn/kubernetes/charts-incubator/
```

将微软的 `helm` 仓库镜像添加到 `helm` 中:

```bash
$ helm repo add azure-stable https://mirror.azure.cn/kubernetes/charts/
$ helm repo add azure-incubator https://mirror.azure.cn/kubernetes/charts-incubator/
```

查看添加的 `helm` 仓库:

```bash
$ helm repo list
```