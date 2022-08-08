# Kubernetes 入门

## 0. 学习Kubernetes基础知识

### Kubernetes功能
- k8s能够**对容器化软件**进行**部署管理**，在**不停机的前提**下提供简单快速的**发布和更新**方式

### k8s集群简单介绍
![](https://kuboard.cn/assets/img/module_01.f6dc9f93.svg)
上图描述的是拥有一个**Master(主)节点**和六个**Worker(工作)节点**的k8s集群  
![](https://kuboard.cn/assets/img/module_01_cluster.8f54b2c5.svg)
- **Master负责管理集群**
  - 负责**协调**集群中的**所有活动**，
  - 例如
    - **调度**应用程序，
    - **维护**应用程序的状态，
    - **扩展和更新**应用程序。
- **Worker节点(即图中的Node)**是**VM(虚拟机)**或**物理计算机**，充当k8s集群中的**工作计算机**
  - 每个Worker节点都有一个**Kubelet**，
    - **管理**该Worker节点
    - 并负责**与Master节点通信**
- 该Worker节点还应具有用于**处理容器操作的工具**，例如Docker

## 1.部署一个应用程序

### Kubernetes 部署
- **Deployment** 译名为 部署
  - 在k8s中，通过发布**Deployment**，可以创建**应用程序(docker image)**的实例**(docker container)**
    - 这个实例会被包含在称为 **Pod** 的概念中，Pod 是 k8s 中**最小可管理单元**
  - 在k8s集群中发布Deployment后，Deployment **将指示** k8s 如何创建和更新应用程序的实例，
    - **master** 节点将应用程序实例调度到集群中的**具体的节点**上