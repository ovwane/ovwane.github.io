---
title: Kubernetes安装配置
date: 2017-08-23 08:51:00
categories:
- 技术
- Linux
tags:
- CentOS
- Kubernetes
- k8s
- Docker
---

>Kubernetes 1.7.3
Kubernetes安装配置

## 环境说明

```
k8s-master-1: 10.6.0.140
k8s-master-2: 10.6.0.187
k8s-master-3: 10.6.0.188
```

```
node1.ovwane.com 192.168.3.221
node2.ovwane.com 192.168.3.222
node3.ovwane.com 192.168.3.223
```
#### 软件版本
```
etcd-3.1.9
Docker version 1.12.6
```

## 创建 验证
### 安装 cfssl
```
wget -O /usr/local/bin/cfssl https://pkg.cfssl.org/R1.2/cfssl_linux-amd64

wget -O /usr/local/bin/cfssljson https://pkg.cfssl.org/R1.2/cfssljson_linux-amd64

wget -O /usr/local/bin/cfssl-certinfo https://pkg.cfssl.org/R1.2/cfssl-certinfo_linux-amd64

# 添加执行的权限
chmod +x /usr/local/bin/{cfssl,cfssljson,cfssl-certinfo}
```

### 创建 CA 证书配置
```
mkdir /root/ssl
cd /root/ssl
cfssl print-defaults config > config.json
cfssl print-defaults csr > csr.json
```

#### config.json
```
cat >config.json<<EOF
{
  "signing": {
    "default": {
      "expiry": "87600h"
    },
    "profiles": {
      "kubernetes": {
        "usages": [
            "signing",
            "key encipherment",
            "server auth",
            "client auth"
        ],
        "expiry": "87600h"
      }
    }
  }
}
EOF
```

#### csr.json
```
cat >csr.json<<EOF
{
  "CN": "kubernetes",
  "key": {
    "algo": "rsa",
    "size": 2048
  },
  "names": [
    {
      "C": "CN",
      "ST": "ShenZhen",
      "L": "ShenZhen",
      "O": "k8s",
      "OU": "System"
    }
  ]
}
EOF
```

### 生成 CA 证书和私钥
```
cfssl gencert -initca csr.json | cfssljson -bare ca
```
```
[root@node01 ssl]# ls
ca.csr  ca-key.pem  ca.pem  config.json  csr.json
```

## 分发证书
```
# 创建证书目录
mkdir -p /etc/kubernetes/ssl

# 拷贝所有文件到目录下
cp * /etc/kubernetes/ssl

# 这里要将文件拷贝到所有的k8s 机器上

scp * 192.168.3.222:/etc/kubernetes/ssl/

scp * 192.168.3.223:/etc/kubernetes/ssl/
```

## etcd 集群
### 安装 etcd
```
yum -y install etcd
```

### 创建 etcd 证书
```
cat >etcd-csr.json<<EOF
{
  "CN": "etcd",
  "hosts": [
    "127.0.0.1",
    "192.168.3.221",
    "192.168.3.222",
    "192.168.3.223"
  ],
  "key": {
    "algo": "rsa",
    "size": 2048
  },
  "names": [
    {
      "C": "CN",
      "ST": "ShenZhen",
      "L": "ShenZhen",
      "O": "k8s",
      "OU": "System"
    }
  ]
}
EOF
```

### 生成 etcd   密钥
```
cfssl gencert -ca=/root/ssl/ca.pem \
  -ca-key=/root/ssl/ca-key.pem \
  -config=/root/ssl/config.json \
  -profile=kubernetes etcd-csr.json|cfssljson -bare etcd
```

#### 查看生成
```
[root@node01 ssl]# ls etcd*
etcd.csr  etcd-csr.json  etcd-key.pem  etcd.pem
```

### 拷贝到etcd服务器
```
# etcd-1 
cp etcd*.pem /etc/kubernetes/ssl/

# etcd-2
scp etcd* 192.168.3.222:/etc/kubernetes/ssl/

# etcd-3
scp etcd* 192.168.3.223:/etc/kubernetes/ssl/

# 如果 etcd 非 root 用户，读取证书会提示没权限

chmod 644 /etc/kubernetes/ssl/etcd-key.pem
```

