---
title: Sidecarless Container with eBPF
status: Emerging
category: concept
---

The Sidecarless Container is an implementation pattern that leverages [eBPF](https://glossary.cncf.io/ebpf/) technology to provide sidecar-like functionality without requiring additional containers.

## Problem it addresses

Traditional sidecar patterns in Kubernetes environments introduce several challenges:
- Resource overhead from running multiple containers per pod that increases with load
- Complex deployment and configuration management
- Performance degradation due to buffer-copying and context switching into user space
- Multiple traversals through TCP/IP and socket stacks for service-to-service communication
- Increased operational complexity and attack surface in large-scale deployments
- Higher latency in service-to-service communication
- Greater resource consumption across the cluster

These challenges become more pronounced as applications scale, particularly in microservices architectures requiring extensive service-to-service communication.

## How it helps

eBPF enables sidecarless implementations by running sandboxed programs directly in the Linux kernel without modifying kernel source code or loading kernel modules. This approach:

- Reduces resource consumption by eliminating additional containers
- Simplifies deployments with single-container configurations
- Improves performance through kernel-level interception instead of proxy-based networking
- Decreases latency by avoiding unnecessary network stack traversals
- Minimizes operational complexity through centralized management
- Enables fine-grained observability and security controls without application changes
- Operates once per host rather than per pod/container

For example, an eBPF program can be linked to a socket connect() call, redirecting traffic to a local port where another eBPF program is listening. This creates an efficient service mesh data plane without the overhead of sidecars.

## Notable implementations

Several Kubernetes-native projects leverage eBPF for sidecarless patterns:

- **Cilium Service Mesh**: Provides service mesh functionality without Envoy sidecars, optimizing L3/L4 networking
- **Tetragon**: Implements security policies and runtime enforcement without sidecars
- **Pixie**: Offers observability and monitoring without dedicated agent containers
- **Inspektor Gadget**: Provides debugging and tracing capabilities without sidecars
