## ETCD 常用操作

ETCD 我们平时常用的操作就那么几种：

- version: 查看版本
- member list: 查看节点
- endpoint status: 节点状态，leader 情况
- endpoint health: 健康状态与耗时
- set app demo: 写入
- get app: 获取
- update app demo1:更新
- rm app: 删除
- mkdir demo 创建文件夹
- rmdir dir 删除文件夹
- backup 备份
- watch key 监测 key 变化
- get / –prefix –keys-only: 查看所有 key

这里只列举几个.

> etcd api 版本为 3
>
> ETCDCTL_API=3

### 查看节点

```bash
$ etcdctl --cacert=/etc/kubernetes/pki/etcd/ca.crt --cert=/etc/kubernetes/pki/etcd/healthcheck-client.crt --key=/etc/kubernetes/pki/etcd/healthcheck-client.key --endpoints ${ETCD_ENDPOINTS} member list -w table
+------------------+---------+------------+--------------------------+--------------------------+------------+
|        ID        | STATUS  |    NAME    |        PEER ADDRS        |       CLIENT ADDRS       | IS LEARNER |
+------------------+---------+------------+--------------------------+--------------------------+------------+
| 2253e2fa75ad1dee | started | etcd-node2 | https://172.30.1.21:2380 | https://172.30.1.21:2379 |      false |
| 9cbb3cd4b5744396 | started | etcd-node1 | https://172.30.1.20:2380 | https://172.30.1.20:2379 |      false |
| d2e66211644ec711 | started | etcd-node3 | https://172.30.1.22:2380 | https://172.30.1.22:2379 |      false |
+------------------+---------+------------+--------------------------+--------------------------+------------+
```

### 查看节点健康状态

```bash
$ etcdctl --cacert=/etc/kubernetes/pki/etcd/ca.crt --cert=/etc/kubernetes/pki/etcd/healthcheck-client.crt --key=/etc/kubernetes/pki/etcd/healthcheck-client.key --endpoints ${ETCD_ENDPOINTS} endpoint health -w table
+--------------------------+--------+-------------+-------+
|         ENDPOINT         | HEALTH |    TOOK     | ERROR |
+--------------------------+--------+-------------+-------+
| https://172.30.1.21:2379 |   true | 26.788842ms |       |
| https://172.30.1.22:2379 |   true | 87.340619ms |       |
| https://172.30.1.20:2379 |   true | 88.982738ms |       |
+--------------------------+--------+-------------+-------+
```

### 查看节点状态（leader 信息）

```bash
$ etcdctl --cacert=/etc/kubernetes/pki/etcd/ca.crt --cert=/etc/kubernetes/pki/etcd/healthcheck-client.crt --key=/etc/kubernetes/pki/etcd/healthcheck-client.key --endpoints ${ETCD_ENDPOINTS} endpoint status -w table
+--------------------------+------------------+---------+---------+-----------+------------+-----------+------------+--------------------+--------+
|         ENDPOINT         |        ID        | VERSION | DB SIZE | IS LEADER | IS LEARNER | RAFT TERM | RAFT INDEX | RAFT APPLIED INDEX | ERRORS |
+--------------------------+------------------+---------+---------+-----------+------------+-----------+------------+--------------------+--------+
| https://172.30.1.20:2379 | 9cbb3cd4b5744396 |   3.4.7 |  2.2 MB |     false |      false |        81 |      94659 |              94659 |        |
| https://172.30.1.21:2379 | 2253e2fa75ad1dee |   3.4.7 |  2.1 MB |     false |      false |        81 |      94659 |              94659 |        |
| https://172.30.1.22:2379 | d2e66211644ec711 |   3.4.7 |  2.1 MB |      true |      false |        81 |      94659 |              94659 |        |
+--------------------------+------------------+---------+---------+-----------+------------+-----------+------------+--------------------+--------+
```

### 查看所有 key

```bash
$ etcdctl --cacert=/etc/kubernetes/pki/etcd/ca.crt --cert=/etc/kubernetes/pki/etcd/healthcheck-client.crt --key=/etc/kubernetes/pki/etcd/healthcheck-client.key --endpoints ${ETCD_ENDPOINTS} get / --prefix --keys-only
...
/registry/services/endpoints/kubernetes-dashboard/kubernetes-dashboard

/registry/services/specs/default/kubernetes

/registry/services/specs/kube-system/kube-dns

/registry/services/specs/kube-system/metrics-server

/registry/services/specs/kube-system/traefik

/registry/services/specs/kubernetes-dashboard/dashboard-metrics-scraper
...
```

### 查看某个 key

```bash
$ etcdctl --cacert=/etc/kubernetes/pki/etcd/ca.crt --cert=/etc/kubernetes/pki/etcd/healthcheck-client.crt --key=/etc/kubernetes/pki/etcd/healthcheck-client.key --endpoints ${ETCD_ENDPOINTS} get /registry/services/specs/kubernetes-dashboard/kubernetes-dashboard -w=json | jq .
```

