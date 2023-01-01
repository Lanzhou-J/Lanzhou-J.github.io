---
title: Kubernetes Service 相关问题笔记
layout: post
subtitle: null
date: '2023-01-01 00:30:00'
author: Lanzhou
tags:
- study
- 中文
---
# Kubernetes Service 相关问题笔记

- ReplicaSet, Deployment和service有什么区别？
    - ReplicaSet: 确保任何时间都有指定数量的Pod副本在运行。
    - Deployment: 管理ReplicaSet，向Pod提供声明式的更新，保证一组pods在处于运行状态。（建议使用Deployment而不是直接使用ReplicaSet）
    - Service：
        - 使得一组pods可以被访问（Pods are ephemeral! 通过pod的IP地址访问不现实，service可以提供稳定的IP address）
        - 实现loadbalancing
        - 还可以帮助实现应用程序中微服务之间的松散耦合（loose coupling）。

- NodePort, ClusterIP等几种Service types有什么区别？
    - ClusterIP Services
      - 通过集群的内部IP暴露服务
      - 通常无法从外部访问
      - 是默认的Service类型
    - NodePort Services
      - 通过Node（节点）上的IP和静态端口暴露服务
      - 用户可以从集群外部访问NodePort service，访问`<NodeIP>:<NodePort>`
    - LoadBalancer Services
      - 使用云提供商(e.g. AWS)的负载均衡器向外部暴露服务。
- Port， TargetPort， NodePort等有什么区别？
    - Port: Service的Port，可以随意自定义
    - TargetPort： Pod上的目标Port，不可随意自定义，必须符合container监听的Port
- Service selector怎么用？多个selector有什么作用？
    - Pods’ labels  - Must match ALL the selectors，这样pod才可以成为Service的一个endpoint

### Reference
- [https://www.youtube.com/watch?v=T4Z7visMM4E](https://www.youtube.com/watch?v=T4Z7visMM4E)

- [https://stackoverflow.com/questions/41509439/whats-the-difference-between-clusterip-nodeport-and-loadbalancer-service-types](https://stackoverflow.com/questions/41509439/whats-the-difference-between-clusterip-nodeport-and-loadbalancer-service-types)

---

新年快乐，Happy New Year!!