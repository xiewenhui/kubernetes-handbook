# Container Runtime Interface

Container Runtime Interface (CRI)是Kubelet 1.5/1.6中主要负责的一块项目，它重新定义了Kubelet Container Runtime API，将原来完全面向Pod级别的API拆分成面向Sandbox和Container的API，并分离镜像管理和容器引擎到不同的服务。

![](images/cri.png)

CRI最早从从1.4版就开始设计讨论和开发，在v1.5中发布第一个测试版。在v1.6时已经有了很多外部Runtime，如frakti、cri-o的alpha支持。

## 如何开发新的Container Runtime

新的Container Runtime需要实现[CRI gRPC Server](https://github.com/kubernetes/kubernetes/tree/master/pkg/kubelet/apis/cri/v1alpha1)，包括RuntimeService和ImageService。该gRPC Server需要监听在本地的unix socket（Linux支持unix socket格式，Windows支持tcp格式），并在启动kubelet时将其传入：

```sh
kubelet --container-runtime=remote --container-runtime-endpoint=/var/run/frakti.sock ..
```

## 目前支持的Container Runtime

目前，有多家厂商都在基于CRI集成自己的容器引擎，其中包括

- Docker: 核心代码依然保留在kubelet内部，依然是最稳定和特性支持最好的Runtime
- HyperContainer: <https://github.com/kubernetes/frakti>，已支持Kubernetes v1.6
- Rkt: <https://github.com/kubernetes-incubator/rktlet>
- Runc有两个实现，cri-o和cri-containerd
  - [cri-containerd](https://github.com/kubernetes-incubator/cri-containerd)
  - [cri-o](https://github.com/kubernetes-incubator/cri-o)，已支持Kubernetes v1.6
- Mirantis: <https://github.com/Mirantis/virtlet>
