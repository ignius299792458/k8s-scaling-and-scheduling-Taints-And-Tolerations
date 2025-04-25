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

➜  Taints-And-Tolerations git:(main) ✗ kubectl apply -f pod.yml 
Error from server (NotFound): error when creating "pod.yml": namespaces "tat-ns" not found

➜  Taints-And-Tolerations git:(main) ✗ kubectl apply -f namespace.yml 
namespace/tat-ns created

➜  Taints-And-Tolerations git:(main) ✗ kubectl apply -f pod.yml 
pod/nginx-pod created

➜  Taints-And-Tolerations git:(main) ✗ kubectl get pods
No resources found in default namespace.

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

➜  Taints-And-Tolerations git:(main) ✗ 

```
