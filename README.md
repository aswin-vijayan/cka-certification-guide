# Ultimate Certified Kubernetes Administrator (CKA) Preparation Guide - V1.31 (2024)

This guide is part of the [Complete CKA Certification Course](https://techiescamp.com/p/certified-kubernetes-administrator-course)

## CKA Exam Overview

The Certified Kubernetes Administrator (CKA) exam has a duration of 2 hours.
To pass the exam, candidates need to achieve a score of at least 66%.
The exam will be on Kubernetes version 1.31.
Once the certificate is earned, the CKA certification remains valid for 2 years. The cost to take the exam is $395 USD.

>**Important Note:** The CKA exam is updating after November 25, 2024, with new topics and a focus on real-world Kubernetes skills like Gateway API, Helm, Kustomize, CRDs & Operators. This guide is based on the new CKA syllabus. You can read more about the exam changes here [CKA Exam Changes](https://blog.techiescamp.com/cka-exam-updates/)

## Table of Contents

1. [Cluster Architecture, Installation & Configuration (25%)](#1-cluster-architecture-installation--configuration-25)
   - [Manage role based access control (RBAC)](#manage-role-based-access-control-rbac)
   - [Prepare underlying infrastructure for installing a Kubernetes cluster](#prepare-underlying-infrastructure-for-installing-a-kubernetes-cluster)
   - [Create and manage Kubernetes clusters using kubeadm](#create-and-manage-kubernetes-clusters-using-kubeadm)
   - [Manage the lifecycle of Kubernetes clusters](#manage-the-lifecycle-of-kubernetes-clusters)
   - [Use Helm and Kustomize to install cluster components](#use-helm-and-kustomize-to-install-cluster-components)
   - [Understand extension interfaces (CNI, CSI, CRI, etc.)](#understand-extension-interfaces-cni-csi-cri-etc)
   - [Understand CRDs, install and configure operators](#understand-crds-install-and-configure-operators)

2. [Workloads & Scheduling (15%)](#2-workloads--scheduling-15)
   - [Understand deployments and how to perform rolling update and rollbacks](#understand-deployments-and-how-to-perform-rolling-update-and-rollbacks)
   - [Use ConfigMaps and Secrets to configure applications](#use-configmaps-and-secrets-to-configure-applications)
   - [Configure workload autoscaling](#configure-workload-autoscaling)
   - [Understand the primitives used to create robust, self-healing, application deployments](#understand-the-primitives-used-to-create-robust-self-healing-application-deployments)
   - [Configure Pod admission and scheduling (limits, node affinity, etc.)](#configure-pod-admission-and-scheduling-limits-node-affinity-etc)

3. [Storage (10%)](#3-storage-10)
   - [Implement storage classes and dynamic volume provisioning](#implement-storage-classes-and-dynamic-volume-provisioning)
   - [Configure volume types, access modes and reclaim policies](#configure-volume-types-access-modes-and-reclaim-policies)
   - [Manage persistent volumes and persistent volume claims](#manage-persistent-volumes-and-persistent-volume-claims)

4. [Services & Networking (20%)](#4-services--networking-20)
   - [Understand connectivity between Pods](#understand-connectivity-between-pods)
   - [Define and enforce Network Policies](#define-and-enforce-network-policies)
   - [Use ClusterIP, NodePort, LoadBalancer service types and endpoints](#use-clusterip-nodeport-loadbalancer-service-types-and-endpoints)
   - [Use the Gateway API to manage Ingress traffic](#use-the-gateway-api-to-manage-ingress-traffic)
   - [Know how to use Ingress controllers and Ingress resources](#know-how-to-use-ingress-controllers-and-ingress-resources)
   - [Understand and use CoreDNS](#understand-and-use-coredns)

5. [Troubleshooting (30%)](#5-troubleshooting-30)
   - [Troubleshoot clusters and nodes](#troubleshoot-clusters-and-nodes)
   - [Troubleshoot cluster components](#troubleshoot-cluster-components)
   - [Monitor cluster and application resource usage](#monitor-cluster-and-application-resource-usage)
   - [Manage and evaluate container output streams](#manage-and-evaluate-container-output-streams)
   - [Troubleshoot services and networking](#troubleshoot-services-and-networking)

## CKA Exam Detailed Study Guide & References

CKA Certification Exam has the following key domains:

## 1. Cluster Architecture, Installation & Configuration (25%)

Following are the subtopics under Cluster Architecture, Installation & Configuration

### Manage role based access control (RBAC).
> [Using RBAC Authorization](https://techiescamp.com/courses/certified-kubernetes-administrator-course/lectures/55733267) | [Roles & ClusterRoles](https://techiescamp.com/courses/certified-kubernetes-administrator-course/lectures/55997133) : Understand the difference between Roles (namespace level) and ClusterRoles (cluster level).

```bash
# Create a service account
k create sa <sa-name> -n <namespace>

# Create a role
k create role <role-name> --verb=<verbs> --resource=<resources> -n <namespace>

# Create rolebinding
k create rolebinding <binding-name> --role=<role-name> --user=<username> -n <namespace>

# Create a clusterrole
k create clusterrole <clusterrole-name> --verb=<verbs> --resource=<resources>

# Create clusterrolebinding
k create clusterrolebinding <binding-name> --clusterrole=<clusterrole-name> --user=<username>

# Check RBAC authorization
k auth can-i <verb> <resource> --as=<username>

```

### Prepare underlying infrastructure for installing a Kubernetes cluster.
> [Overview](https://kubernetes.io/docs/concepts/overview/)

### Create and manage Kubernetes clusters using kubeadm.
> [Creating a cluster with kubeadm](https://techiescamp.com/courses/certified-kubernetes-administrator-course/lectures/55288346) : kubeadm is a tool used for easy cluster bootstrap, be familiar with creating a cluster control plane node and adding worker nodes.

```bash
# Set Up kubeconfig
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

### Manage the lifecycle of Kubernetes clusters.
> [Upgrading kubeadm clusters](https://techiescamp.com/courses/certified-kubernetes-administrator-course/lectures/55120133) : Managing the lifecycle involves upgrading clusters, managing control plane nodes, and ensuring consistency across versions.

### Use Helm and Kustomize to install cluster components.
> [Helm](https://helm.sh/docs/intro/using_helm/) : Helm makes it easier to package and deploy Kubernetes applications. Practice installing, upgrading, and uninstalling releases.

```bash
# Install a helm chart
helm install <release-name> <chart-name>

# List helm releases
helm list

# Upgrade a helm release
helm upgrade <release-name> <chart-name>

# Search for a chart
helm search repo <chart-name>

# Install helm chart
helm install <release-name> <chart-name>

# Uninstall a helm release
helm uninstall <release-name>
```

> [Declarative Management of Kubernetes Objects Using Kustomize](https://kubernetes.io/docs/tasks/manage-kubernetes-objects/kustomization/) : Start by creating a directory containing all the Kubernetes manifests you want to manage with Kustomize.

```bash
# Example directory structure
example-app/
  ├── deployment.yaml
  ├── service.yaml
  └── kustomization.yaml

# Use Kustomize to apply resources
k apply -k kustomization.yaml

```

### Understand extension interfaces (CNI, CSI, CRI, etc.).
> [Container Runtime Interface (CRI)](https://kubernetes.io/docs/concepts/architecture/cri/) : Kubernetes uses the CRI to communicate with container runtimes.

```bash
# Check container runtime
crictl info

# List all containers
crictl ps

# View specific container details
crictl inspect <container-id>

# View container logs
crictl logs <container-id>

```

> [Network Plugins](https://kubernetes.io/docs/concepts/extend-kubernetes/compute-storage-net/network-plugins/) : Kubernetes uses network plugins (CNI) to manage pod networking, get a good understanding of  popular plugins like Calico, Flannel, and Weave Net, and understand the role of CNIs in providing network connectivity, security policies, and IPAM.

```bash
# List installed CNI plugins
ls /etc/cni/net.d/
```

> [Container Storage Interface (CSI) for Kubernetes GA](https://kubernetes.io/blog/2019/01/15/container-storage-interface-ga/) : The CSIs is a standardized mechanism that allows storage providers to provide persistent storage support for Kubernetes.

```bash
# List CSI drivers
k get csidrivers

```

### Understand CRDs, install and configure operators.
> [Custom Resources](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/) : CRDs allows you to extend Kubernetes APIs to create new kinds of Kubernetes objects beyond the built-in ones.

```bash
# List CRDs
k get crd

# Describe a CRD
k describe crd <crd-name>

# Delete a CRD
k delete <resource-name> <name>

```

> [Operator pattern](https://kubernetes.io/docs/concepts/extend-kubernetes/operator/) : The Operator pattern allows you to automate the lifecycle of applications running on Kubernetes by packaging operational knowledge into Kubernetes-native applications.

## 2. Workloads & Scheduling (15%)

Following are the subtopics under Workloads & Scheduling

### Understand deployments and how to perform rolling update and rollbacks.
> [Deployments](https://techiescamp.com/courses/certified-kubernetes-administrator-course/lectures/55402667) : Understand the use of --record for version history, which is crucial for rollbacks.

```bash
# Create deployment with 3 replicas
k create deploy <deployment-name> --image=<image-name> --replicas=3

# Create deployment manifest file
k create deploy <deployment-name> --image <image-name> --replicas=3  --dry-run=client -o yaml > deploy.yaml

# List deployment
k get deploy

# Check replicaSet
k get rs

# Describe the deployment
k describe deploy <deployment-name>

# Scale deployment replicas
k scale deploy <deployment-name> --replicas=2

# View Deployments Manifest file
k get deploy <deployment-name> -o yaml

# Update container image in a deployment
k set image deploy <deployment-name> nginx=<image-name> --record

# Rollback to previous deployment version
k rollout undo deploy <deployment-name>

# View deployment rollout history
k rollout history deploy <deployment-name>

# Rollback to a specific revision
k rollout undo deploy <deployment-name> --to-revision=1

# Pause deployment rollout
k rollout pause deploy <deployment-name>

# Resume deployment rollout
k rollout resume deploy <deployment-name>

# Rollout & restart a deployment
k rollout restart deploy <deployment-name>

# Delete deployment
k delete deploy <deployment-name>
```

### Use ConfigMaps and Secrets to configure applications.
> [ConfigMaps](https://techiescamp.com/courses/certified-kubernetes-administrator-course/lectures/55993034) : Use ConfigMaps to separate environment-specific configurations from the container image.

```bash
# Create configmap
k create cm <configmap-name>

# Edit the configmap
k edit cm <configmap-name>

# Create configmap manifest file
k create cm <configmap-name> --dry-run=client -o yaml > cm.yaml

# Create configmap from literal values
k create cm <configmap-name> --from-literal=<key1>=<value1> --from-literal=<key2>=<value2>

# Create configmap from a file
k create cm <configmap-name> --from-file=<file-name>
```

> [Secrets](https://techiescamp.com/courses/certified-kubernetes-administrator-course/lectures/55993473) : Remember that Secrets are base64-encoded and not encrypted and they are for storing sensitive data.

```bash
# Create generic secret from literal values
k create secret generic <secret-name> --from-literal=<key1>=<value1> --from-literal=<key2>=<value2>

# Create TLS secret from certificate files
k create secret tls <secret-name> --cert=tls.crt --key=tls.key

```

### Configure workload autoscaling.
> [Autoscaling Workloads](https://kubernetes.io/docs/concepts/workloads/autoscaling/) : Practice setting up Horizontal Pod Autoscaler (HPA).

```bash
# Using autoscaling
k autoscale deploy <deployment-name> --min=2 --max=5
```

### Understand the primitives used to create robust, self-healing, application deployments.
> [Configure Liveness, Readiness and Startup Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/) : Using liveness and readiness probes in your deployment ensure that your applications are self-healing and automatically recover from failures.

```bash
# Startup probe
startupProbe:
    httpGet:
    path: /startup
    port: 8080
    failureThreshold: 30
    periodSeconds: 10

# Liveness probe
livenessProbe:
    httpGet:
    path: /healthz
    port: 8080
    initialDelaySeconds: 15
    failureThreshold: 1
    periodSeconds: 10

# Readiness probe
readinessProbe:
    httpGet:
    path: /ready
    port: 8080
    failureThreshold: 30
    periodSeconds: 10
```

### Configure Pod admission and scheduling (limits, node affinity, etc.).
> [Assigning Pods to Nodes](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/) : Use node affinity and anti-affinity to control the placement of your pods, ensuring that workloads are distributed efficiently across nodes as per there requirements.

```bash
# Create a Pod
k run <pod-name> --image=<image-name> --restart=Never

# Add a label to a pod
k label pod <pod-name> <label-key>=<label-value>

# List pods with their labels
k get po --show-labels

# List pods with specific label
k get po --selector <label-key>=<label-value>

# Remove a label from a pod
k label po <pod-name> <label-key>-

# Create a temporary interactive pod
k run -it <pod-name> --image=<image-name> --rm --restart=Never -- sh

# Delete pod immediately
k delete po <pod-name> --grace-period 0 --force

# Pod resource limits
resources:
    requests:
    memory: "64Mi"
    cpu: "250m"
    limits:
    memory: "128Mi"
    cpu: "500m"

# Label a node
k label no <pod-name> <label-key>=<label-value>

# Node affinity
affinity:
nodeAffinity:
    requiredDuringSchedulingIgnoredDuringExecution:
    nodeSelectorTerms:
    - matchExpressions:
        - key: <label-key>
        operator: In
        values:
        - <label-value>
```

> [Admission Controllers Reference](https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/) : Use admission controllers to enforce policies such as resource quotas, pod security policies, and image validation.

## 3. Storage (10%)

Following are the subtopics under Storage

### Implement storage classes and dynamic volume provisioning.
> [Storage Classes](https://kubernetes.io/docs/concepts/storage/storage-classes/) : Understand the difference between default storage class and other classes


```bash
# List storageclasses
k get sc

# Describe storageclass
k describe sc <storageclass-name>

```
> [Dynamic Volume Provisioning](https://kubernetes.io/docs/concepts/storage/dynamic-provisioning/) : Understand which type of persistent storage is supported (like AWS EBS, GCE Persistent Disks) and practice using them.

```bash
# List persistentvolume
k get pv

# Describe persistentvolume
k describe pv <persistentvolume-name>

# Delete persistentvolume
k delete pv <persistentvolume-name>
```

### Configure volume types, access modes and reclaim policies.
> [Persistent Volumes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/) : Remember to know the different reclaim policies: Retain, Delete, and Recycle. Understand access modes like ReadWriteOnce, ReadOnlyMany.

```bash
# List persistentvolumeclaim
k get pvc

# Describe persistentvolumeclaim
k describe pvc <persistentvolumeclaim-name>

# Delete persistentvolumeclaim
k delete pvc <persistentvolumeclaim-name>
```

### Manage persistent volumes and persistent volume claims.
> [Configure a Pod to Use a PersistentVolume for Storage](https://kubernetes.io/docs/tasks/configure-pod-container/configure-persistent-volume-storage/) : Practice creating a pod with persistent storage defined in a YAML manifest. Ensure familiarity with both bindings and mounting.

## 4. Services & Networking (20%)

Following are the subtopics under Services & Networking

### Understand connectivity between Pods.
> [Cluster Networking](https://kubernetes.io/docs/concepts/cluster-administration/networking/) : Use kubectl exec to test network connectivity between pods.

```bash
# Execute shell in a pod
k exec -it <pod-name> -- /bin/sh

# Execute shell in a specific container within a pod
k exec -it <pod-name> -c webserver -- sh

# Get pod IP addresses
k get po -o wide

# Check pod-to-pod connectivity
k exec <pod-name> -- curl <target-service-ip>:<port>

# Check network interfaces inside a pod
k exec <pod-name> -- ifconfig

# Static pod manifest path
/etc/kubernetes/manifests
```

### Define and enforce Network Policies.
> [Network Policies](https://kubernetes.io/docs/concepts/services-networking/network-policies/) : Practice setting up network policies to restrict traffic flow between pods.

```bash
# List network policies
k get netpol

# Description network policies
k describe netpol <policy-name>
```

### Use ClusterIP, NodePort, LoadBalancer service types and endpoints.
> [Service](https://kubernetes.io/docs/concepts/services-networking/service/) : Practice exposing deployments using all types of services: ClusterIP, NodePort, and LoadBalancer.

```bash
# Expose deployment as a service
k expose deploy <deployment-name> --name=<service-name> --port=<port> --target-port=<container-port> --type=<service-type>

# List services
k get svc

# List service endpoints
k get ep

```
### Use the Gateway API to manage Ingress traffic.
> [Gateway API](https://kubernetes.io/docs/concepts/services-networking/gateway/) : The Gateway API provides more flexibility and extensibility compared to traditional Ingress. Use it when you need advanced traffic routing, such as assigning multiple gateways with different capabilities to different services.

### Know how to use Ingress controllers and Ingress resources.
> [Ingress Controllers](https://kubernetes.io/docs/concepts/services-networking/ingress-controllers/) : Practice creating Ingress resources with different rules to route traffic to services based on hostnames and paths.

> [Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/) : You can define multiple services under a single Ingress resource by utilizing both path-based and host-based rules.

```bash
# Example manifest file to create ingress object
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: example-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  ingressClassName: nginx-example
  rules:
  - http:
      paths:
      - path: /examplepath
        pathType: Prefix
        backend:
          service:
            name: example
            port:
              number: 80

# List all ingress resources
k get ing

# Describe an ingress resource
k describe ing <ingress-name>
```

### Understand and use CoreDNS.
> [Using CoreDNS for Service Discovery](https://kubernetes.io/docs/tasks/administer-cluster/coredns/) : CoreDNS is used for service discovery within the Kubernetes cluster. Familiarize yourself with modifying the Corefile configuration to add custom DNS behaviors like forwarding queries for specific domains outside the cluster.

```bash
# Get CoreDNS ConfigMap in the kube-system namespace
k get cm coredns -n kube-system

# Edit CoreDNS ConfigMap for custom DNS configurations
k edit cm coredns -n kube-system

# Get logs of CoreDNS pods
k logs -n kube-system -l k8s-app=kube-dns
```

## 5. Troubleshooting (30%)

Following are the subtopics under Troubleshooting

### Troubleshoot clusters and nodes.
> [Troubleshooting Clusters](https://kubernetes.io/docs/tasks/debug/debug-cluster/) : When draining a node, use --ignore-daemonsets to safely move workloads that can be moved while ignoring daemonsets.

```bash
# List all available nodes
k get no

# Describe nodes
k describe no <node-name>

# Drain a node
k drain <node-name> --ignore-daemonsets

# Cordon/uncordon a node
k cordon <node-name>
k uncordon <node-name>

```

### Troubleshoot cluster components.
> [Troubleshooting kubectl](https://kubernetes.io/docs/tasks/debug/debug-cluster/troubleshoot-kubectl/) : Make sure your kubectl is configured to connect to the correct cluster context.

> [Troubleshooting kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/troubleshooting-kubeadm/) : Monitor control plane components such as API server, etcd, controller-manager for potential issues during the cluster lifecycle and make sure proper certificate expiration and connectivity between components.

```bash
# Cluster components manifest file location
/etc/kubernetes/manifests

# Check kubectl configuration
k config view

# Get logs of kubectl
journalctl -u kubelet

# Check status of kubectl
k get --raw /healthz

# Check status of API server
k get componentstatuses

```

### Monitor cluster and application resource usage.
> [Tools for Monitoring Resources](https://kubernetes.io/docs/tasks/debug/debug-cluster/resource-usage-monitoring/) : Use kubectl top to monitor resource utilization.

```bash
# Get no cpu and memory usage
k top no

# Sort pods based on cpu utilization
k top po --sort-by=cpu

# Sort pods based on memory usage
k top po --sort-by=memory

# Get pod cpu and memory usage
k top pod

```

### Manage and evaluate container output streams.
> [Logging Architecture](https://kubernetes.io/docs/concepts/cluster-administration/logging/) : Logs are vital for understanding what's happening inside containers. Use kubectl logs to access logs from running containers.

```bash
# Get logs of a pod
k logs <pod-name>

# Get logs of a multi container pod
k logs <pod-name> -c <container-name>

# Get live logs of a pod
k logs <pod-name> -f

```

### Troubleshoot services and networking.
> [Debug Services](https://kubernetes.io/docs/tasks/debug/debug-application/debug-service/) : Practice using the describe and logs commands to inspect failed services or pods.

```bash
# Describe a pod
k describe pod <pod-name> 

# Describe a service
k describe svc <service-name>

# Check recent events in the cluster
k get events
```