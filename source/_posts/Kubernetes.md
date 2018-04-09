# Kubernetes安装配置
[Kubernetes Handbook](https://jimmysong.io/kubernetes-handbook/)

[Kubernetes 最佳实践](https://jimmysong.io/kubernetes-handbook/practice/)

[手动档搭建 Kubernetes HA 集群](https://mritd.me/2017/07/21/set-up-kubernetes-ha-cluster-by-binary/)

## 集群详情
- OS：CentOS Linux release 7.4.1708 (Core) 3.10.0-693.21.1.el7.x86_64
- Kubernetes 1.10.0
- Docker 1.13.1（使用yum安装）
- Etcd 3.2.18
- Flannel 0.10.0 vxlan或者host-gw 网络
- TLS 认证通信 (所有组件，如 etcd、kubernetes master 和 node)
- RBAC 授权
- kubelet TLS BootStrapping
- kubedns、dashboard、heapster(influxdb、grafana)、EFK(elasticsearch、fluentd、kibana) 集群插件
- VMware Harbor 1.4.0 (私有docker镜像仓库，harbor提供离线安装包，直接使用docker-compose启动即可）

[Kubernetes v1.10.0](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG-1.10.md#v1100)

[flannel v0.10.0](https://github.com/coreos/flannel/releases)

[etcd v3.2.18](https://github.com/coreos/etcd/releases)

[VMware Harbor v1.4.0](https://github.com/vmware/harbor/releases)

## 环境说明
在下面的步骤中，我们将在三台CentOS系统的物理机上部署具有三个节点的kubernetes1.6.0集群。

角色分配如下：

Master：10.8.8.8

Node：10.8.8.8、10.8.8.10、10.8.8.11

注意：10.8.8.8这台主机master和node复用。所有生成证书、执行kubectl命令的操作都在这台节点上执行。一旦node加入到kubernetes集群之后就不需要再登陆node节点了。

## 安装
### 安装前的准备
- 1.关闭所有节点的SELinux
 修改/etc/selinux/config文件中设置SELINUX=disabled ，然后重启服务器。
  使用命令setenforce 0

- 2.在node节点上安装docker `yum -y install docker`

- 3.准备harbor私有镜像仓库

参考：https://github.com/vmware/harbor

[docker](./docker.md)


## 1.[生成证书](./生成证书.md)
## 2.下载安装文件
```
mkdir /root/k8s
cd /root/k8s

wget https://dl.k8s.io/v1.10.0/kubernetes-server-linux-amd64.tar.gz

wget https://github.com/coreos/flannel/releases/download/v0.10.0/flannel-v0.10.0-linux-amd64.tar.gz

wget https://github.com/coreos/etcd/releases/download/v3.2.18/etcd-v3.2.18-linux-amd64.tar.gz
```

### master 10.8.8.8

```
vim ~/.bashrc
export PATH=/usr/local/bin:$PATH

#kubernetes
tar -xzvf kubernetes-server-linux-amd64.tar.gz
cp -r kubernetes/server/bin/{kube-apiserver,kube-controller-manager,kube-scheduler,kubectl} /usr/local/bin/
```

```
tar -xzf  kubernetes/kubernetes-src.tar.gz
```

### node 10.8.8.10 10.8.8.11
```
vim ~/.bashrc
export PATH=/usr/local/bin:$PATH

# 不确定 node 需要kubectl不，暂时不复制到node节点
cp -r kubernetes/server/bin/{kube-proxy,kubelet} /usr/local/bin/
```

### master和node
```
#etcd
tar -xzvf etcd-*-linux-amd64.tar.gz
mv etcd-*-linux-amd64/etcd* /usr/local/bin

#flanneld
tar -xzvf flannel-*-linux-amd64.tar.gz
mv flanneld mk-docker-opts.sh /usr/local/bin
``` 

## 3.创建 kubeconfig 文件

### 1.创建 TLS Bootstrapping Token
Token auth file

```
cd /etc/kubernetes/

export BOOTSTRAP_TOKEN=$(head -c 16 /dev/urandom | od -An -t x | tr -d ' ')

cat > token.csv <<EOF
${BOOTSTRAP_TOKEN},kubelet-bootstrap,10001,"system:kubelet-bootstrap"
EOF
```

### 2.创建 kubelet bootstrapping kubeconfig 文件

```
export KUBE_APISERVER="https://10.8.8.8:6443"

# 设置集群参数
kubectl config set-cluster kubernetes \
  --certificate-authority=/etc/kubernetes/ssl/ca.pem \
  --embed-certs=true \
  --server=${KUBE_APISERVER} \
  --kubeconfig=bootstrap.kubeconfig

# 设置客户端认证参数
kubectl config set-credentials kubelet-bootstrap \
  --token=${BOOTSTRAP_TOKEN} \
  --kubeconfig=bootstrap.kubeconfig

# 设置上下文参数
kubectl config set-context default \
  --cluster=kubernetes \
  --user=kubelet-bootstrap \
  --kubeconfig=bootstrap.kubeconfig

# 设置默认上下文
kubectl config use-context default --kubeconfig=bootstrap.kubeconfig
```

### 3.创建 kube-proxy kubeconfig 文件

```
export KUBE_APISERVER="https://10.8.8.8:6443"

# 设置集群参数
kubectl config set-cluster kubernetes \
  --certificate-authority=/etc/kubernetes/ssl/ca.pem \
  --embed-certs=true \
  --server=${KUBE_APISERVER} \
  --kubeconfig=kube-proxy.kubeconfig
# 设置客户端认证参数
kubectl config set-credentials kube-proxy \
  --client-certificate=/etc/kubernetes/ssl/kube-proxy.pem \
  --client-key=/etc/kubernetes/ssl/kube-proxy-key.pem \
  --embed-certs=true \
  --kubeconfig=kube-proxy.kubeconfig
# 设置上下文参数
kubectl config set-context default \
  --cluster=kubernetes \
  --user=kube-proxy \
  --kubeconfig=kube-proxy.kubeconfig
# 设置默认上下文
kubectl config use-context default --kubeconfig=kube-proxy.kubeconfig
```

### 4.分发 kubeconfig 文件
将两个 kubeconfig 文件分发到所有 Node 机器的 /etc/kubernetes/ 目录

```
cp bootstrap.kubeconfig kube-proxy.kubeconfig /etc/kubernetes/
```

### 5.创建 kubectl kubeconfig 文件

```
export KUBE_APISERVER="https://10.8.8.8:6443"

# 设置集群参数
kubectl config set-cluster kubernetes \
  --certificate-authority=/etc/kubernetes/ssl/ca.pem \
  --embed-certs=true \
  --server=${KUBE_APISERVER}
# 设置客户端认证参数
kubectl config set-credentials admin \
  --client-certificate=/etc/kubernetes/ssl/admin.pem \
  --embed-certs=true \
  --client-key=/etc/kubernetes/ssl/admin-key.pem
# 设置上下文参数
kubectl config set-context kubernetes \
  --cluster=kubernetes \
  --user=admin
# 设置默认上下文
kubectl config use-context kubernetes

注意：生成的 kubeconfig 被保存到 ~/.kube/config 文件；~/.kube/config文件拥有对该集群的最高权限，请妥善保管。
admin.pem 证书 OU 字段值为 system:masters，kube-apiserver 预定义的 RoleBinding cluster-admin 将 Group system:masters 与 Role cluster-admin 绑定，该 Role 授予了调用kube-apiserver 相关 API 的权限；

```


## 4.创建高可用 etcd 集群
```
mkdir /etc/etcd&&mkdir -p /var/lib/etcd
```

### 1.环境变量配置文件/etc/etcd/etcd.conf
这是10.8.8.8节点的配置，其他两个etcd节点只要将上面的IP地址改成相应节点的IP地址即可。ETCD_NAME换成对应节点的infra1/2/3

```
cat >/etc/etcd/etcd.conf<<EOF
# [member]
ETCD_NAME=infra1
ETCD_DATA_DIR="/var/lib/etcd"
ETCD_LISTEN_PEER_URLS="https://10.8.8.8:2380"
ETCD_LISTEN_CLIENT_URLS="https://10.8.8.8:2379"

#[cluster]
ETCD_INITIAL_ADVERTISE_PEER_URLS="https://10.8.8.8:2380"
ETCD_INITIAL_CLUSTER_TOKEN="etcd-cluster"
ETCD_ADVERTISE_CLIENT_URLS="https://10.8.8.8:2379"

# [security]
ETCD_CERT_FILE="/etc/kubernetes/ssl/kubernetes.pem"
ETCD_KEY_FILE="/etc/kubernetes/ssl/kubernetes-key.pem"
ETCD_CLIENT_CERT_AUTH="true"
ETCD_TRUSTED_CA_FILE="/etc/kubernetes/ssl/ca.pem"
ETCD_AUTO_TLS="true"
ETCD_PEER_CERT_FILE="/etc/kubernetes/ssl/kubernetes.pem"
ETCD_PEER_KEY_FILE="/etc/kubernetes/ssl/kubernetes-key.pem"
ETCD_PEER_CLIENT_CERT_AUTH="true"
ETCD_PEER_TRUSTED_CA_FILE="/etc/kubernetes/ssl/ca.pem"
ETCD_PEER_AUTO_TLS="true"
EOF
```



### 2.创建 etcd 的 systemd unit 文件 etcd.service

```
vim /usr/lib/systemd/system/etcd.service

[Unit]
Description=Etcd Server
After=network.target
After=network-online.target
Wants=network-online.target
Documentation=https://github.com/coreos

[Service]
Type=notify
WorkingDirectory=/var/lib/etcd/
EnvironmentFile=-/etc/etcd/etcd.conf
ExecStart=/usr/local/bin/etcd \
  --name ${ETCD_NAME} \
  --cert-file=/etc/kubernetes/ssl/kubernetes.pem \
  --key-file=/etc/kubernetes/ssl/kubernetes-key.pem \
  --peer-cert-file=/etc/kubernetes/ssl/kubernetes.pem \
  --peer-key-file=/etc/kubernetes/ssl/kubernetes-key.pem \
  --trusted-ca-file=/etc/kubernetes/ssl/ca.pem \
  --peer-trusted-ca-file=/etc/kubernetes/ssl/ca.pem \
  --initial-advertise-peer-urls ${ETCD_INITIAL_ADVERTISE_PEER_URLS} \
  --listen-peer-urls ${ETCD_LISTEN_PEER_URLS} \
  --listen-client-urls ${ETCD_LISTEN_CLIENT_URLS},http://127.0.0.1:2379 \
  --advertise-client-urls ${ETCD_ADVERTISE_CLIENT_URLS} \
  --initial-cluster-token ${ETCD_INITIAL_CLUSTER_TOKEN} \
  --initial-cluster infra1=https://10.8.8.8:2380,infra2=https://10.8.8.10:2380,infra3=https://10.8.8.11:2380 \
  --initial-cluster-state new \
  --data-dir=${ETCD_DATA_DIR}
Restart=on-failure
RestartSec=5
LimitNOFILE=65536

[Install]
WantedBy=multi-user.target
```

### 3.启动 etcd 服务

```
systemctl daemon-reload.service
systemctl enable etcd.service
systemctl start etcd.service
systemctl status etcd.service
```

### 4.验证服务

```
etcdctl \
  --ca-file=/etc/kubernetes/ssl/ca.pem \
  --cert-file=/etc/kubernetes/ssl/kubernetes.pem \
  --key-file=/etc/kubernetes/ssl/kubernetes-key.pem \
  cluster-health
```

## 5.部署master节点

### config

/etc/kubernetes/config

```
cat >/etc/kubernetes/config<<EOF
# kubernetes system config
#
# The following values are used to configure various aspects of all
# kubernetes services, including
#
#   kube-apiserver.service
#   kube-controller-manager.service
#   kube-scheduler.service
#   kubelet.service
#   kube-proxy.service
# logging to stderr means we get it in the systemd journal
KUBE_LOGTOSTDERR="--logtostderr=true"

# journal message level, 0 is debug
KUBE_LOG_LEVEL="--v=0"

# Should this cluster be allowed to run privileged docker containers
KUBE_ALLOW_PRIV="--allow-privileged=true"

# How the controller-manager, scheduler, and proxy find the apiserver
#KUBE_MASTER="--master=http://sz-pg-oam-docker-test-001.tendcloud.com:8080"
KUBE_MASTER="--master=http://10.8.8.8:8080"
EOF
```

/etc/kubernetes/apiserver

```
cat >/etc/kubernetes/apiserver<<EOF
## kubernetes system config
##
## The following values are used to configure the kube-apiserver
##
#
## The address on the local server to listen to.
KUBE_API_ADDRESS="--advertise-address=10.8.8.8 --bind-address=10.8.8.8 --insecure-bind-address=10.8.8.8"
#
## The port on the local server to listen on.
#KUBE_API_PORT="--port=8080"
#
## Port minions listen on
#KUBELET_PORT="--kubelet-port=10250"
#
## Comma separated list of nodes in the etcd cluster
KUBE_ETCD_SERVERS="--etcd-servers=https://10.8.8.8:2379"
#
## Address range to use for services
KUBE_SERVICE_ADDRESSES="--service-cluster-ip-range=10.254.0.0/16"
#
## default admission control policies
KUBE_ADMISSION_CONTROL="--admission-control=ServiceAccount,NamespaceLifecycle,NamespaceExists,LimitRanger,ResourceQuota"
#
## Add your own!
KUBE_API_ARGS="--authorization-mode=RBAC --runtime-config=rbac.authorization.k8s.io/v1beta1 --kubelet-https=true --enable-bootstrap-token-auth --token-auth-file=/etc/kubernetes/token.csv --service-node-port-range=30000-32767 --tls-cert-file=/etc/kubernetes/ssl/kubernetes.pem --tls-private-key-file=/etc/kubernetes/ssl/kubernetes-key.pem --client-ca-file=/etc/kubernetes/ssl/ca.pem --service-account-key-file=/etc/kubernetes/ssl/ca-key.pem --etcd-cafile=/etc/kubernetes/ssl/ca.pem --etcd-certfile=/etc/kubernetes/ssl/kubernetes.pem --etcd-keyfile=/etc/kubernetes/ssl/kubernetes-key.pem --enable-swagger-ui=true --apiserver-count=3 --audit-log-maxage=30 --audit-log-maxbackup=3 --audit-log-maxsize=100 --audit-log-path=/var/lib/audit.log --event-ttl=1h"
EOF
```

/etc/kubernetes/controller-manager

```
cat >/etc/kubernetes/controller-manager<<EOF
# The following values are used to configure the kubernetes controller-manager

# defaults from config and apiserver should be adequate

# Add your own!
KUBE_CONTROLLER_MANAGER_ARGS="--address=127.0.0.1 --service-cluster-ip-range=10.254.0.0/16 --cluster-name=kubernetes --cluster-signing-cert-file=/etc/kubernetes/ssl/ca.pem --cluster-signing-key-file=/etc/kubernetes/ssl/ca-key.pem  --service-account-private-key-file=/etc/kubernetes/ssl/ca-key.pem --root-ca-file=/etc/kubernetes/ssl/ca.pem --leader-elect=true"
EOF
```

/etc/kubernetes/scheduler

```
cat >/etc/kubernetes/scheduler<<EOF
# kubernetes scheduler config

# default config should be adequate

# Add your own!
KUBE_SCHEDULER_ARGS="--leader-elect=true --address=127.0.0.1"
EOF
```

### serivce配置文件

创建 kube-apiserver的service配置文件

```
vim /usr/lib/systemd/system/kube-apiserver.service

[Unit]
Description=Kubernetes API Service
Documentation=https://github.com/GoogleCloudPlatform/kubernetes
After=network.target
After=etcd.service

[Service]
EnvironmentFile=-/etc/kubernetes/config
EnvironmentFile=-/etc/kubernetes/apiserver
ExecStart=/usr/local/bin/kube-apiserver \
        $KUBE_LOGTOSTDERR \
        $KUBE_LOG_LEVEL \
        $KUBE_ETCD_SERVERS \
        $KUBE_API_ADDRESS \
        $KUBE_API_PORT \
        $KUBELET_PORT \
        $KUBE_ALLOW_PRIV \
        $KUBE_SERVICE_ADDRESSES \
        $KUBE_ADMISSION_CONTROL \
        $KUBE_API_ARGS
Restart=on-failure
Type=notify
LimitNOFILE=65536

[Install]
WantedBy=multi-user.target
```

创建 kube-controller-manager的serivce配置文件

```
vim /usr/lib/systemd/system/kube-controller-manager.service

[Unit]
Description=Kubernetes Controller Manager
Documentation=https://github.com/GoogleCloudPlatform/kubernetes

[Service]
EnvironmentFile=-/etc/kubernetes/config
EnvironmentFile=-/etc/kubernetes/controller-manager
ExecStart=/usr/local/bin/kube-controller-manager \
        $KUBE_LOGTOSTDERR \
        $KUBE_LOG_LEVEL \
        $KUBE_MASTER \
        $KUBE_CONTROLLER_MANAGER_ARGS
Restart=on-failure
LimitNOFILE=65536

[Install]
WantedBy=multi-user.target
```

创建 kube-scheduler的serivce配置文件

```
vim /usr/lib/systemd/system/kube-scheduler.service

[Unit]
Description=Kubernetes Scheduler Plugin
Documentation=https://github.com/GoogleCloudPlatform/kubernetes

[Service]
EnvironmentFile=-/etc/kubernetes/config
EnvironmentFile=-/etc/kubernetes/scheduler
ExecStart=/usr/local/bin/kube-scheduler \
            $KUBE_LOGTOSTDERR \
            $KUBE_LOG_LEVEL \
            $KUBE_MASTER \
            $KUBE_SCHEDULER_ARGS
Restart=on-failure
LimitNOFILE=65536

[Install]
WantedBy=multi-user.target
```

### 启动

`systemctl daemon-reload`

```bash
for service in kube-apiserver kube-controller-manager kube-scheduler; do
	systemctl enable $service
	systemctl start $service
	systemctl status $service
done
```

### 验证 master 节点功能
我们启动每个组件后可以通过执行命令kubectl get componentstatuses，来查看各个组件的状态;

`kubectl get componentstatuses`

## 6.安装flannel网络插件
/etc/sysconfig/flanneld配置文件

```
cat >/etc/sysconfig/flanneld<<EOF
# Flanneld configuration options  

# etcd url location.  Point this to the server where etcd runs
FLANNEL_ETCD_ENDPOINTS="https://10.8.8.8:2379"

# etcd config key.  This is the configuration key that flannel queries
# For address range assignment
FLANNEL_ETCD_PREFIX="/kube-centos/network"

# Any additional options that you want to pass
FLANNEL_OPTIONS="-etcd-cafile=/etc/kubernetes/ssl/ca.pem -etcd-certfile=/etc/kubernetes/ssl/kubernetes.pem -etcd-keyfile=/etc/kubernetes/ssl/kubernetes-key.pem"
EOF
```

service配置文件/usr/lib/systemd/system/flanneld.service

```
vim /usr/lib/systemd/system/flanneld.service

[Unit]
Description=Flanneld overlay address etcd agent
After=network.target
After=network-online.target
Wants=network-online.target
After=etcd.service
Before=docker.service

[Service]
Type=notify
EnvironmentFile=/etc/sysconfig/flanneld
EnvironmentFile=-/etc/sysconfig/docker-network
ExecStart=/usr/local/bin/flanneld \
  -etcd-endpoints=${FLANNEL_ETCD_ENDPOINTS} \
  -etcd-prefix=${FLANNEL_ETCD_PREFIX} \
  $FLANNEL_OPTIONS
ExecStartPost=/usr/local/bin/mk-docker-opts.sh -k DOCKER_NETWORK_OPTIONS -d /run/flannel/docker
Restart=on-failure

[Install]
WantedBy=multi-user.target
RequiredBy=docker.service
```

### 3.在etcd中创建网络配置
```
#新建目录 /kube-centos/network
etcdctl --endpoints=https://10.8.8.8:2379 \
  --ca-file=/etc/kubernetes/ssl/ca.pem \
  --cert-file=/etc/kubernetes/ssl/kubernetes.pem \
  --key-file=/etc/kubernetes/ssl/kubernetes-key.pem \
  mkdir /kube-centos/network
  
#指定IP段
etcdctl --endpoints=https://10.8.8.8:2379 \
  --ca-file=/etc/kubernetes/ssl/ca.pem \
  --cert-file=/etc/kubernetes/ssl/kubernetes.pem \
  --key-file=/etc/kubernetes/ssl/kubernetes-key.pem \
  mk /kube-centos/network/config '{"Network":"172.30.0.0/16","SubnetLen":24,"Backend":{"Type":"vxlan"}}'
  
#查看数据
etcdctl --endpoints=https://10.8.8.8:2379 \
  --ca-file=/etc/kubernetes/ssl/ca.pem \
  --cert-file=/etc/kubernetes/ssl/kubernetes.pem \
  --key-file=/etc/kubernetes/ssl/kubernetes-key.pem \
  get /kube-centos/network/config 

#查看目录  
etcdctl --endpoints=${ETCD_ENDPOINTS} \
  --ca-file=/etc/kubernetes/ssl/ca.pem \
  --cert-file=/etc/kubernetes/ssl/kubernetes.pem \
  --key-file=/etc/kubernetes/ssl/kubernetes-key.pem \
  ls /kube-centos/network/subnets
```  

启动flannel

```
systemctl daemon-reload
systemctl enable flanneld.service
systemctl start flanneld.service
systemctl status flanneld.service
```

使用systemctl命令启动flanneld后，会自动执行./mk-docker-opts.sh -i生成如下两个文件环境变量文件：

/run/flannel/subnet.env
/run/docker_opts.env

配置docker

二进制方式安装的flannel

修改docker的配置文件/usr/lib/systemd/system/docker.service，增加如下几条环境变量配置：

```
EnvironmentFile=-/run/docker_opts.env
EnvironmentFile=-/run/flannel/subnet.env
```

## 7.部署Node节点
```
mkdir /var/lib/kubelet
```

- 1.--kubeconfig=/etc/kubernetes/kubelet.kubeconfig中指定的kubelet.kubeconfig文件在第一次启动kubelet之前并不存在，请看下文，当通过CSR请求后会自动生成kubelet.kubeconfig文件，如果你的节点上已经生成了~/.kube/config文件，你可以将该文件拷贝到该路径下，并重命名为kubelet.kubeconfig，所有node节点可以共用同一个kubelet.kubeconfig文件，这样新添加的节点就不需要再创建CSR请求就能自动添加到kubernetes集群中。同样，在任意能够访问到kubernetes集群的主机上使用kubectl --kubeconfig命令操作集群时，只要使用~/.kube/config文件就可以通过权限认证，因为这里面已经有认证信息并认为你是admin用户，对集群拥有所有权限。

```
cp ~/.kube/config /etc/kubernetes/kubelet.kubeconfig
```

- 1.修改/etc/fstab将，swap系统注释掉。
- 2.kubelet 启动时向 kube-apiserver 发送 TLS bootstrapping 请求，需要先将 bootstrap token 文件中的 kubelet-bootstrap 用户赋予 system:node-bootstrapper cluster 角色(role)， 然后 kubelet 才能有权限创建认证请求(certificate signing requests)：

```
cd /etc/kubernetes

kubectl create clusterrolebinding kubelet-bootstrap \
  --clusterrole=system:node-bootstrapper \
  --user=kubelet-bootstrap
```

/etc/kubernetes/kubelet

```
cat >/etc/kubernetes/kubelet<<EOF
###
## kubernetes kubelet (minion) config
#
## The address for the info server to serve on (set to 0.0.0.0 or "" for all interfaces)
KUBELET_ADDRESS="--address=10.8.8.8"
#
## The port for the info server to serve on
#KUBELET_PORT="--port=10250"
#
## You may leave this blank to use the actual hostname
KUBELET_HOSTNAME="--hostname-override=10.8.8.8"
#
## location of the api-server
## COMMENT THIS ON KUBERNETES 1.8+
#KUBELET_API_SERVER="--api-servers=http://10.8.8.8:8080"
#
## pod infrastructure container
KUBELET_POD_INFRA_CONTAINER="--pod-infra-container-image=index.tenxcloud.com/jimmy/pod-infrastructure:rhel7"
#
## Add your own!
KUBELET_ARGS="--runtime-cgroups=/systemd/system.slice --kubelet-cgroups=/systemd/system.slice --cgroup-driver=systemd --cluster-dns=10.254.0.2 --bootstrap-kubeconfig=/etc/kubernetes/bootstrap.kubeconfig --kubeconfig=/etc/kubernetes/kubelet.kubeconfig --cert-dir=/etc/kubernetes/ssl --cluster-domain=cluster.local --hairpin-mode promiscuous-bridge --serialize-image-pulls=false"
# --require-kubeconfig 
EOF
```

[Remove --require-kubeconfig and the default kubeconfig path in v1.10](https://github.com/kubernetes/kubernetes/issues/41161)

[kubelet Error: unknown flag: --api-servers](https://github.com/kubernetes/contrib/issues/2830)

创建kubelet的service配置文件

```
vim /usr/lib/systemd/system/kubelet.service

[Unit]
Description=Kubernetes Kubelet Server
Documentation=https://github.com/GoogleCloudPlatform/kubernetes
After=docker.service
Requires=docker.service

[Service]
WorkingDirectory=/var/lib/kubelet
EnvironmentFile=-/etc/kubernetes/config
EnvironmentFile=-/etc/kubernetes/kubelet
ExecStart=/usr/local/bin/kubelet \
            $KUBE_LOGTOSTDERR \
            $KUBE_LOG_LEVEL \
            $KUBELET_API_SERVER \
            $KUBELET_ADDRESS \
            $KUBELET_PORT \
            $KUBELET_HOSTNAME \
            $KUBE_ALLOW_PRIV \
            $KUBELET_POD_INFRA_CONTAINER \
            $KUBELET_ARGS
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

启动kublet

```
systemctl daemon-reload
systemctl enable kubelet.service
systemctl start kubelet.service
systemctl status kubelet.service
```

### 2.配置 kube-proxy
/etc/kubernetes/proxy

```
cat >/etc/kubernetes/proxy<<EOF
###
# kubernetes proxy config

# default config should be adequate

# Add your own!
KUBE_PROXY_ARGS="--bind-address=10.8.8.8 --hostname-override=10.8.8.8 --kubeconfig=/etc/kubernetes/kube-proxy.kubeconfig --cluster-cidr=10.254.0.0/16"
EOF
```

/usr/lib/systemd/system/kube-proxy.service

```
vim /usr/lib/systemd/system/kube-proxy.service

[Unit]
Description=Kubernetes Kube-Proxy Server
Documentation=https://github.com/GoogleCloudPlatform/kubernetes
After=network.target

[Service]
EnvironmentFile=-/etc/kubernetes/config
EnvironmentFile=-/etc/kubernetes/proxy
ExecStart=/usr/local/bin/kube-proxy \
        $KUBE_LOGTOSTDERR \
        $KUBE_LOG_LEVEL \
        $KUBE_MASTER \
        $KUBE_PROXY_ARGS
Restart=on-failure
LimitNOFILE=65536

[Install]
WantedBy=multi-user.target
```

启动 kube-proxy

```
systemctl daemon-reload
systemctl enable kube-proxy
systemctl start kube-proxy
systemctl status kube-proxy
```

由于采用了 TLS Bootstrapping，所以 kubelet 启动后不会立即加入集群，而是进行证书申请，此时只需要在 master 允许其证书申请即可。

```
# 查看 csr
➜  kubectl get csr
NAME        AGE       REQUESTOR           CONDITION
csr-l9d25   2m        kubelet-bootstrap   Pending

# 签发证书
➜  kubectl certificate approve csr-l9d25
certificatesigningrequest "csr-l9d25" approved

# 查看 node
kubectl get node
```
```
docker pull jimmysong/pause-amd64:3.0

docker pull registry.cn-hangzhou.aliyuncs.com/sunyuki/pod-infrastructure:latest

docker pull nginx:1.13.11

```

### 4.验证测试
我们创建一个nginx的service试一下集群是否可用。

```
kubectl run nginx --replicas=2 --labels="run=nginx-load-balancer" --image=nginx:1.13.11  --port=80

kubectl expose deployment nginx --type=NodePort --name=nginx-service

kubectl describe svc nginx-service
```

删除

```
kubectl delete svc nginx
kubectl delete deploy nginx
```