### 修改 etcd 配置
修改 etcd 启动文件 /usr/lib/systemd/system/etcd.service

#### etcd-1
```
cat >/usr/lib/systemd/system/etcd.service<<EOF
[Unit]
Description=Etcd Server
After=network.target
After=network-online.target
Wants=network-online.target

[Service]
Type=notify
WorkingDirectory=/var/lib/etcd/
User=etcd
# set GOMAXPROCS to number of processors
ExecStart=/usr/bin/etcd \
  --name=etcd1 \
  --cert-file=/etc/kubernetes/ssl/etcd.pem \
  --key-file=/etc/kubernetes/ssl/etcd-key.pem \
  --peer-cert-file=/etc/kubernetes/ssl/etcd.pem \
  --peer-key-file=/etc/kubernetes/ssl/etcd-key.pem \
  --trusted-ca-file=/etc/kubernetes/ssl/ca.pem \
  --peer-trusted-ca-file=/etc/kubernetes/ssl/ca.pem \
  --initial-advertise-peer-urls=https://192.168.3.221:2380 \
  --listen-peer-urls=https://192.168.3.221:2380 \
  --listen-client-urls=https://192.168.3.221:2379,http://127.0.0.1:2379 \
  --advertise-client-urls=https://192.168.3.221:2379 \
  --initial-cluster-token=k8s-etcd-cluster \
  --initial-cluster=etcd1=https://192.168.3.221:2380,etcd2=https://192.168.3.222:2380,etcd3=https://192.168.3.223:2380 \
  --initial-cluster-state=new \
  --data-dir=/var/lib/etcd
Restart=on-failure
RestartSec=5
LimitNOFILE=65536

[Install]
WantedBy=multi-user.target
EOF
```

#### etcd-2
```
cat >/usr/lib/systemd/system/etcd.service<<EOF
[Unit]
Description=Etcd Server
After=network.target
After=network-online.target
Wants=network-online.target

[Service]
Type=notify
WorkingDirectory=/var/lib/etcd/
User=etcd
# set GOMAXPROCS to number of processors
ExecStart=/usr/bin/etcd \
  --name=etcd2 \
  --cert-file=/etc/kubernetes/ssl/etcd.pem \
  --key-file=/etc/kubernetes/ssl/etcd-key.pem \
  --peer-cert-file=/etc/kubernetes/ssl/etcd.pem \
  --peer-key-file=/etc/kubernetes/ssl/etcd-key.pem \
  --trusted-ca-file=/etc/kubernetes/ssl/ca.pem \
  --peer-trusted-ca-file=/etc/kubernetes/ssl/ca.pem \
  --initial-advertise-peer-urls=https://192.168.3.222:2380 \
  --listen-peer-urls=https://192.168.3.222:2380 \
  --listen-client-urls=https://192.168.3.222:2379,http://127.0.0.1:2379 \
  --advertise-client-urls=https://192.168.3.222:2379 \
  --initial-cluster-token=k8s-etcd-cluster \
  --initial-cluster=etcd1=https://192.168.3.221:2380,etcd2=https://192.168.3.222:2380,etcd3=https://192.168.3.223:2380 \
  --initial-cluster-state=new \
  --data-dir=/var/lib/etcd
Restart=on-failure
RestartSec=5
LimitNOFILE=65536

[Install]
WantedBy=multi-user.target
EOF
```

