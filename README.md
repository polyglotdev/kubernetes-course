# Kubernetes Course

[Course Link](https://www.udemy.com/course/kubernetes-bootcamp-kubernetes-from-zero-to-cloud/)

> 💡 Going forward, I will be refer to Kubernetes as K8's.

## Section 1

The first thing that we are going to look at is the anatomy of K8's file:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: grade-submission-portal
  labels:
    app.kubernetes.io/name: grade-submission
    app.kubernetes.io/component: frontend
    app.kubernetes.io/instance: grade-submission-portal
spec:
  containers:
    - name: grade-submission-portal
      image: rslim087/kubernetes-course-grade-submission-portal
      resources:
        requests:
          memory: 128Mi
          cpu: 200m
        limits:
          memory: 128Mi
          cpu: 200m
      ports:
        - containerPort: 5001
    - name: grade-submission-portal-health-checker
      image: rslim087/kubernetes-course-grade-submission-portal-health-checker
      resources:
        requests:
          memory: 128Mi
          cpu: 200m
        limits:
          memory: 128Mi
          cpu: 200m
```

- `apiVersion`: The version of the Kubernetes API that we are using.
- `kind`: The type of object that we are creating.
- `metadata`: The metadata for the object.
  - `name`: The name of the object.
  - `labels`: The labels for the object.
    - `app.kubernetes.io/name`: The name of the application.
    - `app.kubernetes.io/component`: The component of the application.
    - `app.kubernetes.io/instance`: The instance of the application.
- `spec`: The specification for the object.
  - `containers`: The containers that make up the object.
    - `name`: The name of the container.
    - `image`: The image that we are using for the container.
    - `resources`: The resources that we are requesting for the container.
      - `requests`: The minimum resources that we need for the container to run.
      - `limits`: The maximum resources that we are allowing for the container.

First, we define the metadata for the object; then we define the specification for the object. There is a one-to-one relationship between the pod and containerized microservice application within it.

We label the pod to distinguish it from other pods in the cluster as well so that we group into distinct groups.

> 💡 We should in the within the labels prefix the label with `app.kubernetes.io/` to avoid conflicts in third-party labels.

Memory is **the workspace where a computer stores and retrieves data for immediate use**. CPU is **the compute that performs calculations and executes instructions**.

You can throttle compute, but you cannot throttle data. So that makes CPU a compressible resource. Therefore memory is an incompressible resource. Without a limit of how much memory the pod can use, it will use all of the memory on the node. And if other containers do the same, it will cause the node to run out of memory. Pods would just start dying off.

We normally don't want to place a limit on the CPU. Remember that CPU is a compressible resource. So if the node happens to have some extra CPU we would want the application to use it.

> 🎮 A tip for when you want to understand what a field in the YAML means is to use the `kubectl explain` command.

```bash
kubectl explain pods.spec.containers
```

You can literally just follow the path of the field in the YAML file and it will explain it to you. Starting with what sort of resource it is. To bring up a table with all the resources, you can use

```bash
k api-resources
```

> At some point you will get nit-picked about the code you write and one of those spots will probably be limits. You don't want the containers to exceed the initial limit that they are allocated. So in `limits` set the memory to exactly what the `resources.requests.memory` is. The reason that we don't often set a limit on the CPU is because the kernels scheduler will automatically throttle to just use the amount of CPU defined in `resources.requests.cpu`. So when someone complains about your PR, just shoot them [this](https://kubernetes.io/docs/setup/production-environment/container-runtimes), because that's right from Kubernetes creators.

## Alias for K8s

```bash
k=kubectl
kaf='kubectl apply -f'
kca='_kca(){ kubectl "$@" --all-namespaces;  unset -f _kca; }; _kca'
kccc='kubectl config current-context'
kcdc='kubectl config delete-context'
kcgc='kubectl config get-contexts'
kcn='kubectl config set-context --current --namespace'
kcp='kubectl cp'
kcsc='kubectl config set-context'
kcuc='kubectl config use-context'
kdcj='kubectl describe cronjob'
kdcm='kubectl describe configmap'
kdd='kubectl describe deployment'
kdds='kubectl describe daemonset'
kdel='kubectl delete'
kdelcj='kubectl delete cronjob'
kdelcm='kubectl delete configmap'
kdeld='kubectl delete deployment'
kdelds='kubectl delete daemonset'
kdelf='kubectl delete -f'
kdeli='kubectl delete ingress'
kdelj='kubectl delete job'
kdelno='kubectl delete node'
kdelns='kubectl delete namespace'
kdelp='kubectl delete pods'
kdelpvc='kubectl delete pvc'
kdels='kubectl delete svc'
kdelsa='kubectl delete sa'
kdelsec='kubectl delete secret'
kdelss='kubectl delete statefulset'
kdi='kubectl describe ingress'
kdj='kubectl describe job'
kdno='kubectl describe node'
kdns='kubectl describe namespace'
kdp='kubectl describe pods'
kdpvc='kubectl describe pvc'
kdrs='kubectl describe replicaset'
kds='kubectl describe svc'
kdsa='kubectl describe sa'
kdsec='kubectl describe secret'
kdss='kubectl describe statefulset'
kecj='kubectl edit cronjob'
kecm='kubectl edit configmap'
ked='kubectl edit deployment'
keds='kubectl edit daemonset'
kei='kubectl edit ingress'
kej='kubectl edit job'
keno='kubectl edit node'
kens='kubectl edit namespace'
kep='kubectl edit pods'
kepvc='kubectl edit pvc'
kers='kubectl edit replicaset'
kes='kubectl edit svc'
kess='kubectl edit statefulset'
keti='kubectl exec -t -i'
kga='kubectl get all'
kgaa='kubectl get all --all-namespaces'
kgcj='kubectl get cronjob'
kgcm='kubectl get configmaps'
kgcma='kubectl get configmaps --all-namespaces'
kgd='kubectl get deployment'
kgda='kubectl get deployment --all-namespaces'
kgds='kubectl get daemonset'
kgdsa='kubectl get daemonset --all-namespaces'
kgi='kubectl get ingress'
kgia='kubectl get ingress --all-namespaces'
kgj='kubectl get job'
kgno='kubectl get nodes'
kgnosl='kubectl get nodes --show-labels'
kgns='kubectl get namespaces'
kgp='kubectl get pods'
kgpa='kubectl get pods --all-namespaces'
kgpall='kubectl get pods --all-namespaces -o wide'
kgpsl='kubectl get pods --show-labels'
kgpvc='kubectl get pvc'
kgpvca='kubectl get pvc --all-namespaces'
kgrs='kubectl get replicaset'
kgs='kubectl get svc'
kgsa='kubectl get svc --all-namespaces'
kgsec='kubectl get secret'
kgseca='kubectl get secret --all-namespaces'
kgss='kubectl get statefulset'
kgssa='kubectl get statefulset --all-namespaces'
kl='kubectl logs'
kl1h='kubectl logs --since 1h'
kl1m='kubectl logs --since 1m'
kl1s='kubectl logs --since 1s'
klf='kubectl logs -f'
klf1h='kubectl logs --since 1h -f'
klf1m='kubectl logs --since 1m -f'
klf1s='kubectl logs --since 1s -f'
kpf='kubectl port-forward'
krh='kubectl rollout history'
krsd='kubectl rollout status deployment'
krsss='kubectl rollout status statefulset'
kru='kubectl rollout undo'
ksd='kubectl scale deployment'
ksss='kubectl scale statefulset'
```

## Section 7 Key Takeaways

> Service Primitive in K8s abstracts away network complexity and provides a durable endpoint for accessing pods.

### Node Port Service

Allows external access to K8s network by exposing static port on node(range: 30000-32000)

```yaml
apiVersion: v1
kind: Service
metadata:
  name: grade-submission-api
spec:
  type: NodePort
  selector:
    app.kubernetes.io/instance: grade-submission-api
  ports:
    - port: 8080
      targetPort: 8080
      nodePort: 32000
```

- external request is initiated on the node's static port(nodePort: 32000)
- The request enters the cluster thru the Service's internal port(port: 8080). With nodePort services, this internal port can be any valid port number, as the external nodePort will be mapped to whatever internal port is specified
- The service specifies a target port (targetPort: 8080) that ensures the request reaches the container port on the pod.
- The NodePort service is often used when prototyping, rarely in practice.

### ClusterIP Service

```yaml
apiVersion: v1
kind: Service
metadata:
  name: backend-service
spec:
  type: ClusterIP
  selector:
    app: backend
  ports:
    - port: 8080
      targetPort: 8080
```

- Pods within the cluster can access the service using its name (backend-service) and service's internal port (port: 8080).
- The service acts as a proxy by directing the request to a matching pod using a label selector.
- The service specifies a target port (targetPort: 8080) that ensures the request reaches the container port on the pod.

## The Kubernetes Cluster

### Master Node

- **API Server**: The API server is the front-end for the Kubernetes control plane. It exposes the Kubernetes API.
- **Controller Manager**: The controller manager is a daemon that embeds the core control loops shipped with Kubernetes. Each controller tries to move the cluster state closer to the desired state.
- **Scheduler**: The scheduler watches for newly created pods with no assigned node, and selects a node for them to run on.
- **etcd**: etcd is a consistent and highly-available key value store used as Kubernetes' backing store for all cluster data.
- **Cloud Controller Manager**: The cloud controller manager lets you link your cluster into your cloud provider's API. It separates components that interact with the cloud platform from components that just interact with your cluster.

### Worker Node

- **Kubelet**: The kubelet is an agent that runs on each worker node in the cluster. It makes sure that containers are running in a Pod.
- **Kube Proxy**: Kube-proxy is a network proxy that runs on each node in your cluster, implementing part of the Kubernetes Service concept.
- **Container Runtime**: The container runtime is the software that is responsible for running containers. Examples include Docker, containerd, and CRI-O.
- **Pods**: A Pod is the basic deployable unit in Kubernetes. It represents a single instance of a running process in your cluster.
- **Containers**: Containers are the actual applications that run inside the pods. They are isolated from each other and share the same network namespace.
- **Volumes**: Volumes are used to persist data in Kubernetes. They provide a way for containers to share data and for data to survive container restarts.
- **Namespaces**: Namespaces are a way to divide cluster resources between multiple users. They provide a scope for names, allowing for resource isolation and organization.
- **Network**: The network in Kubernetes allows communication between pods, services, and external clients. It provides a way for pods to discover and communicate with each other.
- **Storage**: Storage in Kubernetes provides a way to persist data beyond the lifecycle of a pod. It includes persistent volumes, persistent volume claims, and storage classes.
- **Secrets and ConfigMaps**: Secrets and ConfigMaps are used to store sensitive information and configuration data, respectively. They provide a way to decouple configuration from application code.
- **Ingress**: Ingress is a collection of rules that allow inbound connections to reach the cluster services. It provides HTTP and HTTPS routing to services based on host and path.
- **Service Accounts**: Service accounts are used to provide an identity for processes that run in a pod. They are used to authenticate to the Kubernetes API and other services.
- **Network Policies**: Network policies are used to control the traffic flow between pods. They provide a way to define rules for ingress and egress traffic.
- **Horizontal Pod Autoscaler**: The Horizontal Pod Autoscaler automatically scales the number of pods in a deployment, replica set, or stateful set based on observed CPU utilization or other select metrics.
- **Vertical Pod Autoscaler**: The Vertical Pod Autoscaler automatically adjusts the CPU and memory requests and limits for containers in a pod based on usage.
- **Custom Resource Definitions (CRDs)**: CRDs allow you to extend Kubernetes with your own API objects. They provide a way to define custom resources and controllers.
- **Operators**: Operators are a method of packaging, deploying, and managing a Kubernetes application. They use custom resources to manage applications and their components.
- **Admission Controllers**: Admission controllers are plugins that govern and enforce how the cluster is used. They can modify or reject requests to the Kubernetes API server.
- **Metrics Server**: The metrics server is a cluster-wide aggregator of resource usage data. It collects metrics from the kubelet and exposes them via the Kubernetes API.
- **Logging and Monitoring**: Logging and monitoring are essential for observing the health and performance of your Kubernetes cluster. They provide insights into the behavior of your applications and infrastructure.
- **Service Mesh**: A service mesh is a dedicated infrastructure layer that controls service-to-service communication over a network. It provides features like traffic management, security, and observability.
- **Federation**: Federation allows you to manage multiple Kubernetes clusters as a single entity. It provides a way to deploy applications across clusters and manage resources at scale.
- **Backup and Disaster Recovery**: Backup and disaster recovery solutions are essential for protecting your data and applications in case of failures or disasters. They provide a way to restore your cluster to a previous state.
- **Security**: Security is a critical aspect of Kubernetes. It includes authentication, authorization, encryption, and network security to protect your cluster and applications.
