## ETCD 备份

所有 Kubernetes 对象都存储在 etcd 中。所以定期备份 etcd 集群数据对于在灾难情况下（例如丢失所有主节点）恢复 Kubernetes 集群非常重要。

快照文件包含所有Kubernetes状态和关键信息。为了确保敏感的 Kubernetes 数据安全，可以加密快照文件。

备份 etcd 集群有两种方式，分别为:

- etcd 内置的快照方式
- 卷快照

### 内置的快照方式（Built-in snapshot）

我们可以用 `etcdctl snapshot save` 保存快照，具体备份细节如下：

```bash
$ ETCDCTL_API=3 etcdctl --endpoints $ENDPOINT snapshot save snapshotdb
# exit 0

# verify the snapshot
$ ETCDCTL_API=3 etcdctl --write-out=table snapshot status snapshotdb
+----------+----------+------------+------------+
|   HASH   | REVISION | TOTAL KEYS | TOTAL SIZE |
+----------+----------+------------+------------+
| fe01cf57 |       10 |          7 | 2.1 MB     |
+----------+----------+------------+------------+
```

大多数情况下，我们的 etcd 集群是通过 tls 证书认证的，这里可以加入证书相关参数：

```bash
$ ETCDCTL_API=3 etcdctl --endpoints $ENDPOINT --cacert=ca.pem --cert=cert.pem --key=key.pem snapshot save snapshotdb
```

这里举例说明，我是 `kind` 安装的集群：

```bash
$ kubectl get nodes
NAME                  STATUS   ROLES    AGE   VERSION
myk8s-control-plane   Ready    master   73m   v1.17.0
myk8s-worker          Ready    <none>   72m   v1.17.0
```

获取 etcd 集群的 `endpoints` 和 `tls` 证书相关内容:

```bash
$ kubectl -n kube-system describe pods kube-apiserver-myk8s-control-plane | grep 'etcd'
      --etcd-cafile=/etc/kubernetes/pki/etcd/ca.crt
      --etcd-certfile=/etc/kubernetes/pki/apiserver-etcd-client.crt
      --etcd-keyfile=/etc/kubernetes/pki/apiserver-etcd-client.key
      --etcd-servers=https://127.0.0.1:2379
```

拼接备份命令:

```bash
$ ETCDCTL_API=3 etcdctl --endpoints https://127.0.0.1:2379 \
--cacert=/etc/kubernetes/pki/etcd/ca.crt \
--cert=/etc/kubernetes/pki/apiserver-etcd-client.crt \
--key=/etc/kubernetes/pki/apiserver-etcd-client.key \
snapshot save snapshotdb
```

官方参考链接：[etcd 创建快照](https://kubernetes.io/docs/tasks/administer-cluster/configure-upgrade-etcd/#backing-up-an-etcd-cluster)