#### etcd-3
```
cat >/usr/lib/systemd/system/etcd.service<<EOF
[Unit]
Description=Etcd Server
After=network.target
After=network-online.target
Wants=network-online.target

[Service]
Type=notify
WorkingDirectory=/var/lib/etcd/
User=etcd
# set GOMAXPROCS to number of processors
ExecStart=/usr/bin/etcd \
  --name=etcd3 \
  --cert-file=/etc/kubernetes/ssl/etcd.pem \
  --key-file=/etc/kubernetes/ssl/etcd-key.pem \
  --peer-cert-file=/etc/kubernetes/ssl/etcd.pem \
  --peer-key-file=/etc/kubernetes/ssl/etcd-key.pem \
  --trusted-ca-file=/etc/kubernetes/ssl/ca.pem \
  --peer-trusted-ca-file=/etc/kubernetes/ssl/ca.pem \
  --initial-advertise-peer-urls=https://192.168.3.223:2380 \
  --listen-peer-urls=https://192.168.3.223:2380 \
  --listen-client-urls=https://192.168.3.223:2379,http://127.0.0.1:2379 \
  --advertise-client-urls=https://192.168.3.223:2379 \
  --initial-cluster-token=k8s-etcd-cluster \
  --initial-cluster=etcd1=https://192.168.3.221:2380,etcd2=https://192.168.3.222:2380,etcd3=https://192.168.3.223:2380 \
  --initial-cluster-state=new \
  --data-dir=/var/lib/etcd
Restart=on-failure
RestartSec=5
LimitNOFILE=65536

[Install]
WantedBy=multi-user.target
EOF
```

### 启动 etcd
systemctl enable etcd&&systemctl start etcd

systemctl status etcd

### 验证 etcd 集群状态
#### 查看 etcd 集群状态
```
etcdctl --endpoints=https://192.168.3.221:2379 \
        --cert-file=/etc/kubernetes/ssl/etcd.pem \
        --ca-file=/etc/kubernetes/ssl/ca.pem \
        --key-file=/etc/kubernetes/ssl/etcd-key.pem \
        cluster-health
```

#### 查看etcd集群成员
```
etcdctl --endpoints=https://192.168.3.221:2379 \
        --cert-file=/etc/kubernetes/ssl/etcd.pem \
        --ca-file=/etc/kubernetes/ssl/ca.pem \
        --key-file=/etc/kubernetes/ssl/etcd-key.pem \
        member list
```

## 安装 kubectl 工具
### Master 端
#### 安装 kubectl
```
wget https://dl.k8s.io/v1.7.3/kubernetes-client-linux-amd64.tar.gz

tar -xzvf kubernetes-server-v1.7.3-linux-amd64.tar.gz

cp kubernetes/server/bin/* /usr/local/bin/

chmod a+x /usr/local/bin/kube*
```
#### 验证安装
```
kubectl version

[root@node01 ~]# kubectl version
Client Version: version.Info{Major:"1", Minor:"7", GitVersion:"v1.7.3", GitCommit:"2c2fe6e8278a5db2d15a013987b53968c743f2a1", GitTreeState:"clean", BuildDate:"2017-08-03T07:00:21Z", GoVersion:"go1.8.3", Compiler:"gc", Platform:"linux/amd64"}
```

### 创建 admin 证书
cd /root/ssl

#### admin-csr.json
```
cat >admin-csr.json<<EOF
{
  "CN": "admin",
  "hosts": [],
  "key": {
    "algo": "rsa",
    "size": 2048
  },
  "names": [
    {
      "C": "CN",
      "ST": "ShenZhen",
      "L": "ShenZhen",
      "O": "system:masters",
      "OU": "System"
    }
  ]
}
EOF
```

#### 生成 admin 证书和私钥
```
cfssl gencert -ca=/etc/kubernetes/ssl/ca.pem \
  -ca-key=/etc/kubernetes/ssl/ca-key.pem \
  -config=/etc/kubernetes/ssl/config.json \
  -profile=kubernetes admin-csr.json |cfssljson -bare admin
```

#### 查看生成
```
[root@node01 ssl]# ls admin*
admin.csr  admin-csr.json  admin-key.pem  admin.pem
```

#### 分发
```
cp admin*.pem /etc/kubernetes/ssl/

scp admin*.pem 192.168.3.222:/etc/kubernetes/ssl/

scp admin*.pem 192.168.3.223:/etc/kubernetes/ssl/
```

