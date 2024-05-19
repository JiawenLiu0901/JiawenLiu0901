---
weight: 4
title: "Raft常见问题"
date: 2024-05-19T21:57:40+08:00
lastmod: 2024-05-19T16:45:40+08:00
draft: false
author: "Jiawen Liu"
authorLink: "https://jiawenliu0901.github.io"
description: "Raft常见问题"
tags: ["KV分布式项目"]
categories: ["KV分布式项目"]
lightgallery: true
---
## 领导者选举
### 简练的语言概述领导者选举的机制
### Raft协议中有哪些角色？分别负责什么任务？
### 在Raft协议中，什么是Leader选举？Leader选举失败怎么办？
### 在Raft协议中，Follower、Candidate和Leader分别是如何转换状态的？
### Raft协议中的Term是什么？如何使用Term来保证一致性？
### 在Raft协议中，如何避免过多的Leader选举导致性能下降？
### Raft协议中的心跳机制是什么？它有什么作用？
### Raft协议中的Quorum是什么？它有什么作用？
## 日志复制
### 简练的语言概述日志复制的机制
### Raft协议中的日志压缩（log compaction）是什么？有什么作用？
### Raft协议如何处理日志复制的问题？如何处理日志冲突？
### 在Raft协议中，如何处理节点动态加入和删除的情况？
### Raft协议如何处理客户端请求？Leader如何将请求分发给其他节点？
## 使用场景
### Raft协议有哪些局限性？有哪些场景下不适合使用Raft协议？
## 周边协议
### Raft协议的优点和缺点是什么？它与其他分布式一致性协议相比有哪些优劣之处？
### Raft协议有哪些变体
## 安全性与故障恢复
### Raft协议如何防止脑裂（split brain）的问题？
### Raft协议如何处理节点宕机的情况？如何保证数据一致性不受影响？
### Raft协议中有哪些重要的安全性质？如何保证这些安全性质？
