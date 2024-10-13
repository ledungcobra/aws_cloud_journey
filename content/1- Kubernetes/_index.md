---
title: "Kubernetes Introduction"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 1. </b> "
---

**Content:**

- [The need for a system like Kubernetes](#the-need-for-a-system-like-kubernetes)
- [Container technology](#container-technology)
- [The architecture of Kubernetes](#the-architecture-of-kubernetes)
- [Running application in Kubernetes](#running-application-in-kubernetes)
- [Microservices vs. Monolithic Applications](#microservices-vs.-monolithic-applications)
- [The Shift to Continuous Delivery](#the-shift-to-continuous-delivery)
- [Containers and the Need for Isolation](#containers-and-the-need-for-isolation)
- [Docker as a Container Platform](#docker-as-a-container-platform)
- [Alternatives to Docker](#alternatives-to-docker)
- [Kubernetes as an Operating System for the Cluster](#kubernetes-as-an-operating-system-for-the-cluster)
- [Scaling and Resource Utilization](#scaling-and-resource-utilization)
- [Self-Healing and Resilience](#self-healing-and-resilience)
- [Developer and Operations Collaboration](#developer-and-operations-collaboration)

#### The need for a system like Kubernetes

Let's compare the features of **Monolithic Application** and **Microservices Application**

| Feature              | Monolithic Application                                                                                             | Microservices Application                                                                                                                           |
| -------------------- | ------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| Architecture         | Single, tightly coupled unit. All components are interdependent and deployed as a single entity.                   | Collection of loosely coupled, independently deployable services. Services communicate with each other through well-defined APIs.                   |
| Development          | Initially simpler to develop and deploy.                                                                           | Can be more complex to develop initially. Requires careful API design and inter-service communication planning.                                     |
| Deployment           | Deployed as a single unit, simplifying initial deployment but making updates and rollouts more complex.            | Individual services can be deployed independently, allowing for more frequent updates and easier rollouts.                                          |
| Scalability          | Scaling often requires scaling the entire application, even if only specific components need additional resources. | Individual services can be scaled independently based on their specific resource requirements, leading to more efficient resource utilization.      |
| Maintenance          | Tight coupling between components makes modifications and updates risky, potentially impacting the whole system.   | Easier to maintain as changes to one service are less likely to affect others, enabling faster development cycles and reduced risk of errors.       |
| Technology Adoption  | Less flexible in adopting new technologies and frameworks. Changes may require updates throughout the application. | Greater flexibility to use different technologies for different services, allowing teams to choose the best tool for the job and foster innovation. |
| Resilience           | A failure in one component can bring down the entire application.                                                  | More resilient as a failure in one service is less likely to cascade and impact other services, leading to improved overall application stability.  |
| Monitoring/Debugging | Easier to monitor and debug as logs and metrics are centralized.                                                   | More challenging to monitor and debug in a distributed system. Requires tools and strategies for tracing requests across multiple services.         |

The Kubernetes system is designed to address the challenges and complexities of managing microservices applications.

- **Managing the Complexity of Microservices**: While microservices offer numerous advantages over monolithic architectures, they introduce complexities in terms of deployment, management, and inter-service communication. Kubernetes provides a platform that effectively addresses these challenges by automating many of the tasks involved in operating a distributed system.

- **Abstracting Away Infrastructure**: Kubernetes acts as an abstraction layer over the underlying infrastructure, allowing developers to deploy and manage applications without needing to be concerned about the specifics of the hardware or the operating system. This abstraction simplifies development and deployment, enabling greater focus on application logic rather than infrastructure concerns.

- **Automated Deployment and Scaling**: Kubernetes automates the deployment and scaling of applications, making it easy to roll out new versions and adjust the number of running instances based on demand or resource requirements. This automation reduces manual effort and allows applications to adapt dynamically to changing conditions

- **Self-Healing and Resilience**: Kubernetes continuously monitors the health of applications and automatically takes corrective actions, such as restarting failed containers or rescheduling them to healthy nodes. This self-healing capability ensures high availability and reduces the impact of failures, leading to more resilient applications.

- **Service Discovery and Load Balancing**: Kubernetes provides built-in mechanisms for service discovery and load balancing, enabling microservices to easily communicate with each other and distribute traffic efficiently. This functionality eliminates the need for developers to implement custom solutions for these tasks, simplifying development and improving application performance.

- **Declarative Management**: Kubernetes relies on a declarative approach to managing applications. Developers define the desired state of the system, and Kubernetes automatically takes the necessary steps to achieve and maintain that state. This approach simplifies configuration management and reduces the risk of errors compared to imperative approaches.

- **Improved Resource Utilization**: By automatically scheduling pods to nodes with available resources, Kubernetes optimizes resource utilization across the cluster. This efficiency can lead to significant cost savings, particularly in large-scale deployments or cloud environments.

- **Empowering Development Teams**: Kubernetes enables developers to deploy and manage their applications independently, without relying on operations teams for every task. This self-service capability streamlines development workflows and fosters greater collaboration between development and operations.

#### Container technology

- **What They Are**: The sources explain that containers are a lightweight approach to process isolation, achieving a similar outcome to virtual machines but with less overhead. Instead of running separate operating systems like VMs, containers use features of the Linux kernel, specifically Linux Namespaces and Control Groups (cgroups), to isolate processes and manage their resource usage'

- **Benefits over VMs**:
  - **Lower Overhead**: Containers are more lightweight than VMs because they share the host OS kernel and don't require booting a separate OS for each application. This efficiency allows a greater number of applications to run on the same hardware.
  - **Faster Startup**: Since containers don't involve booting a new OS, application startup is almost instantaneous.
  - **Portability**: Container images package the application and its dependencies, making them easily transferable and runnable on any machine with a compatible container runtime.
- **Platform**
  - **Docker**: is the most popular runtime, simplify build, distribute, run containerized applications.
  - **Rocket**: is alternative to Docker, it's good at security, adhere to open standard OCI format. It can also run Docker images.

In summary, Kubernetes leverages container technology to provide a robust platform for deploying, scaling, and managing containerized applications.

#### The architecture of Kubernetes

- It contains master node which host the Kubernetes Control Plane that controls and manage the whole Kubernetes system.
- Worker nodes are the machines that host the application containers.
- It's posible to run application on master node, but it's not recommended because it will affect the stability of the system.

  ![K8S Architecture](./images/k8s-architecture.png)

##### **The Control Plane**

- Controls the cluster and make it function.

- **Components**:
  - **API Server**: Exposes the Kubernetes API that allows users to interact with the cluster.
  - **Scheduler**: Responsible for distributing work across the worker nodes.
  - **Controller Manager**: Enforces cluster policies and manages various aspects of the cluster state.
  - **etcd**: A consistent and highly-available key-value store used as Kubernetes' backing store for all cluster data.

#### Running application in Kubernetes

#### Microservices vs. Monolithic Applications

#### The Shift to Continuous Delivery

#### Containers and the Need for Isolation

#### Docker as a Container Platform

#### Alternatives to Docker

#### Kubernetes as an Operating System for the Cluster

#### Scaling and Resource Utilization

#### Self-Healing and Resilience

#### Developer and Operations Collaboration