### 配置 kubectl kubeconfig 文件
```
# 配置 kubernetes 集群
kubectl config set-cluster kubernetes \
  --certificate-authority=/etc/kubernetes/ssl/ca.pem \
  --embed-certs=true \
  --server=https://192.168.3.221:6443

# 配置 客户端认证
kubectl config set-credentials admin \
  --client-certificate=/etc/kubernetes/ssl/admin.pem \
  --embed-certs=true \
  --client-key=/etc/kubernetes/ssl/admin-key.pem

kubectl config set-context kubernetes \
  --cluster=kubernetes \
  --user=admin

kubectl config use-context kubernetes
```

#### kubectl config 文件
```
# kubeconfig 文件在 如下:

/root/.kube
```

### 部署 Kubernetes Master 节点
Master 需要部署 kube-apiserver , kube-scheduler , kube-controller-manager 这三个组件。

#### 创建 kubernetes 证书
```
cat >kubernetes-csr.json<<EOF
{
  "CN": "kubernetes",
  "hosts": [
    "127.0.0.1",
    "192.168.3.221",
    "192.168.3.222",
    "192.168.3.223",
    "10.254.0.1",
    "kubernetes",
    "kubernetes.default",
    "kubernetes.default.svc",
    "kubernetes.default.svc.cluster",
    "kubernetes.default.svc.cluster.local"
  ],
  "key": {
    "algo": "rsa",
    "size": 2048
  },
  "names": [
    {
      "C": "CN",
      "ST": "ShenZhen",
      "L": "ShenZhen",
      "O": "k8s",
      "OU": "System"
    }
  ]
}
EOF
```

#### 生成 kubernetes 证书和私钥
```
cfssl gencert -ca=/etc/kubernetes/ssl/ca.pem \
  -ca-key=/etc/kubernetes/ssl/ca-key.pem \
  -config=/etc/kubernetes/ssl/config.json \
  -profile=kubernetes kubernetes-csr.json |cfssljson -bare kubernetes
```

#### 查看生成
```
[root@node01 ssl]# ls kubernetes*
kubernetes.csr       kubernetes-key.pem
kubernetes-csr.json  kubernetes.pem
```

#### 分发
```
cp -r kubernetes* /etc/kubernetes/ssl/

scp -r kubernetes* 192.168.3.222:/etc/kubernetes/ssl/

scp -r kubernetes* 192.168.3.223:/etc/kubernetes/ssl/
```

### 配置 kube-apiserver
kubelet 首次启动时向 kube-apiserver 发送 TLS Bootstrapping 请求，kube-apiserver 验证 kubelet 请求中的 token 是否与它配置的 token 一致，如果一致则自动为 kubelet生成证书和秘钥。

#### 生成 token
```
# 生成 token
[root@node01 ssl]# head -c 16 /dev/urandom | od -An -t x | tr -d ' '
3f11d8b3c0bd83c61794652230c6df9a

# 创建 token.csv 文件
cd /root/ssl

vi token.csv

3f11d8b3c0bd83c61794652230c6df9a,kubelet-bootstrap,10001,"system:kubelet-bootstrap"

# 分发
cp token.csv /etc/kubernetes/

scp token.csv 192.168.3.222:/etc/kubernetes/

scp token.csv 192.168.3.223:/etc/kubernetes/
```

