# ModelGuard

## Background

In the AI era, the use of models in various applications needs to transition from the laboratory to the production environment.

Edge inference is a very important scenario, and it faces many challenges:
-  Availability: Multiple applications call hardware inference, hardware virtualization has granularity and support issues, software virtualization can effectively control traffic, allocate computing resources, and control application QoS.
-  Security: Some private models are not allowed to be called arbitrarily.
-  Collaboration: Including cloud-edge collaboration of resources and collaboration with IoT devices.
-  Observability: Lack of effective monitoring and logging of model inference services, making it difficult to perform performance analysis and troubleshooting.


## Introduction

`Edgewize ModelGuard` is an inference service governance framework designed specifically for edge computing environments. On resource-constrained edge devices, traditional model servers face challenges such as resource contention, load fluctuations, model privacy, and service quality. ModelGuard effectively addresses these issues through innovative mechanisms such as intelligent queue management, resource isolation and sharing, adaptive load balancing, edge collaborative computing, and fine-grained access control. It provides a flexible and efficient inference service management solution for edge AI applications, ensuring critical task performance while optimizing resource utilization to meet complex business needs.


![`Edgewize ModelGuard` Intro](docs/static/modelguard-intro.svg)


## Architecture

The architecture of `Edgewize ModelGuard` is as follows:



![`Edgewize ModelGuard` Arch](docs/static/modelguard-arch.svg)



The functions of the three components are as follows:

* ***ModelGuard-Controller***: A control plane implemented in Go, providing management of data plane components, such as Sidecar injection, configuration conversion, and distribution. It is the entry point for all configurations of `Edgewize ModelGuard`.

* ***ModelGuard-Proxy***: A high-performance proxy implemented in Go, capturing application inference service access traffic and implementing various governance capabilities such as traffic management, access control, firewall, and observability based on this.

* ***ModelMesh-Broker***: The inference service data plane, deployed on each node in the cluster, providing programmable resource management through various capabilities of the host kernel, such as TrafficQoS.


## Features

### Inference Service Traffic Management

When applications access inference services, `Edgewize ModelGuard` can intercept all traffic.

### Observability

Inference service monitoring metrics are usually obtained from related instances. With `Edgewize ModelGuard`, various inference service access metrics can be visualized.

### Multi-Mode

`Edgewize ModelGuard` supports multiple ways to access inference services, and will support methods such as `MQTT` in the future.



