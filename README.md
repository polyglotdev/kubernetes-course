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
      - `requests`: The minimum resources that we are requesting for the container.
      - `limits`: The maximum resources that we are allowing for the container.

First, we define the metadata for the object; then we define the specification for the object. There is a one-to-one relationship between the pod and containerized microservice application within it.

We label the pod to distinguish it from other pods in the cluster as well so that we group into distinct groups.

> 💡 We should in the within the labels prefix the label with `app.kubernetes.io/` to avoid conflicts in third-party labels.