#### 创建 kube-apiserver.service 文件
```
cat >/etc/systemd/system/kube-apiserver.service<<EOF
[Unit]
Description=Kubernetes API Server
Documentation=https://github.com/GoogleCloudPlatform/kubernetes
After=network.target

[Service]
User=root
ExecStart=/usr/local/bin/kube-apiserver \
  --admission-control=NamespaceLifecycle,LimitRanger,ServiceAccount,DefaultStorageClass,ResourceQuota \
  --advertise-address=192.168.3.221 \
  --allow-privileged=true \
  --apiserver-count=3 \
  --audit-log-maxage=30 \
  --audit-log-maxbackup=3 \
  --audit-log-maxsize=100 \
  --audit-log-path=/var/lib/audit.log \
  --authorization-mode=RBAC \
  --bind-address=192.168.3.221 \
  --client-ca-file=/etc/kubernetes/ssl/ca.pem \
  --enable-swagger-ui=true \
  --etcd-cafile=/etc/kubernetes/ssl/ca.pem \
  --etcd-certfile=/etc/kubernetes/ssl/etcd.pem \
  --etcd-keyfile=/etc/kubernetes/ssl/etcd-key.pem \
  --etcd-servers=https://192.168.3.221:2379,https://192.168.3.222:2379,https://192.168.3.223:2379 \
  --event-ttl=1h \
  --kubelet-https=true \
  --insecure-bind-address=192.168.3.221 \
  --runtime-config=rbac.authorization.k8s.io/v1alpha1 \
  --service-account-key-file=/etc/kubernetes/ssl/ca-key.pem \
  --service-cluster-ip-range=10.254.0.0/16 \
  --service-node-port-range=30000-32000 \
  --tls-cert-file=/etc/kubernetes/ssl/kubernetes.pem \
  --tls-private-key-file=/etc/kubernetes/ssl/kubernetes-key.pem \
  --experimental-bootstrap-token-auth \
  --token-auth-file=/etc/kubernetes/token.csv \
  --v=2
Restart=on-failure
RestartSec=5
Type=notify
LimitNOFILE=65536

[Install]
WantedBy=multi-user.target
EOF
```

#### 启动 kube-apiserver
```
systemctl daemon-reload
systemctl enable kube-apiserver&&systemctl start kube-apiserver
systemctl status kube-apiserver
```

### 配置 kube-controller-manager
```
cat >/etc/systemd/system/kube-controller-manager.service<<EOF
[Unit]
Description=Kubernetes Controller Manager
Documentation=https://github.com/GoogleCloudPlatform/kubernetes

[Service]
ExecStart=/usr/local/bin/kube-controller-manager \
  --address=127.0.0.1 \
  --master=http://192.168.3.221:8080 \
  --allocate-node-cidrs=true \
  --service-cluster-ip-range=10.254.0.0/16 \
  --cluster-cidr=10.233.0.0/16 \
  --cluster-name=kubernetes \
  --cluster-signing-cert-file=/etc/kubernetes/ssl/ca.pem \
  --cluster-signing-key-file=/etc/kubernetes/ssl/ca-key.pem \
  --service-account-private-key-file=/etc/kubernetes/ssl/ca-key.pem \
  --root-ca-file=/etc/kubernetes/ssl/ca.pem \
  --leader-elect=true \
  --v=2
Restart=on-failure
RestartSec=5

[Install]
WantedBy=multi-user.target
EOF
```

#### 启动 kube-controller-manager
```
systemctl daemon-reload
systemctl enable kube-controller-manager&&systemctl start kube-controller-manager
systemctl status kube-controller-manager
```

### 配置 kube-scheduler
```
cat >/etc/systemd/system/kube-scheduler.service<<EOF
[Unit]
Description=Kubernetes Scheduler
Documentation=https://github.com/GoogleCloudPlatform/kubernetes

[Service]
ExecStart=/usr/local/bin/kube-scheduler \
  --address=127.0.0.1 \
  --master=http://192.168.3.221:8080 \
  --leader-elect=true \
  --v=2
Restart=on-failure
RestartSec=5

[Install]
WantedBy=multi-user.target
EOF
```

#### 启动 kube-scheduler
```
systemctl daemon-reload
systemctl enable kube-scheduler&&systemctl start kube-scheduler
systemctl status kube-scheduler
```

### 验证 Master 节点
```
kubectl get componentstatuses
```
```
[root@node01 ssl]# kubectl get componentstatuses
NAME                 STATUS    MESSAGE              ERROR
scheduler            Healthy   ok
controller-manager   Healthy   ok
etcd-1               Healthy   {"health": "true"}
etcd-2               Healthy   {"health": "true"}
etcd-0               Healthy   {"health": "true"}
```

## 部署 Master Node 部分
Node 部分 需要部署的组件有 docker calico kubectl kubelet kube-proxy 这几个组件。

### 配置 kubelet
kubelet 启动时向 kube-apiserver 发送 TLS bootstrapping 请求，需要先将 bootstrap token 文件中的 kubelet-bootstrap 用户赋予 system:node-bootstrapper 角色，然后 kubelet 才有权限创建认证请求(certificatesigningrequests)。

