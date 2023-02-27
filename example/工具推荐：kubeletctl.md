---
tags: 云原生安全资讯,项目推荐
spec version: v0.1.0
---

## 1. 工具推荐：kubeletctl 

* 项目地址：https://github.com/cyberark/kubeletctl
* 原文介绍：https://www.cyberark.com/resources/threat-research-blog/using-kubelet-client-to-attack-the-kubernetes-cluster

### 1.1 What

kubeletctl 实现了 kubelet 的cli客户端，可以调用kubelet的大部分API，也可以实现了一些实用的利用。目前该项目仍在继续更新，前两个release版本是2023-01-09，2022-10-31。

### 1.2 Why

尽管官方有kubectl，但kubectl实际实现的是API Server的部分API。kubeletctl直接与kubelet交互，甚至还包括一些官方文档未列出的隐藏API。以往我们访问kubelet 的10250 接口时，可能会使用到curl，但对于一些复杂请求，curl可能会比较难用。kubeletctl是比较适合解决这些问题的。

### 1.3 How 

kubeletctl 支持以下子命令。

```
Available Commands:
  attach        Attach to a container
  checkpoint    Taking a container snapshot
  configz       Return kubelet's configuration.
  containerLogs Return container log
  cri           Run commands inside a container through the Container Runtime Interface (CRI)
  debug         Return debug information (pprof or flags)
  exec          Run commands inside a container
  healthz       Check the state of the node
  help          Help about any command
  log           Return the log from the node.
  metrics       Return resource usage metrics (such as container CPU, memory usage, etc.)
  pods          Get list of pods on the node
  portForward   Attach to a container
  run           Run commands inside a container
  runningpods   Returns all pods running on kubelet from looking at the container runtime cache.
  scan          Scans for nodes with opened kubelet API
  spec          Cached MachineInfo returned by cadvisor
  stats         Return statistical information for the resources in the node.
  version       Print the version of the kubeletctl
```

除了常见的 pods, exec等命令，kubeletctl 还支持一些特色的命令。

比如 scan命令，可以扫描kubelet节点

```
$ kubeletctl scan
[*] Using KUBECONFIG environment variable
[*] You can ignore it by modifying the KUBECONFIG environment variable, file "~/.kube/config" or use the "-i" switch
┌───────────────────┐
│ Nodes with opened │
│    Kubelet API    │
├───┬───────────────┤
│   │ NODE IP       │
├───┼───────────────┤
│ 1 │ 192.168.1.193 │
└───┴───────────────┘
```

`kubeletctl run "whoami" --all-pods`支持在每个pod内批量执行命令

```
# kubeletctl run "whoami" --all-pods
[*] Using KUBECONFIG environment variable
[*] You can ignore it by modifying the KUBECONFIG environment variable, file "~/.kube/config" or use the "-i" switch
[*] Running command on all pods
...
14. Pod: kube-proxy-rpf76
    Namespace: kube-system
    Container: kube-proxy
    Url: https://192.168.1.193:10250/run/kube-system/kube-proxy-rpf76/kube-proxy
    Output:
root



15. Pod: calico-node-lq7mv
    Namespace: calico-system
    Container: calico-node
    Url: https://192.168.1.193:10250/run/calico-system/calico-node-lq7mv/calico-node
    Output:
root
...
```

`kubeletctl scan token` 支持扫描每个pod内挂载的token

```
# kubeletctl scan token
[*] Using KUBECONFIG environment variable
[*] You can ignore it by modifying the KUBECONFIG environment variable, file "~/.kube/config" or use the "-i" switch
...
4. Pod: kube-proxy-rpf76
    Namespace: kube-system
    Container: kube-proxy
    Url: https://192.168.1.193:10250/run/kube-system/kube-proxy-rpf76/kube-proxy
    Output:
eyJ***************0A


╭───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╮
│                                                                                                     Decoded JWT token                                                                                                     │
├───────────────┬───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┤
│      KEY      │                                                                                                   VALUE                                                                                                   │
├───────────────┼───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┤
│      iat      │                                                                                              1.676617112e+09                                                                                              │
├───────────────┼───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┤
│      iss      │                                                                                https://kubernetes.default.svc.cluster.local                                                                               │
├───────────────┼───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┤
│ kubernetes.io │ map[namespace:kube-system pod:map[name:kube-proxy-rpf76 uid:21b28e12-6e4f-4eee-98ac-b96c4f32d766] serviceaccount:map[name:kube-proxy uid:a3d90886-4e75-4bd9-af61-85741cb5036d] warnafter:1.676620719e+09] │
├───────────────┼───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┤
│      nbf      │                                                                                              1.676617112e+09                                                                                              │
├───────────────┼───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┤
│      sub      │                                                                                system:serviceaccount:kube-system:kube-proxy                                                                               │
├───────────────┼───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┤
│      aud      │                                                                               [https://kubernetes.default.svc.cluster.local]                                                                              │
├───────────────┼───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┤
│      exp      │                                                                                              1.708153112e+09                                                                                              │
╰───────────────┴───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯
...
```

### 1.4 Try

试用了，好使。可以考虑在该工具的基础上，迭代一些自定义的功能。

```
$ wget https://github.com/cyberark/kubeletctl/releases/download/v1.10/kubeletctl_linux_amd64 && chmod a+x ./kubeletctl_linux_amd64 && mv ./kubeletctl_linux_amd64 /usr/local/bin/kubeletctl
$ kubeletctl pods
[*] Using KUBECONFIG environment variable
[*] You can ignore it by modifying the KUBECONFIG environment variable, file "~/.kube/config" or use the "-i" switch
┌──────────────────────────────────────────────────────────────────────────────────────────────────┐
│                                         Pods from Kubelet                                        │
├────┬────────────────────────────────────────────────┬──────────────────┬─────────────────────────┤
│    │ POD                                            │ NAMESPACE        │ CONTAINERS              │
├────┼────────────────────────────────────────────────┼──────────────────┼─────────────────────────┤
│  1 │ coredns-64897985d-vpwqz                        │ kube-system      │ coredns                 │
│    │                                                │                  │                         │
├────┼────────────────────────────────────────────────┼──────────────────┼─────────────────────────┤
│  2 │ calico-apiserver-5bbd46dfd6-p67bc              │ calico-apiserver │ calico-apiserver        │
│    │                                                │                  │                         │
├────┼────────────────────────────────────────────────┼──────────────────┼─────────────────────────┤
│  3 │ demo-deployment-84b5ff54fb-rdpfn               │ default          │ demo                    │
│    │                                                │                  │                         │
├────┼────────────────────────────────────────────────┼──────────────────┼─────────────────────────┤
│  4 │ exp                                            │ default          │ apparmor-bypass         │
│    │                                                │                  │                         │
├────┼────────────────────────────────────────────────┼──────────────────┼─────────────────────────┤
│  5 │ kube-proxy-rpf76                               │ kube-system      │ kube-proxy              │
│    │                                                │                  │                         │

```

----

> 本文使用[云原生安全资讯：项目推荐](https://github.com/ssst0n3/security-research-specification/blob/main/%E4%BA%91%E5%8E%9F%E7%94%9F%E5%AE%89%E5%85%A8%E8%B5%84%E8%AE%AF/%E9%A1%B9%E7%9B%AE%E6%8E%A8%E8%8D%90.md)作为文档基线
