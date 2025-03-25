# Dom's Notes on Kubernetes

When I started this course, I did the thing that I tell other people not to do of just blindly following the course. Never having asked how and why because if you don't then you become dependant on tutorial based learning which will break you as an engineer because not everything that you do is going to come with a tutorial. So lets just run down the things that we have learned thus far.

## What is Kubernetes?

The standard answer here is going to be: Kubernetes is a orchestration platform for containers. To me this answer is greatly lacking any real context giving me a definition without telling me not just what it is, but what it does.

## What it means to Orchestrate Containers

Imagine you have a band of musicians(containers) each playing their own piece of music(running a service). Now if every musician just does their own thing, the result would be chaos. Kubernetes as a the conductor that:

- Schedules and Deploys: It decides which machine (node) gets which container, ensuring that the best use of your available resources is made. This means no single server is overwhelmed while another is sitting idle.
- Manages Scaling: When you app's popularity surges, or takes a nap for that matter, Kubernetes will automatically start or stop containers to match the demand. It's like having a extra band members that are waiting to jump in when the original members need a break.
- Ensure Self-Healing: If a container crashes, Kubernetes will detect the problem and spins up a replacement automatically. Your system stays on beat even when the individual parts falter.
- Provides Load Balancing: It routes traffic to the right container, ensuring that requests are handled smoothly-- no one is stuck in lobby waiting to get into the auditorium.
- Simplifies Rollouts and Rollbacks: Need to update the application? Kubernetes can deploy new versions incrementally and revert if something goes sideways, keeping your performance in check.

> 🧠 Going forward we will use K8s as a abbreviation for the word Kubernetes.

## Kubernetes Architecture

### 1. The Control Plane

This is the brain of the cluster. Its where decisions are made and the state of your entire cluster is managed.

- API Sever: Acts like the entry point for K8s. Its a RESTful interface through which you and other components communicate with the cluster. Every request -- be it from the CLI, a Dashboard, or a custom application -- goes through the API server. Imagine this as the stage manager for the band.
- etcd: A lightweight key-value store that holds the entire state of the cluster. Think of this as the memory-bank for the band that records all actions/interactions between the conductor and the musicians. etcd is the reliable historian.
- Scheduler: Decides which node should run which container. It's like the conductor who assigns the right musicians to the right instrument.
- Controller Manager: Runs a bunch og controllers that watch the state of your cluster. Its the stage crew that ensures that the set up of chairs and stands are correct and is constantly monitoring to ensure that the stage is in the desired state.

### 2. The Node(worker) Components

These are the workhorses where your containers actually run. Every worker node runs several key components:

- Kubelet: An agent that runs on each node. It listens for instructions from the API server and ensures that the containers are running(encapsulated by a pod) in the desired state.
- Kube-proxy: Maintains network rules on nodes, allowing communication between pods. Think of it as the traffic cop directing the flow of data in and out of the cluster.
- Container Runtime: The software that runs the containers. It's like the engine that powers the containers. Think Docker or CRI-O.

### 3. Other Key Components

- **pods**: The smallest deployable unit in K8s which can host one or more containers that share the same context (networking, storage, etc).
- **services**: abstract a set of pods and provide a stable network endpoint. Services ensure that even if your pods come and go, clients can still reliably access the application. Its like having a permanent marquee outside the auditorium that always points to the current show.
- **Deployments**, **ReplicaSets** and **StatefulSets**: These are higher-level constructs that manage the lifecycle of your pods. They handle things like scaling, rolling updates, and storage. Think of them as the conductor's score that guides the musicians through the piece. They are like the sound engineer that from the board can handle who is at what volume and lower and raise the volume so the product is always right where you want it.
- **ConfigMaps** and **Secrets**: This is the band manager that handles all the logistics for the band's pay and what not. They store all the sensitive information about the members as well as the set list. So we can update the set list and the band manager will ensure that the correct song is played.
- **Volumes**: offer persistent storage that lives beyond the lifecycle of the containers. They ensure that data ins't lost when a container crashes--akin to having a safe place where the band's priceless memorabilia is stored.

## The Mechanics in Action

1. You send a request to the **API server**, which updates the etcd database.
2. The **scheduler** then picks a node based on resource availability.
3. **kublet** on the chosen node picks up the instructions, ensuring that the pod is created.
4. **kube-proxy** sets up networking so your service can be accessed.
5. **Controllers** continuously monitor the state, making sure that the actual state of the cluster matches the desired state--even if a container goes down unexpectedly.