```
# 先创建认证请求
# user 为 master 中 token.csv 文件里配置的用户
# 只需创建一次就可以

kubectl create clusterrolebinding kubelet-bootstrap --clusterrole=system:node-bootstrapper --user=kubelet-bootstrap
```

### 创建 kubelet kubeconfig 文件
```
cd /etc/kubernetes/

# 配置集群

kubectl config set-cluster kubernetes \
  --certificate-authority=/etc/kubernetes/ssl/ca.pem \
  --embed-certs=true \
  --server=https://192.168.3.221:6443 \
  --kubeconfig=bootstrap.kubeconfig

# 配置客户端认证

kubectl config set-credentials kubelet-bootstrap \
  --token=3f11d8b3c0bd83c61794652230c6df9a \
  --kubeconfig=bootstrap.kubeconfig


# 配置关联

kubectl config set-context default \
  --cluster=kubernetes \
  --user=kubelet-bootstrap \
  --kubeconfig=bootstrap.kubeconfig
  
  
# 配置默认关联
kubectl config use-context default --kubeconfig=bootstrap.kubeconfig
```

### 创建 kubelet.service 文件
```
# 创建 kubelet 目录
> 配置为 node 本机 IP

mkdir /var/lib/kubelet
/etc/systemd/system/kubelet.service
```
```
cat >/usr/lib/systemd/system/kubelet.service<<EOF
[Unit]
Description=Kubernetes Kubelet
Documentation=https://github.com/GoogleCloudPlatform/kubernetes
After=docker.service
Requires=docker.service

[Service]
WorkingDirectory=/var/lib/kubelet
ExecStart=/usr/local/bin/kubelet \
  --address=192.168.3.221 \
  --hostname-override=192.168.3.221 \
  --pod-infra-container-image=jicki/pause-amd64:3.0 \
  --experimental-bootstrap-kubeconfig=/etc/kubernetes/bootstrap.kubeconfig \
  --kubeconfig=/etc/kubernetes/kubelet.kubeconfig \
  --require-kubeconfig \
  --cert-dir=/etc/kubernetes/ssl \
  --cluster_dns=10.254.0.2 \
  --cluster_domain=cluster.local. \
  --hairpin-mode promiscuous-bridge \
  --allow-privileged=true \
  --serialize-image-pulls=false \
  --logtostderr=true \
  --v=2
ExecStopPost=/sbin/iptables -A INPUT -s 10.0.0.0/8 -p tcp --dport 4194 -j ACCEPT
ExecStopPost=/sbin/iptables -A INPUT -s 172.16.0.0/12 -p tcp --dport 4194 -j ACCEPT
ExecStopPost=/sbin/iptables -A INPUT -s 192.168.0.0/16 -p tcp --dport 4194 -j ACCEPT
ExecStopPost=/sbin/iptables -A INPUT -p tcp --dport 4194 -j DROP
Restart=on-failure
RestartSec=5

[Install]
WantedBy=multi-user.target
EOF
```

### 启动 kubelet
```
systemctl daemon-reload
systemctl enable kubelet&&systemctl start kubelet
systemctl status kubelet
```

## 配置 TLS 认证
#### 查看 csr 的名称
```
kubectl get csr
```
```
[root@node01 ~]# kubectl get csr
NAME                                                   AGE       REQUESTOR           CONDITION
node-csr-8yn8D11LnIZF3PPKtj9s2fFuojUUJeNlk12Ik5ZYSJ4   14h       kubelet-bootstrap   Pending
```

#### 增加 认证
```
kubectl certificate approve node-csr-8yn8D11LnIZF3PPKtj9s2fFuojUUJeNlk12Ik5ZYSJ4
```
```
[root@node01 ~]# kubectl certificate approve node-csr-8yn8D11LnIZF3PPKtj9s2fFuojUUJeNlk12Ik5ZYSJ4
certificatesigningrequest "node-csr-8yn8D11LnIZF3PPKtj9s2fFuojUUJeNlk12Ik5ZYSJ4" approved
```

## 验证 nodes
