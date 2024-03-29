---
title: "Kubernetes 镜像更新问题"
date: 2022-06-04T12:34:56+08:00
summary: "Kubernetes 中镜像名及标签没有更新时应当如何更新镜像？"
tags: [cloud,kubernetes]
---
# Kubernetes 镜像更新问题

## 问题描述

在服务部署中，想要更新自己所使用的镜像。

特别是在开发过程中，经常遇到镜像内容更新，但是镜像名及标签没有更新的情况。

## 错误尝试

根据之前 `docker compose` 的相关经验，直接使用

```shell
docker compose pull
```

但是实际上并没有效果。

## 最佳实践

虽然这个最佳实践确实非常愚蠢，但是可能确实是最佳实践。

根据[官方文档](https://kubernetes.io/docs/concepts/containers/images/)我们可以知道，镜像拉取策略分为三种：

- `IfNotPresent`：如果本地不存在的话才拉取

-  `Always`：无论何时总是拉取
-  `Never`：无论何时总是不拉取

在默认情况下镜像拉取策略会是 `IfNotPresent`，因此一旦（拉取然后）运行之后就不会再次拉取最新的标签。此时需要我们手动修改成 `Always` 拉取最新镜像，然后再改回原来的设置。

> 因此，与 docker 不同，在 Kubernetes 中镜像应该尽量避免使用 `latest` 这样的标签，因为它认不出来！

## 远期拓展

一个个去修改 `deployment` 文件其实也是一件非常麻烦的事情，未来可以考虑使用 `helm` 这样的工具，更为方便地达成相关目的。
