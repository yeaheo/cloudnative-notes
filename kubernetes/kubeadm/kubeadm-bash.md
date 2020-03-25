## kubeadm 杂谈

### 集群添加节点 token 相关

如果使用 `kubeadm` 工具创建的集群，当 `kubeadm init` 初始化控制面节点后，默认会提供一个类似如下样式的 `kubeadm join` 命令,我们需要对其做记录：

```bash
# kubeadm init 示例:
$ kubeadm join --discovery-token abcdef.1234567890abcdef --discovery-token-ca-cert-hash sha256:1234..cdef 1.2.3.4:6443
```

但有时候我们会忘记上述命令，这个时候我们需要重新获取，这里提供两种方式，详细说明如下：

#### 利用之前的 token

列出当前可用 token 如下:

```bash
$ kubeadm token list
TOKEN                     TTL     EXPIRES               USAGES                   DESCRIPTION                                                 EXTRA GROUPS
abcdef.0123456789abcdef   2h      2020-03-19T11:53:03Z   authentication,signing   <none>                                                     system:bootstrappers:kubeadm:default-node-token
```

如果有可用 token 可以重复利用，如果没有可以直接新建 `kubeadm join` 相关命令。

获取 ca 证书 sha256 编码 hash 值:

```bash
$ openssl x509 -pubkey -in /etc/kubernetes/pki/ca.crt | openssl rsa -pubin -outform der 2>/dev/null | openssl dgst -sha256 -hex | sed 's/^.* //'
e082fbb4ab7684d9f8537ddec49924a1e77a058f0db5929d66b8e429a8fa8367
```

重组 kubeadm join 命令:

```bash
$ kubeadm join --discovery-token abcdef.1234567890abcdef --discovery-token-ca-cert-hash sha256:e082fbb4ab7684d9f8537ddec49924a1e77a058f0db5929d66b8e429a8fa8367 1.2.3.4:6443
```

#### 生成新的 token

如下:

```bash
$ kubeadm token create --print-join-command
kubeadm join 172.17.0.2:6443 --token i8s131.w7mfu9ppflygkqgb     --discovery-token-ca-cert-hash sha256:e082fbb4ab7684d9f8537ddec49924a1e77a058f0db5929d66b8e429a8fa8367
```

查看新的 token 如下:

```bash
$ kubeadm token list
TOKEN                     TTL         EXPIRES                USAGES                   DESCRIPTION                                                EXTRA GROUPS
abcdef.0123456789abcdef   2h          2020-03-19T11:53:03Z   authentication,signing   <none>                                                     system:bootstrappers:kubeadm:default-node-token
i8s131.w7mfu9ppflygkqgb   23h         2020-03-20T09:28:54Z   authentication,signing   <none>                                                     system:bootstrappers:kubeadm:default-node-token
```

利用上述的 `kubeadm join` 加入集群即可：

```bash
$ kubeadm token create --print-join-command
kubeadm join 172.17.0.2:6443 --token i8s131.w7mfu9ppflygkqgb     --discovery-token-ca-cert-hash sha256:e082fbb4ab7684d9f8537ddec49924a1e77a058f0db5929d66b8e429a8fa8367
```









 



