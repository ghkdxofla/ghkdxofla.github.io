---
layout: post
title:  "Local Server에 Kubernetes Cluster 구축하기 (0)"
categories: [Distributed System, Kubernetes]
shortinfo: "남는 Local Server 성능이 좋다. 여기의 자원을 잘 활용할 겸, 공부 겸... Kubernetes로 infra를 구축해본다. "
tags: [Distributed System, Kubernetes, container orchestration]
comments: true
---

# 마스터 노드에서 필요한 필수 포트

> 6443 포트 : Kubernetes API Server / Used By All
> 2379~2380 포트 : etcd server client API / Used By kube-apiserver, etcd
> 10250 포트 : Kubelet API / Used By Self, Control plane
> 10251 포트 : kube-scheduler / Used By Self
> 10252 포트 : kube-controller-manager / Used By Self

# 워커 노드에서 필요한 필수 포트

> 10250 포트 : Kubelet API / Used By Self, Control plane
> 30000~32767 포트 : NodePort Services / Used By All

# Before installation

## Swap off

```bash
swapoff -a 
sed -i '2s/^/#/' /etc/fstab
```

ex) #/swapfile        none    swap  sw    0   0

# Set kubernetes core package

# Install kubernetes package

```bash
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
cat <<EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF
```

```bash
sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl
```

## 패키지가 자동으로 설치, 업그레이드, 제거되지 않도록 hold함.

```bash
sudo apt-mark hold kubelet kubeadm kubectl
```

## 설치 완료 확인

```bash
kubeadm version
kubelet --version
kubectl version
```

# Set containerd

## Before installing containerd

```bash
cat <<EOF | sudo tee /etc/modules-load.d/containerd.conf
overlay
br_netfilter
EOF
```

```bash
sudo modprobe overlay
sudo modprobe br_netfilter
```

## 필요한 sysctl 파라미터를 설정하면 재부팅 후에도 유지된다
```bash
cat <<EOF | sudo tee /etc/sysctl.d/99-kubernetes-cri.conf
net.bridge.bridge-nf-call-iptables  = 1
net.ipv4.ip_forward                 = 1
net.bridge.bridge-nf-call-ip6tables = 1
EOF
```

## 재부팅하지 않고 sysctl 파라미터 적용

```bash
sudo sysctl --system
```

## Install containerd

```bash
sudo apt-get update && sudo apt-get install -y containerd
```

## containerd 구성

```bash
sudo mkdir -p /etc/containerd
containerd config default | sudo tee /etc/containerd/config.toml
```

## set systemd cgroup driver
To use the systemd cgroup driver in /etc/containerd/config.toml with runc, set

```
[plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc]
  ...
  [plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc.options]
    SystemdCgroup = true
```

## Restart containerd

```bash
sudo systemctl restart containerd
```

# Init kubeadm

```bash
sudo kubeadm Init
```

## When error is occured(The connection to the server localhost:8080 was refused - did you specify the right host or port?)

```bash
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

## kubeadm reset 시 Error 발생할 경우

### unknown service runtime.v1alpha2.RuntimeService

이 경우는 containerd를 사용할 때 발생함

```bash
rm /etc/containerd/config.toml
systemctl restart containerd
kubeadm reset -f
kubeadm init
```

### kubectl get pods --all-namespaces 시, Error 발생할 경우

Error message는 다음과 같다
- Unable to connect to the server: x509: certificate signed by unknown authority (possibly because of "crypto/rsa: verification error" while trying to verify candidate authority certificate "kubernetes")

```bash
mv  $HOME/.kube $HOME/.kube.bak
mkdir $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```
참고 : [TLS certificate errors](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/troubleshooting-kubeadm/#tls-certificate-errors)

# Set Flannel

## CNI?

다른 Worker Node의 Pod 간 통신 문제점을 해결

참고 : [Kubernetes Network 이론](https://ikcoo.tistory.com/11)

## Flannel?

서로 다른 노드에 있는 pod 간 통신을 위한 CNI Plug-in

Flannel is a simple and easy way to configure a layer 3 network fabric designed for Kubernetes.

Flannel runs a small, single binary agent called flanneld on each host, and is responsible for allocating a subnet lease to each host out of a larger, preconfigured address space. Flannel uses either the Kubernetes API or etcd directly to store the network configuration, the allocated subnets, and any auxiliary data (such as the host's public IP). Packets are forwarded using one of several backend mechanisms including VXLAN and various cloud integrations.

참고 : [Flannel homepage](https://github.com/flannel-io/flannel)

## coredns의 status가 pending일 경우

아래와 같이 Status가 pending으로, 컨테이너가 running status가 아님

```bash
NAMESPACE     NAME                                     READY   STATUS    RESTARTS   AGE
kube-system   coredns-74ff55c5b-lc48g                  0/1     Pending   0          22h
kube-system   coredns-74ff55c5b-spf2t                  0/1     Pending   0          22h
kube-system   etcd-taelim-desktop                      1/1     Running   0          22h
kube-system   kube-apiserver-taelim-desktop            1/1     Running   0          22h
kube-system   kube-controller-manager-taelim-desktop   1/1     Running   0          22h
kube-system   kube-proxy-jlhv5                         1/1     Running   0          22h
kube-system   kube-scheduler-taelim-desktop            1/1     Running   0          22h
```

## coredns의 loop 제외

```bash
kubectl edit cm coredns -n kube-system
```

```
# Please edit the object below. Lines beginning with a '#' will be ignored,
# and an empty file will abort the edit. If an error occurs while saving this file will be
# reopened with the relevant failures.
#
apiVersion: v1
data:
  Corefile: |
    .:53 {
        errors
        health {
           lameduck 5s
        }
        ready
        kubernetes cluster.local in-addr.arpa ip6.arpa {
           pods insecure
           fallthrough in-addr.arpa ip6.arpa
           ttl 30
        }
        prometheus :9153
        forward . /etc/resolv.conf {
           max_concurrent 1000
        }
        cache 30
        # loop # 여기를 주석 처리
        reload
        loadbalance
    }
kind: ConfigMap
metadata:
  creationTimestamp: "2021-03-25T00:57:38Z"
  name: coredns
  namespace: kube-system
  resourceVersion: "97463"
  uid: da297432-8aa8-4d73-a3d6-547e69e582de
```

## Install Flannel

```bash
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
```

이후 확인 시, coredns 또한 running status 임을 확인 가능

참고 : [Kubernetes Cluster 구성하기](https://www.howtodo.cloud/kubernetes/2019/08/13/kubernetes-kubeadm.html)

---
이후의 Worker node 생성 및 연결 작업, Dashboard 세팅 등을 업로드 할 예정