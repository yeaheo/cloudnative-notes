## kubectl 拷贝容器文件

有时候需要将外部文件拷贝到运行 pod 内，或者将运行 pod 内的文件拷贝到外部，这个时候需要用到 `kubectl cp` 命令。

### Local to Remote

拷贝本地文件到运行 pod 容器：

```bash
$ kubectl cp /tmp/foo_dir <some-pod>:/tmp/bar_dir
```

如果一个 pod 内包含多个容器，需要 `-c` 指定容器：

```bash
$ kubectl cp /tmp/foo <some-pod>:/tmp/bar -c <specific-container>
```

如果需要确认所在 pod 的 namespace 参考如下即可：

```bash
$ kubectl cp /tmp/foo <some-namespace>/<some-pod>:/tmp/bar
```

### Remote to Local

拷贝运行 pod 容器文件到本地，其实可以参考拷贝本地文件至 pod 容器，只需要将源地址和目的地址对调即可。

### Links

- https://kubectl.docs.kubernetes.io/pages/container_debugging/copying_container_files.html