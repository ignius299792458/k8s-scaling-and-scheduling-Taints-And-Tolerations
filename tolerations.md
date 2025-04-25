# Workout on Tolerations

Tolerations is applied within the pod configuration, configuring the tainted node's key and value

Example:

```
➜  Taints-And-Tolerations git:(main) vim pod.yml 
➜  Taints-And-Tolerations git:(main) kubectl apply -f pod.yml 
pod/nginx-pod created
➜  Taints-And-Tolerations git:(main) kubectl get pods -n tat-ns
NAME        READY   STATUS    RESTARTS   AGE
nginx-pod   1/1     Running   0          17s
➜  Taints-And-Tolerations git:(main) kubectl describe pod nginx-pod -n tat-ns
Name:             nginx-pod
Namespace:        tat-ns
Priority:         0
Service Account:  default
Node:             ignius-k8s-cluster-worker/172.18.0.4
Start Time:       Fri, 25 Apr 2025 20:32:49 +0545
Labels:           <none>
Annotations:      <none>
Status:           Running
IP:               10.244.1.13
IPs:
  IP:  10.244.1.13
Containers:
  nginx:
    Container ID:   containerd://8ebda1130b147cb83061ebfeece0ea8016ba225686c1605a90ae99517c19fecf
    Image:          nginx:latest
    Image ID:       docker.io/library/nginx@sha256:5ed8fcc66f4ed123c1b2560ed708dc148755b6e4cbd8b943fab094f2c6bfa91e
    Port:           80/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Fri, 25 Apr 2025 20:32:52 +0545
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-x95jl (ro)
Conditions:
  Type                        Status
  PodReadyToStartContainers   True 
  Initialized                 True 
  Ready                       True 
  ContainersReady             True 
  PodScheduled                True 
Volumes:
  kube-api-access-x95jl:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
                             prod=true:NoSchedule
Events:
  Type    Reason     Age   From               Message
  ----    ------     ----  ----               -------
  Normal  Scheduled  32s   default-scheduler  Successfully assigned tat-ns/nginx-pod to ignius-k8s-cluster-worker
  Normal  Pulling    32s   kubelet            Pulling image "nginx:latest"
  Normal  Pulled     30s   kubelet            Successfully pulled image "nginx:latest" in 2.157s (2.157s including waiting). Image size: 68844367 bytes.
  Normal  Created    30s   kubelet            Created container: nginx
  Normal  Started    30s   kubelet            Started container nginx
➜  Taints-And-Tolerations git:(main) kubectl port-forward pod/nginx-pod -n tat-ns 8000:80
Forwarding from 127.0.0.1:8000 -> 80
Forwarding from [::1]:8000 -> 80
Handling connection for 8000

```
