---
title: Kubernetes Service Q & A
layout: post
subtitle: null
date: '2023-01-01 00:30:00'
author: Lanzhou
tags:
- study
- 中文
---
# Kubernetes Service Q & A

- ReplicaSet, Deployment和service有什么区别？
    - ReplicaSet: 确保任何时间都有指定数量的Pod副本在运行。
    - Deployment: 管理ReplicaSet，向Pod提供声明式的更新，保证一组pods在处于运行状态。（建议使用Deployment而不是直接使用ReplicaSet）
    - Service：
        - 使得一组pods可以被访问（Pods are ephemeral! 通过pod的IP地址访问不现实，service可以提供稳定的IP address）
        - 实现loadbalancing
        - 还可以帮助实现应用程序中微服务之间的松散耦合（loose coupling）。

- NodePort, ClusterIP等几种services type有什么区别？
    - [https://stackoverflow.com/questions/41509439/whats-the-difference-between-clusterip-nodeport-and-loadbalancer-service-types](https://stackoverflow.com/questions/41509439/whats-the-difference-between-clusterip-nodeport-and-loadbalancer-service-types)
    - ClusterIP Services
    - NodePort Services
    - Headless Services
    - LoadBalancer Services
- Port TargetPort NodePort等有什么区别？
        - Port: Service的Port，可以随意自定义
        - TargetPort： Pod上的目标Port，不可随意自定义，必须符合container监听的Port
- Service selector怎么用？多个selector有什么作用？
    - Pods’ labels  - Must match ALL the selectors，这样pod才可以成为Service的一个endpoint

[https://www.youtube.com/watch?v=T4Z7visMM4E](https://www.youtube.com/watch?v=T4Z7visMM4E)

---

新年快乐，Happy New Year!!