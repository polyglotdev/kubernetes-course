# Kubernetes Coure

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

We normally don't want to place a limit on the CPU. Rememmber that CPU is a compressible resource. So if the node happens to have some extra CPU we would want the application to use it.

> 🎮 A tip for when you want to understand what a field in the YAML means is to use the `kubectl explain` command.

```bash
kubectl explain pods.spec.containers
```

You can literally just follow the path of the field in the YAML file and it will explain it to you. Starting with what sort of resource it is. To bring up a table with all the resources, you can use

```bash
k api-resources
```

> At some point you will get nit-picked about the code you write and one of those spots will probably be limits. You don't want the containers to exceed the inital limit that they are allocated. So in `limits` set the memory to exactly what the `resources.requests.memory` is. The reason that we don't often set a limit on the CPU is because the kernels scheduler will automatically throttle to just use the amount of CPU defined in `resources.requests.cpu`. So when someone complains about your PR, just shoot them [this](https://kubernetes.io/docs/setup/production-environment/container-runtimes), because that's right from Kubernetes creators.
