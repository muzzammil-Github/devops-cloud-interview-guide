## How various components of Kubernetes interact when you run `kubectl apply` (Pod)

### Question  
What happens behind the scenes when you run `kubectl apply -f pod.yaml` to deploy a pod in Kubernetes?

### Short explanation of the question  
This question checks whether you understand the full control flow and the interaction between the Kubernetes components — from API request to pod scheduling and execution on a node.

---

### Answer  
When you run `kubectl apply -f pod.yaml`, the request is sent to the API server, which stores the desired state in etcd. The scheduler then selects a node for the pod, and the kubelet on that node pulls the image and starts the container using the container runtime.

---

### Detailed explanation of the answer for readers’ understanding

Let’s break it down step-by-step:

---

## 🧾 Step 1: `kubectl apply -f pod.yaml`

You define a Pod manifest like this:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: myapp
spec:
  containers:
  - name: app
    image: nginx
```

You run:

```bash
kubectl apply -f pod.yaml
```

This sends a REST request to the Kubernetes API server.

---

## 🔐 Step 2: API Server receives and validates the request

- The `kube-apiserver` is the front-end of the control plane.
- It authenticates and validates the request.
- If valid, it stores the desired state of the Pod object in **etcd**, the cluster’s key-value store.

---

## 🧠 Step 3: Controllers monitor etcd state

- The **scheduler** constantly watches for unscheduled pods via the API server.
- It sees that your pod has no assigned node.

---

## ⚖️ Step 4: Scheduler picks a suitable node

- It considers available resources, taints, affinities, and other rules.
- It assigns the pod to a specific node by updating the pod spec with `nodeName`.

---

## 🔧 Step 5: kubelet on the selected node takes over

- The **kubelet** on that node:
  - Sees the updated pod spec.
  - Pulls the `nginx` image from the registry.
  - Uses the **container runtime** (like containerd) to start the container.

---

## 🔌 Step 6: kube-proxy handles networking

- **kube-proxy** sets up networking rules to route traffic to the pod if it's part of a Service.
- DNS resolution (via CoreDNS) also becomes available.

---

## ✅ Step 7: Pod is now running

You can check it using:

```bash
kubectl get pods
kubectl describe pod myapp
```

---

### 🔄 Summary of Interactions

| Component        | Role in the Flow |
|------------------|------------------|
| `kubectl`        | Sends request to API |
| `kube-apiserver` | Validates & stores the object |
| `etcd`           | Stores desired state |
| `kube-scheduler` | Chooses node for pod |
| `kubelet`        | Pulls image and starts container |
| `containerd`     | Runs the actual container |
| `kube-proxy`     | Sets up network rules |
| `CoreDNS`        | Resolves internal DNS |

---

### 🧠 Real-world Insight

> “We faced an issue where pods remained in `Pending` state. On investigating, we found that the scheduler couldn’t place the pod due to node taints. Understanding this internal flow helped us fix the issue quickly.”

---

### Key takeaway

> "Running `kubectl apply` kicks off a coordinated flow involving the API server, etcd, scheduler, kubelet, and container runtime — all working together to ensure your pod reaches its desired state."
> 

--------------

So when user makes a request using cube CTL to create a pod on the Kubernetes cluster.

Initially, user request is sent to API server component of Kubernetes.

API server is part of the control plane and API server validates it.

Performs authentication and authorization to see if user has the right permissions to apply configuration

on the cluster.

If user has the permissions.

If user is authorized to create a pod, then API server forwards the request to scheduler.

The scheduler identifies which node of the Kubernetes cluster is right for the pod.

So scheduler looks at the pod specification, understands if there is any node affinity that is set,

or any node label selector Elector or any tolerations on the board.

Similarly, it looks for taints on the nodes, and it makes a decision which node of the Kubernetes

cluster is right for the pod.

It also takes into account which are the free nodes on the cluster.

Finally, it makes a decision on which node the pod has to be scheduled.

Now, once this decision is made, API server talks to the kubelet of that particular node and Kubelet

invokes container runtime.

So container runtime is needed to run the container.

Typically within your pod, a container has to be executed or a container has to be run.

So container runtime takes care of running the container.

And this is how your pod is run on the Kubernetes cluster.

And once the pod is run API server updates that information onto etcd, or it persists the object onto

etcd.

And as long as your pod is running fine, it's perfect.

For some reason, if your pod goes down, there is a replica set controller which takes care of the

pod, and this is managed by Controller manager component of Kubernetes.

So these are the various components of Kubernetes and how they interact.

When you run kubectl create or kubectl apply to deploy a pod onto Kubernetes.

I'll also provide information in the description.

You will find the notes.

And you can also find this architecture diagram in the GitHub repository.

So this is how you explain when interviewer asks you how various components of Kubernetes interact to

run a pod on Kubernetes cluster.
