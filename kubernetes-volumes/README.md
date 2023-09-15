```bash
kubectl get statefulsets
NAME    READY   AGE
minio   1/1     4m30s

kubectl get po
NAME      READY   STATUS    RESTARTS   AGE
minio-0   1/1     Running   0          4s

kubectl get svc
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)    AGE
minio        ClusterIP   None         <none>        9000/TCP   4m52s

kubectl get pvc
NAME           STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
data-minio-0   Bound    pvc-05ed866e-8023-488b-8e29-8577ead3566a   10Gi       RWO            standard       6m56s

kubectl get pv
NAME                                       CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                  STORAGECLASS   REASON   AGE
pvc-05ed866e-8023-488b-8e29-8577ead3566a   10Gi       RWO            Delete           Bound    default/data-minio-0   standard                7m16s

kubectl describe po minio-0
Name:         minio-0
Namespace:    default
Priority:     0
Node:         minikube-m02/192.168.49.3
Start Time:   Sat, 30 Sep 2023 23:31:20 +0300
Labels:       app=minio
              controller-revision-hash=minio-7cf67fcb75
              statefulset.kubernetes.io/pod-name=minio-0
Annotations:  cni.projectcalico.org/containerID: ba48e8baf2e2a8b9f9ce23dd55191d256b04cd02676ed4deb686bafdb14d5190
              cni.projectcalico.org/podIP: 10.244.205.198/32
              cni.projectcalico.org/podIPs: 10.244.205.198/32
Status:       Running
IP:           10.244.205.198
IPs:
  IP:           10.244.205.198
Controlled By:  StatefulSet/minio
Containers:
  minio:
    Container ID:  docker://87629b56499e1d3128d0f15b1fc319d892e7da81b5fbef87315d119fa8853265
    Image:         minio/minio:latest
    Image ID:      docker-pullable://minio/minio@sha256:6262bc9a2730eeaf16be1bf436a3c2bca2ab76639f113778601a9f89c1485b56
    Port:          9000/TCP
    Host Port:     0/TCP
    Args:
      server
      /data
    State:          Running
      Started:      Sat, 30 Sep 2023 23:31:22 +0300
    Ready:          True
    Restart Count:  0
    Liveness:       http-get http://:9000/minio/health/live delay=120s timeout=1s period=20s #success=1 #failure=3
    Environment:
      MINIO_ACCESS_KEY:  <set to the key 'username' in secret 'minio-secret'>  Optional: false
      MINIO_SECRET_KEY:  <set to the key 'password' in secret 'minio-secret'>  Optional: false
    Mounts:
      /data from data (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-fz57r (ro)
Conditions:
  Type              Status
  Initialized       True
  Ready             True
  ContainersReady   True
  PodScheduled      True
Volumes:
  data:
    Type:       PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
    ClaimName:  data-minio-0
    ReadOnly:   false
  kube-api-access-fz57r:
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
  Type    Reason     Age   From               Message
  ----    ------     ----  ----               -------
  Normal  Scheduled  8s    default-scheduler  Successfully assigned default/minio-0 to minikube-m02
  Normal  Pulling    7s    kubelet            Pulling image "minio/minio:latest"
  Normal  Pulled     6s    kubelet            Successfully pulled image "minio/minio:latest" in 1.344392329s (1.344416265s including waiting)
  Normal  Created    6s    kubelet            Created container minio
  Normal  Started    5s    kubelet            Started container minio

kubectl describe svc minio
Name:              minio
Namespace:         default
Labels:            app=minio
Annotations:       <none>
Selector:          app=minio
Type:              ClusterIP
IP Family Policy:  SingleStack
IP Families:       IPv4
IP:                None
IPs:               None
Port:              minio  9000/TCP
TargetPort:        9000/TCP
Endpoints:         10.244.205.198:9000
Session Affinity:  None
Events:            <none>
```