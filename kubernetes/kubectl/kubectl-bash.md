## kubectl 常用命令

### 获取集群证书相关信息
```bash
# 获取集群 CA 证书
$ kubectl -n xx get secrets xx -o json | jq -r '.data["ca.crt"]' | base64 -d

# 获取 SA 的 token
kubectl -n xx get secrets xx -o json | jq -r '.data["token"]' | base64 -d
```

### 创建集群管理员账号
```bash
# 创建 serviceaccount
$ kubectl create serviceaccount dashboard-admin -n kube-system
> 创建完成后会有一个对应的 secret

# 创建 clusterrolebinding
$ kubectl create clusterrolebinding dashboard-cluster-admin --clusterrole=cluster-admin --serviceaccount=kube-system:dashboard-admin

# 获取 kube-system 的 secret
$ kubectl get secrets -n kube-system

# 获取 token
$ kubectl describe secrets dashboard-admin-token-pmr5z -n kube-system
```

### 准备链接集群的配置文件
```bash
# 创建集群
$ kubectl config set-cluster kubernetes --certificate-authority=/etc/kubernetes/pki/ca.crt --server="xxx" --embed-certs=true --kubeconfig=/opt/work/default-ns-admin.conf

# 获取 token
$ export CLUSTER_ADMIN_USER_TOKEN=$(kubectl get secrets -n kube-system dashboard-admin-token-pmr5z -o jsonpath={.data.token} | base64 -d)

# 验证 token
$ echo $CLUSTER_ADMIN_USER_TOKEN

# 创建用户
$ kubectl config set-credentials cluster-admin --token=$CLUSTER_ADMIN_USER_TOKEN --kubeconfig=/opt/work/default-ns-admin.conf

# 创建上下文
$ kubectl config set-context cluster-admin@kubernetes --cluster=kubernetes --user=cluster-admin --kubeconfig=/opt/work/default-ns-admin.conf

# 应用上下文
$ kubectl config use-context --kubeconfig=/opt/work/default-ns-admin.conf

# 查看完成的配置文件内容
$ kubectl config view --kubeconfig=/opt/work/default-ns-admin.conf
```

### 创建连接私有镜像仓库的 Secret
```bash
$ kubectl create secret docker-registry xx --docker-server=xx --docker-username='xx' --docker-password='xx' -n xx
```

### 创建用于 Ingress 的 Secret
```bash
$ kubectl create secret tls xx --key xx --cert xx -n xx
```

### 节点管理

在集群节点出问题时，可能希望把一个节点不再被调度 pod 或把节点当前存在的 pod 都驱逐出去

```bash
# 标记 node 为 unschedulable , 禁止 pod 被调度过来。注意这时现有的 pod 还会继续运行，不会被驱逐。
$ kubectl cordon <node_name>

# 与 cordon 相反，标记 node 为允许调度。
$ kubectl uncordon <node_name>

# 把 node 的当前 pod 平滑切换到其他 node，同时标记 node 为 unschedulable，也就是包含了 cordon 效果。
$ kubectl drain  <node_name>

## 但是直接使用命令一般不会成功，建议在要维护节点时，加上以下参数：
$ kubectl drain <node_name>  --ignore-daemonsets  --force  --delete-local-data

s## --ignore-daemonsets 忽略 daemonset 部署的 pod
## --force 直接删除不由 workload 对象（Deployment、Job等）管理的 pod
## --delete-local-data  直接删除挂载有本地目录(empty-dir方式）的 pod
```