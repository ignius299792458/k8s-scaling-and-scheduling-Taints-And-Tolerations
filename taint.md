# Workout with Taint

If any worker/node are tainted, pods aren't allowed to be applied on them

```
➜  Taints-And-Tolerations git:(main) ✗ kubectl get nodes
NAME                               STATUS   ROLES           AGE   VERSION
ignius-k8s-cluster-control-plane   Ready    control-plane   9d    v1.32.2
ignius-k8s-cluster-worker          Ready    <none>          9d    v1.32.2
ignius-k8s-cluster-worker2         Ready    <none>          9d    v1.32.2
ignius-k8s-cluster-worker3         Ready    <none>          9d    v1.32.2

➜  Taints-And-Tolerations git:(main) ✗ kubectl taint node ignius-k8s-cluster-worker prod=true:NoSchedule
node/ignius-k8s-cluster-worker tainted

➜  Taints-And-Tolerations git:(main) ✗ kubectl get nodes
NAME                               STATUS   ROLES           AGE   VERSION
ignius-k8s-cluster-control-plane   Ready    control-plane   9d    v1.32.2
ignius-k8s-cluster-worker          Ready    <none>          9d    v1.32.2
ignius-k8s-cluster-worker2         Ready    <none>          9d    v1.32.2
ignius-k8s-cluster-worker3         Ready    <none>          9d    v1.32.2

➜  Taints-And-Tolerations git:(main) ✗ kubectl taint node ignius-k8s-cluster-worker2 prod=true:NoSchedule
node/ignius-k8s-cluster-worker2 tainted

➜  Taints-And-Tolerations git:(main) ✗ kubectl taint node ignius-k8s-cluster-worker3 prod=true:NoSchedule
node/ignius-k8s-cluster-worker3 tainted

➜  Taints-And-Tolerations git:(main) ✗ kubectl apply -f taint-test-pod.yml
Error from server (NotFound): error when creating "taint-test-pod.yml": namespaces "tat-ns" not found

➜  Taints-And-Tolerations git:(main) ✗ kubectl apply -f namespace.yml 
namespace/tat-ns created

➜  Taints-And-Tolerations git:(main) ✗ kubectl apply -f taint-test-pod.yml
pod/nginx-pod created

➜  Taints-And-Tolerations git:(main) ✗ kubectl get pods -n tat-ns
NAME        READY   STATUS    RESTARTS   AGE
nginx-pod   0/1     Pending   0          13s

➜  Taints-And-Tolerations git:(main) ✗ kubectl get pods -n tat-ns
NAME        READY   STATUS    RESTARTS   AGE
nginx-pod   0/1     Pending   0          15s

➜  Taints-And-Tolerations git:(main) ✗ kubectl get pods -n tat-ns
NAME        READY   STATUS    RESTARTS   AGE
nginx-pod   0/1     Pending   0          16s

➜  Taints-And-Tolerations git:(main) ✗ echo "Since all nodes are tainted, no where to run pod, remain Pending"
Since all nodes are tainted, no where to run pod, remain Pending

➜  Taints-And-Tolerations git:(main) kubectl get pods -n tat-ns
NAME        READY   STATUS    RESTARTS   AGE
nginx-pod   0/1     Pending   0          5m49s

➜  Taints-And-Tolerations git:(main) kubectl describe pod nginx-pod -n tat-ns
Name:             nginx-pod
Namespace:        tat-ns
Priority:         0
Service Account:  default
Node:             <none>
Labels:           <none>
Annotations:      <none>
Status:           Pending
IP:               
IPs:              <none>
Containers:
  nginx:
    Image:        nginx:latest
    Port:         80/TCP
    Host Port:    0/TCP
    Environment:  <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-xpw9b (ro)
Conditions:
  Type           Status
  PodScheduled   False 
Volumes:
  kube-api-access-xpw9b:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type     Reason            Age   From               Message
  ----     ------            ----  ----               -------
Warning  FailedScheduling  6m7s  default-scheduler  0/4 nodes are available: 1 node(s) had untolerated taint {node-role.kubernetes.io/control-plane: }, 3 node(s) had untolerated taint {prod: true}. preemption: 0/4 nodes are available: 4 Preemption is not helpful for scheduling.
  Warning  FailedScheduling  65s   default-scheduler  0/4 nodes are available: 1 node(s) had untolerated taint {node-role.kubernetes.io/control-plane: }, 3 node(s) had untolerated taint {prod: true}. preemption: 0/4 nodes are available: 4 Preemption is not helpful for scheduling.

➜  Taints-And-Tolerations git:(main) kubectl taint node ignius-k8s-cluster-worker3 prod=true:NoSchedule-
node/ignius-k8s-cluster-worker3 untainted

➜  Taints-And-Tolerations git:(main) kubectl get pods -n tat-ns                                         
NAME        READY   STATUS    RESTARTS   AGE
nginx-pod   1/1     Running   0          8m6s

➜  Taints-And-Tolerations git:(main) echo "nginx-pod is running due removal of one tainted pod"
nginx-pod is running due removal of one tainted pod

➜  Taints-And-Tolerations git:(main) 

```
