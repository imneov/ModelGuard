# ModelGuard

## Background

AI 时代，模型在各种应用程序中的使用需要从实验室走向生产环境

边缘推理是非常重要的一个场景，这里面临很多挑战
-  可用性，多个应用调用硬件推理，硬件虚拟化存在颗粒度以及支持性问题，软件虚拟化可以有效的进行流量控制，分配计算资源，控制应用QoS
-  安全性，某些私有模型不允许随意调用
-  协同性，包括资源的云边协同以及与 IoT 设备的协同
-  可观测性，缺乏对模型推理服务的有效监控和日志记录，难以进行性能分析和故障排查


## Introduction

`Edgewize ModelGuard`  是一款专为边缘计算环境设计的推理服务治理框架。在资源受限的边缘设备上，传统模型服务器面临资源争用、负载波动、模型隐私和服务质量等挑战。ModelGuard通过智能队列管理、资源隔离与共享、自适应负载均衡、边缘协同计算和细粒度访问控制等创新机制，有效解决了这些问题。它为边缘AI应用提供了灵活、高效的推理服务管理方案，确保关键任务性能，同时优化资源利用，满足复杂业务需求。


![`Edgewize ModelGuard` Intro](docs/static/modelguard-intro.svg)


## Architecture

`Edgewize ModelGuard` 的架构图如下：



![`Edgewize ModelGuard` Arch](docs/static/modelguard-arch.svg)



三个组件的功能分别为：

* ***ModelGuard-Controller***: 用 Go 实现的控制面，提供对数据面组件的管控，如 Sidecar 注入、配置转换和下发，是 `Edgewize ModelGuard` 所有配置的入口。

* ***ModelGuard-Proxy***: 用 Go 实现的高性能量代理，获取应用的推理服务访问流量，并基于此实现流量治理、访问控制、防火墙、可观测性等各种治理能力。

* ***ModelMesh-Broker***: 推理服务数据面，部署在集群中每个节点上并通过宿主机内核的各种能力提供可编程资源管理，如 TrafficQoS 等。


## 特性

### 推理服务流量治理

应用访问推理服务，`Edgewize ModelGuard` 可以劫持所有的流量。

### 可观测性

推理服务的监控指标通常从相关实例处获取，借助 `Edgewize ModelGuard` 可以透视多种推理服务访问指标。

### 多模式

`Edgewize ModelGuard` 支持多种方式访问推理服务，未来还会支持 `MQTT` 等方式。





## 开发手册

### 测试

```
go get github.com/onsi/ginkgo/ginkgo
go get github.com/onsi/gomega/...@v1.27.7
```

### 本地调试

```
cd tests/e2e
go test

ginkgo bootstrap
```

### 修改 proto

如果修改了 `api/modelfulx/v1alpha` 下的 protobuf 文件，需要用以下命令重新生成对应的 go 文件

```
make proto
```


***注意：修改完 proto 后需要修改`import`***

- `api/modelfulx/v1alpha/modelmesh_service.pb.go`
```
import {
    ...
    proto1 "github.com/edgewize/modelmesh/mindspore_serving/proto"
    ...
}
```

- `api/modelfulx/v1alpha/modelmesh_service.validator.pb.go`
```
import {
    ...
    _ "github.com/edgewize/modelmesh/mindspore_serving/proto"
    ...
}
```