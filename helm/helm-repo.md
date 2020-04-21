## 官方 helm 仓库

helm 官方的仓库如下所示:

```yaml
https://kubernetes-charts.storage.googleapis.com/
---
stable: https://hub.kubeapps.com/charts/stable
incubator: https://hub.kubeapps.com/charts/incubator
```

## 微软 helm 仓库镜像

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
NAME           	URL                                                 
azure-stable   	https://mirror.azure.cn/kubernetes/charts/          
azure-incubator	https://mirror.azure.cn/kubernetes/charts-incubator/
```

从 helm 仓库找到需要的 chart：

```bash
$ helm search repo redis
NAME                                  	CHART VERSION	APP VERSION  	DESCRIPTION                        
azure-incubator/redis-cache           	0.5.0        	4.0.12-alpine	A pure in-memory redis cache, using statefulset...
azure-stable/prometheus-redis-exporter	3.3.3        	1.3.4        	Prometheus exporter for Redis metrics             
azure-stable/redis                    	10.5.7       	5.0.7        	DEPRECATED Open source, advanced key-value stor...
azure-stable/redis-ha                 	4.4.4        	5.0.6        	Highly available Kubernetes implementation of R...
azure-stable/sensu                    	0.2.3        	0.28         	Sensu monitoring framework backed by the Redis ...
```

