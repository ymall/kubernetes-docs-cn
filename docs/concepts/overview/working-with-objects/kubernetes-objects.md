---
title: 理解 Kubernetes 对象

redirect_from:
- "/docs/concepts/abstractions/overview/"
- "/docs/concepts/abstractions/overview.html"
---

{% capture overview %}
<!--
This page explains how Kubernetes objects are represented in the Kubernetes API, and how you can express them in `.yaml` format.
-->
该页说明了 Kubernetes 对象在 Kubernetes API 中是如何表示的，以及如何在 `.yaml` 格式的文件中表示。
{% endcapture %}

{% capture body %}

<!--
## Understanding Kubernetes Objects

*Kubernetes Objects* are persistent entities in the Kubernetes system. Kubernetes uses these entities to represent the state of your cluster. Specifically, they can describe:

* What containerized applications are running (and on which nodes)
* The resources available to those applications
* The policies around how those applications behave, such as restart policies, upgrades, and fault-tolerance
-->


## 理解 Kubernetes 对象

在 Kubernetes 系统中，*Kubernetes 对象* 是持久化的条目。Kubernetes 使用这些条目去表示整个集群的状态。特别地，它们描述了如下信息：

* 什么容器化应用在运行（以及在哪个 Node 上）
* 可以被应用使用的资源
* 关于应用如何表现的策略，比如重启策略、升级策略，以及容错策略

<!--
A Kubernetes object is a "record of intent"--once you create the object, the Kubernetes system will constantly work to ensure that object exists. By creating an object, you're effectively telling the Kubernetes system what you want your cluster's workload to look like; this is your cluster's **desired state**.

To work with Kubernetes objects--whether to create, modify, or delete them--you'll need to use the [Kubernetes API](https://git.k8s.io/community/contributors/devel/api-conventions.md). When you use the `kubectl` command-line interface, for example, the CLI makes the necessary Kubernetes API calls for you; you can also use the Kubernetes API directly in your own programs. Kubernetes currently provides a `golang` [client library](https://github.com/kubernetes/client-go) for this purpose, and other language libraries (such as [Python](https://github.com/kubernetes-client/python)) are being developed.
-->

Kubernetes 对象是 “目标性记录” —— 一旦创建对象，Kubernetes 系统将持续工作以确保对象存在。通过创建对象，可以有效地告知 Kubernetes 系统，所需要的集群工作负载看起来是什么样子的，这就是 Kubernetes 集群的 **期望状态**。

