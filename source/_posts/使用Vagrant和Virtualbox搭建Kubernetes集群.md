---
date: 2018-07-08 17:00:16
---

# 使用Vagrant和Virtualbox搭建Kubernetes集群

## 安装Vagrant和VirtualBox

```shell
brew cask install vagrant
brew cask install virtualbox
```

## 使用说明

确保安装好以上的准备环境后，执行下列命令启动kubernetes集群：

```shell
git clone https://github.com/ovwane/kubernetes-vagrant-centos-cluster.git
cd kubernetes-vagrant-centos-cluster
vagrant up
```

**注意**：克隆完Git仓库后，需要提前下载kubernetes的压缩包到`kubenetes-vagrant-centos-cluster`目录下，包括如下两个文件：

- kubernetes-client-linux-amd64.tar.gz
- kubernetes-server-linux-amd64.tar.gz

如果是首次部署，会自动下载`centos/7`的box，这需要花费一些时间，另外每个节点还需要下载安装一系列软件包，整个过程大概需要10几分钟。

如果您在运行`vagrant up`的过程中发现无法下载`centos/7`的box，可以手动下载后将其添加到vagrant中。

**手动添加centos/7 box**

```
wget -c http://cloud.centos.org/centos/7/vagrant/x86_64/images/CentOS-7-x86_64-Vagrant-1805_01.VirtualBox.box
vagrant box add CentOS-7-x86_64-Vagrant-1805_01.VirtualBox.box --name centos/7
```

这样下次运行`vagrant up`的时候就会自动读取本地的`centos/7` box而不会再到网上下载。

### 访问kubernetes集群

访问Kubernetes集群的方式有三种：

- 本地访问
- 在VM内部访问
- kubernetes dashboard

**通过本地访问**

可以直接在你自己的本地环境中操作该kubernetes集群，而无需登录到虚拟机中，执行以下步骤：

将`conf/admin.kubeconfig`文件放到`~/.kube/config`目录下即可在本地使用`kubectl`命令操作集群。

```
mkdir -p ~/.kube
cp conf/admin.kubeconfig ~/.kube/config
```

我们推荐您使用这种方式。

**在虚拟机内部访问**

如果有任何问题可以登录到虚拟机内部调试：

```
vagrant ssh node1
sudo -i
kubectl get nodes
```

**Kubernetes dashboard**

还可以直接通过dashboard UI来访问：[https://172.17.8.101:8443](https://172.17.8.101:8443/)

可以在本地执行以下命令获取token的值（需要提前安装kubectl）：

```
kubectl -n kube-system describe secret `kubectl -n kube-system get secret|grep admin-token|cut -d " " -f1`|grep "token:"|tr -s " "|cut -d " " -f2
```

**注意**：token的值也可以在`vagrant up`的日志的最后看到。

**Heapster监控**

创建Heapster监控：

```
kubectl apply -f addon/heapster/
```

访问Grafana

使用Ingress方式暴露的服务，在本地`/etc/hosts`中增加一条配置：

```
172.17.8.102 grafana.jimmysong.io
```

访问Grafana：[http://grafana.jimmysong.io](http://grafana.jimmysong.io/)

**Traefik**

部署Traefik ingress controller和增加ingress配置：

```
kubectl apply -f addon/traefik-ingress
```

在本地`/etc/hosts`中增加一条配置：

```
172.17.8.102 traefik.jimmysong.io
```

访问Traefik UI：[http://traefik.jimmysong.io](http://traefik.jimmysong.io/)

**EFK**

使用EFK做日志收集。

```
kubectl apply -f addon/efk/
```

**注意**：运行EFK的每个节点需要消耗很大的CPU和内存，请保证每台虚拟机至少分配了4G内存。

**Helm**

用来部署helm。

```
hack/deploy-helm.sh
```

### Service Mesh

我们使用 [istio](https://istio.io/) 作为 service mesh。

**安装**

```
kubectl apply -f addon/istio/
```

**运行示例**

```
kubectl apply -n default -f <(istioctl kube-inject -f yaml/istio-bookinfo/bookinfo.yaml)
istioctl create -f yaml/istio-bookinfo/bookinfo-gateway.yaml
```

在您自己的本地主机的`/etc/hosts`文件中增加如下配置项。

```
172.17.8.102 grafana.istio.jimmysong.io
172.17.8.102 servicegraph.istio.jimmysong.io
```

我们可以通过下面的URL地址访问以上的服务。

| Service      | URL                                                          |
| ------------ | ------------------------------------------------------------ |
| grafana      | [http://grafana.istio.jimmysong.io](http://grafana.istio.jimmysong.io/) |
| servicegraph | [http://servicegraph.istio.jimmysong.io/dotviz>](http://servicegraph.istio.jimmysong.io/dotviz%3E), <http://servicegraph.istio.jimmysong.io/graph>,<http://servicegraph.istio.jimmysong.io/force/forcegraph.html> |
| tracing      | [http://172.17.8.101:$JAEGER_PORT](http://172.17.8.101:$JAEGER_PORT/) |
| productpage  | <http://172.17.8.101:$GATEWAY_PORT/productpage>              |

**注意**：`JAEGER_PORT`可以通过`kubectl -n istio-system get svc tracing -o jsonpath='{.spec.ports[0].nodePort}'`获取，`GATEWAY_PORT`可以通过`kubectl -n istio-system get svc istio-ingressgateway -o jsonpath='{.spec.ports[0].nodePort}'`获取。

详细信息请参阅 <https://istio.io/docs/guides/bookinfo.html>

## 管理

除了特别说明，以下命令都在当前的repo目录下操作。

### 挂起

将当前的虚拟机挂起，以便下次恢复。

```
vagrant suspend
```

### 恢复

恢复虚拟机的上次状态。

```
vagrant resume
```

注意：我们每次挂起虚拟机后再重新启动它们的时候，看到的虚拟机中的时间依然是挂载时候的时间，这样将导致监控查看起来比较麻烦。因此请考虑先停机再重新启动虚拟机。

### 重启

停机后重启启动。

```
vagrant halt
vagrant up
# login to node1
vagrant ssh node1
# run the prosivision scripts
/vagrant/hack/k8s-init.sh
exit
# login to node2
vagrant ssh node2
# run the prosivision scripts
/vagrant/hack/k8s-init.sh
exit
# login to node3
vagrant ssh node3
# run the prosivision scripts
/vagrant/hack/k8s-init.sh
sudo -i
cd /vagrant/hack
./deploy-base-services.sh
exit
```

现在你已经拥有一个完整的基础的kubernetes运行环境，在该repo的根目录下执行下面的命令可以获取kubernetes dahsboard的admin用户的token。

```
hack/get-dashboard-token.sh
```

根据提示登录即可。

### 清理

清理虚拟机。

```
vagrant destroy
rm -rf .vagrant
```

### 注意

仅做开发测试使用，不要在生产环境使用该项目。

## 参考

[本地分布式开发环境搭建（使用Vagrant和Virtualbox）](https://jimmysong.io/kubernetes-handbook/develop/using-vagrant-and-virtualbox-for-development.html)