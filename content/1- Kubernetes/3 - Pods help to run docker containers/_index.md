---
title: "Pods help to run docker containers"
date: "`r Sys.Date()`"
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

**Content:**

- [Decide wheather containers should be grouped together in a pod or not](#decide-wheather-containers-should-be-grouped-together-in-a-pod-or-not)

- [Understand pods](#understand-pods)

- [How to define pod using YAML or JSON descriptor](#how-to-define-pod-using-yaml-or-json-descriptor)

- [Labels and selectors](#labels-and-selectors)

- [Schedule pods on specific nodes](#schedule-pods-on-specific-nodes)

- [Annotations](#annotations)

- [Namespaces to virtualy separate cluster for different teams](#namespaces-to-virtualy-separate-cluster-for-different-teams)

- [Kubectl explain](#kubectl-explain)

#### Decide wheather containers should be grouped together in a pod or not

- A pod is a group of one or more containers, with shared storage and network resources. But it is common that a pod has only one container. The key take away is that if a pod contains multiple containers, all the containers that run on the same host machine can access the shared storage and network resources.

![Nodes and containers](images/_index.png)

- There is one question that arises: **Why don't we just use containers directly?**
  The answer for this is that if you have multiple processes that either communicate with each other through IPC (Inter-Process Communication) or through locally stored files, which require them to run on the same machine.

Containers are designed to run only one process per container unless you are spawn child processes. If you run multiple unrelated process on the same container it can mix up your log since all of these processes will log to the same standard output. Therefore, you need to run one process per container. That's how Docker and Kubernetes are meant to be used.

#### Understand pods

- Because you are not group containers in group, so you need a way to bind containers together as a single unit. This is where pods come into play.
- A pod of containers allow you to run closely related processes together and provide them with the same environment as if they were running in a single container, while keeping them somewhat isolated from each other. By this way you can get the best of both worlds: you can leverage the feature that container provide while giving the processes the illusion of running together.

**The partial isolation between containers in same pod**

- The containers in pods share the same **Linux namespace** because all containers of pod un under the same Network, UTS namespaces. (Linux namespaces). They all share the same hostname and network interfaces. Also, all containers in a pod share the same se of IPC namespace and can communicate through IPC.

- Regarding filesystem, each container has its own filesystem. The filesystem of a container is isolated from other containers. However, it's posible to have them share file directories using Kubernetes concept called `Volume` we will see later.
- Containers in a pod run in the same Network namespace, they share the same IP address and port space, this means that containers in the same pod should avoid to use the same port, because they will collide with each other. All the containers in a pod share the same loopback network interface.

**Flat inter-pod network**

- All pods in Kubernetes cluster are using a flat network model, once a pod has IP address of other node it can communicate directly with other pods without NAT between them.
- Communicate between pods is simple since it doesn't matter if a pod is on the same node or on a different node, they can communicate with each other using their IP address.

![Flat inter-pod network](images/_index-1.png)

- To demonstrate this, I've created two simple applications using Node.js comunicate through IPC using loopback network.
  You can find the source code [here](files/demo_ipc.zip).

{{%notice info%}}
If you got error when running the application, make sure to build the image first using docker build command. You can troubleshooting the issue by using `kubectl describe pod ipc-pod`. If you are encounter the error ErrImagePull... 100% need to set up local repository for minikube please refer to this [1 - Setup a local environment K8s](../1%20-%20Setup%20a%20local%20environment%20K8s/_index.md)
{{%/notice%}}

**We can have multiple containers in a pod, there are some reason to do so:**

- **Sidecar container**: the container that periodically download content from an external source and store it in the webserver directory.

![Sidecar container](images/_index-2.png)

Answer three questions when you decide to put containers in the same pod:

- Do they need to run together or can run on separate hosts.
- Do they represent a single entity or independent components.
- Must they be scaled together or can run independently.

There are some pattern that help you memorize this:

![Example of compose containers in to single pod](images/_index-3.png)

#### How to define pod using YAML or JSON descriptor

- Pods and other kubernetes objects are defined using YAML or JSON.
- By using `kubectl apply -f <filename>.yaml` this will make a REST call to Kubernetes API to create the object if it doesn't exist, otherwise it will update the existing object.
- To use all features of Kubernetes, you need to use YAML or JSON. It's common to see people out there using YAML because it's more human-readable.

[Kubernetes API Reference](https://kubernetes.io/docs/reference/)

**Main part of Kubernetes POD resource definition**

- Metadata: including `name`, `namespace`, `labels` and `annotations`.
- Spec: including description of pod's content like pod's containers, volumes and other data.
- Status: including current infomation about running pod such as what condition of the pods is in, the description and status of each container, pod internal IP and other basic information.

```yaml
apiVersion: v1 # API version
kind: Pod # Type of the resource
metadata:
  name: kubia-manual # Name of the pod
spec:
  containers:
    - image: luksa/kubia # Reference to Docker image
      name: kubia # Name of the container inside the pod
      ports:
        - containerPort: 8080 # Expose port 8080 of the container
```

{{% notice info %}}
Specify `containerPort` is optional, the client can connect to any container in the pod. But it useful for other people who read your YAML file
{{% /notice%}}

{{% notice tip %}}
You can explain the component in K8S using command `kubectl explain pod.spec.containers`
{{% /notice%}}

- To create pod name simply run either one of the following command:

```shell
kubectl apply -f <filename>.yaml
kubectl create -f <filename>.yaml
```

- To get the pod manifest, you can use the following command:

```shell
kubectl get pod <pod-name> -o yaml
kubectl get pod <pod-name> -o json
```

- To see logs of the pod, you can use the following command:

```shell
kubectl logs <pod-name>
```

If you are using multi container in a pod, you can use the following command to get the logs of a specific container:

```shell
kubectl logs <pod-name> -c <container-name>
```

{{%notice info%}}
Note that you can only retrieve logs of a container when the pod is still running. If the pod is terminated, the logs will be deleted. But this issue can be resolve by set up centralized, cluster-wide logging solution which will be discussed in the later section. #TODO
{{%/notice%}}

- To talk to a specific pod without go through `service` or `ingress` you can use the following command:

```shell
kubectl port-forward <pod-name> 8080:8080
```

![Port forwarding](images/_index-4.png)

#### Labels and selectors
- When the number of pods in your cluster grow, it will be useful to classify and organize them using labels.
- Label are simple, but incredible powerful, Kubernetes feature 
#### Schedule pods on specific nodes

#### Annotations

#### Namespaces to virtualy separate cluster for different teams

#### Kubectl explain
