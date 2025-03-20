# Key Takeaways from Section 10

The ingress controller in Kubernetes acts as a reverse proxy, which means it sits in front of web servers and acts on their behalf to forward external HTTP requests to the appropriate internal services.

The ingress controller is responsible for routing traffic to the correct services based on the rules defined in the ingress resource.

The ingress resource is a Kubernetes resource that defines the rules for routing traffic to the appropriate services.

1. An external HTTP request arrives at the Ingress controller.
2. The controller examines the Ingress resource.
3. The controller identifies that this is an ingress resource in the grade-submission namespace.
4. It notes that the nginx Ingress controller should be used: `ingressClassName: nginx`
5. The controller then looks at the rules. In this case, there's a single rule that applies to all HTTP traffic:
    - `path: /`
    - `backend: serviceName: grade-submission-portal`
    - `servicePort: 5001`
6. The rule specifies that all paths starting with `/` should be forwarded to the `grade-submission-portal` service on port 5001.