与 Kubernetes 对象工作 —— 是否创建、修改，或者删除 —— 需要使用 [Kubernetes API](https://git.k8s.io/community/contributors/devel/api-conventions.md)。当使用 `kubectl` 命令行接口时，比如，CLI 会使用必要的 Kubernetes API 调用，也可以在程序中直接使用 Kubernetes API。为了实现该目标，Kubernetes 当前提供了一个 `golang` [客户端库](https://github.com/kubernetes/client-go) 
，其它语言库（例如[Python](https://github.com/kubernetes-client/python)）也正在开发中。

<!--
### Object Spec and Status

Every Kubernetes object includes two nested object fields that govern the object's configuration: the object *spec* and the object *status*. The *spec*, which you must provide, describes your *desired state* for the object--the characteristics that you want the object to have. The *status* describes the *actual state* for the object, and is supplied and updated by the Kubernetes system. At any given time, the Kubernetes Control Plane actively manages an object's actual state to match the desired state you supplied.
-->

### 对象 Spec 与状态

每个 Kubernetes 对象包含两个嵌套的对象字段，它们负责管理对象的配置：对象 *spec* 和 对象 *status*。*spec* 必须提供，它描述了对象的 *期望状态* —— 希望对象所具有的特征。*status* 描述了对象的 *实际状态*，它是由 Kubernetes 系统提供和更新。在任何时刻，Kubernetes 控制平面一直处于活跃状态，管理着对象的实际状态以与我们所期望的状态相匹配。

<!--
For example, a Kubernetes Deployment is an object that can represent an application running on your cluster. When you create the Deployment, you might set the Deployment spec to specify that you want three replicas of the application to be running. The Kubernetes system reads the Deployment spec and starts three instances of your desired application--updating the status to match your spec. If any of those instances should fail (a status change), the Kubernetes system responds to the difference between spec and status by making a correction--in this case, starting a replacement instance.

For more information on the object spec, status, and metadata, see the [Kubernetes API Conventions](https://git.k8s.io/community/contributors/devel/api-conventions.md).
-->

例如，Kubernetes Deployment 对象能够表示运行在集群中的应用。当创建 Deployment 时，可能需要设置 Deployment 的 spec，以指定该应用需要有 3 个副本在运行。Kubernetes 系统读取 Deployment spec，启动我们所期望的该应用的 3 个实例 —— 更新状态以与 spec 相匹配。如果那些实例中有失败的（一种状态变更），Kubernetes 系统通过修正来响应 spec 和状态之间的不一致 —— 这种情况，启动一个新的实例来替换。

关于对象 spec、status 和 metadata 更多信息，查看 [Kubernetes API Conventions](https://git.k8s.io/community/contributors/devel/api-conventions.md)。

<!--
### Describing a Kubernetes Object

When you create an object in Kubernetes, you must provide the object spec that describes its desired state, as well as some basic information about the object (such as a name). When you use the Kubernetes API to create the object (either directly or via `kubectl`), that API request must include that information as JSON in the request body. **Most often, you provide the information to `kubectl` in a .yaml file.** `kubectl` converts the information to JSON when making the API request.

Here's an example `.yaml` file that shows the required fields and object spec for a Kubernetes Deployment:

{% include code.html language="yaml" file="nginx-deployment.yaml" ghlink="/docs/concepts/overview/working-with-objects/nginx-deployment.yaml" %}

One way to create a Deployment using a `.yaml` file like the one above is to use the [`kubectl create`](/docs/user-guide/kubectl/v1.7/#create) command in the `kubectl` command-line interface, passing the `.yaml` file as an argument. Here's an example:
-->

### 描述 Kubernetes 对象

当创建 KUbernetes 对象时，必须提供对象的 spec，用来描述该对象的期望状态，以及关于对象的一些基本信息（例如，名称）。当使用 KUbernetes API 创建对象时（或者直接创建，或者基于`kubectl`），API 请求必须在请求体中包含 JSON 格式的信息。**更常用的是，需要在 .yaml 文件中为 `kubectl` 提供这些信息**。 `kubectl` 在执行 API 请求时，将这些信息转换成 JSON 格式。

这里有一个 `.yaml` 示例文件，展示了 KUbernetes Deployment 的必需字段和对象 spec：

{% include code.html language="yaml" file="nginx-deployment.yaml" ghlink="/docs/concepts/overview/working-with-objects/nginx-deployment.yaml" %}

一种创建 Deployment 的方式，类似上面使用 `.yaml` 文件，是使用 `kubectl` 命令行接口（CLI）中的 [`kubectl create`](/docs/user-guide/kubectl/v1.7/#create) 命令，传递 `.yaml` 作为参数。下面是一个示例：

```shell
$ kubectl create -f docs/user-guide/nginx-deployment.yaml --record
```
<!--
The output is similar to this:
-->

输出类似如下这样：

```shell
deployment "nginx-deployment" created
```

<!--
### Required Fields

In the `.yaml` file for the Kubernetes object you want to create, you'll need to set values for the following fields:

* `apiVersion` - Which version of the Kubernetes API you're using to create this object
* `kind` - What kind of object you want to create
* `metadata` - Data that helps uniquely identify the object, including a `name` string, UID, and optional `namespace`
-->

### 必需字段

在想要创建的 KUbernetes 对象对应的 `.yaml` 文件中，需要配置如下的字段：

* `apiVersion` - 创建该对象所使用的 Kubernetes API 的版本
* `kind` - 想要创建的对象的类型
* `metadata` - 帮助识别对象唯一性的数据，包括一个 `name` 字符串、UID 和可选的 `namespace`

<!--
You'll also need to provide the object `spec` field. The precise format of the object `spec` is different for every Kubernetes object, and contains nested fields specific to that object. The [Kubernetes API reference](/docs/api/) can help you find the spec format for all of the objects you can create using Kubernetes.
-->

也需要提供对象的 `spec` 字段。对象 `spec` 的精确格式对每个 Kubernetes 对象来说是不同的，包含了特定于该对象的嵌套字段。[Kubernetes API 参考](/docs/api/)能够帮助我们找到任何我们想创建的对象的 spec 格式。

{% endcapture %}

{% capture whatsnext %}
<!--
* Learn about the most important basic Kubernetes objects, such as [Pod](/docs/concepts/abstractions/pod/).
-->
* 了解最重要的基本 Kubernetes 对象，例如 [Pod](/docs/concepts/abstractions/pod/)。
{% endcapture %}

{% include templates/concept.md %}
