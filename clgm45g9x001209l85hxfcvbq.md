---
title: "Running VS Code Devcontainers with X86_64 Runtime on Apple Silicon"
seoTitle: "Running Devcontainers with X86_64 Runtime on Apple Silicon"
seoDescription: "This article showcases the features of Colima especially at if affects running devcontainers in architectures different from the host machine architecture."
datePublished: Tue Apr 18 2023 10:19:52 GMT+0000 (Coordinated Universal Time)
cuid: clgm45g9x001209l85hxfcvbq
slug: running-vs-code-devcontainers-with-x8664-runtime-on-apple-silicon
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1681813175368/8f9a9702-99b6-4bf7-a1b2-cf7700d41c89.jpeg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1681813129987/312ca525-7945-49da-a8b2-ae3df52094da.jpeg
tags: docker, virtualization, qemu, colima, devcontainer

---

This article will guide you through the steps required to run `X86_64` containers on Apple Silicon machines. Although Docker Desktop for Mac added beta support for `X86_64` emulation via Rosetta in version [4.16.0](https://docs.docker.com/desktop/release-notes/#4160), it doesn’t work for some `X86_64` containers, such as oracle-xe container images. A unique issue with VS Code Devcontainers is that it selects its container runtime based on the docker runtime. This means that it will always try to use `ARM64` images, regardless of whether you have Rosetta emulation enabled or not.

## Background

Recently, I had to work on a Python project that depended on some libraries which appear to only work on `X86_64` machines, I came across Colima during my struggles with getting `oracle-xe` containers to work on my machine and settled completely with it after this episode of failing to run `X86_64` dev containers with Docker Desktop.

## Hello Colima,

[Colima](https://github.com/abiosoft/colima), literally meaning **Co**ntainers in **Li**nux on **ma**cOS is a container runtime with minimal setup. it boasts several awesome features, including the ability to run multiple Linux instances, support for both docker and container runtimes and lightweight Kubernetes clusters powered by [k3s](https://k3s.io/).

I settled for Colima due to the following reasons;

1. Support for everything I get from Docker Desktop (I don't use Docker extensions, although I got the lead from `oracle-xe` from a Docker desktop plugin)
    
2. QEMU-powered `X86_64` emulation for the `X86_64` runtime (may be slow, but has better compatibility than the Rosetta-based emulation in Docker Desktop).
    
3. Run two or more docker runtimes at the same time (I prefer to continue running my arm native tools in an arm-based docker instance).
    
4. Spin up a `k3s` Kubernetes cluster the easiest way.
    

## Docker Contexts

[Docker Contexts](https://docs.docker.com/engine/context/working-with-contexts/#introduction) is a Docker CLI feature that enables a single Docker CLI installation to manage multiple Docker hosts, swarm clusters, and even Kubernetes clusters.

Colima rides on this feature to provide great features like running multiple Linux instances on a single machine and using docker context, you can easily switch, and manage container deployments across several docker instances, including instances not managed by Colima (e.g Docker Desktop).

## VS Code Dev Containers

For developers who do not like clogging their dev machines with all the tools, [Development Container (or Dev Container for short)](https://containers.dev/) allows you to use a container as a full-featured development environment. It can be used to run an application, to separate tools, libraries, or runtimes needed for working with a codebase, and to aid in continuous integration and testing. Dev containers can be run locally or remotely, in a private or public cloud.

Visual Studio Code has the VS Code Dev Container plugin that simplifies the process to spin up development containers right within the editor. While it has support for most CPU architecture, it derives the instantiated container architecture based on the OS architecture of the active Docker context.

This means that to run an `X86_64` devcontainer, you need to ensure that the active Docker context is based on the `X86_64` architecture. This can be verified by doing the following:

```bash
# start X86_64 container in colima (skip if you already have an X86_64 docker instance)
colima start --arch x86_64 -p qemu

# review available docker contexts
docker context list

# switch active docker context to an X86_64 instance (colima instances are always prefixed with 'colima-'
docker context use colima-qemu
```

You can easily switch back to any other context, once the devcontainer is running.

## Conclusion

Using this approach, we can successfully run X86\_64 (aka AMD64) development workloads on Apple Silicon without spinning up a full-fledged virtual machine.

Maybe this approach can be used to achieve the reverse (ARM64 workloads on X86\_64 machines), I am not sure, but it may one day be worth the try for me.