kubectl apply -f deploy/crd.yaml
kubectl apply -f deploy/cr.yml

kubectl get mysqls.otus.homework
NAME             AGE
mysql-instance   5m31s

kubectl describe mysqls.otus.homework mysql-instance
Name:         mysql-instance
Namespace:    default
Labels:       <none>
Annotations:  <none>
API Version:  otus.homework/v1
Kind:         MySQL
Metadata:
  Creation Timestamp:  2023-10-12T17:16:53Z
  Generation:          1
  Managed Fields:
    API Version:  otus.homework/v1
    Fields Type:  FieldsV1
    fieldsV1:
      f:metadata:
        f:annotations:
          .:
          f:kubectl.kubernetes.io/last-applied-configuration:
      f:spec:
        .:
        f:database:
        f:image:
        f:password:
        f:storage_size:
    Manager:         kubectl-client-side-apply
    Operation:       Update
    Time:            2023-10-12T17:16:53Z
  Resource Version:  219808
  UID:               2273cd86-acdb-4a2c-9857-c92cc5dd053d
Spec:
  Database:      otus-database
  Image:         mysql:5.7
  Password:      otuspassword-new
  storage_size:  1Gi
Events:          <none>



kubectl describe po mysql-operator-757cc7584c-nc5p2
Name:         mysql-operator-757cc7584c-nc5p2
Namespace:    default
Priority:     0
Node:         cl1n1ndloj5k8np6mrai-iriq/10.129.0.13
Start Time:   Thu, 12 Oct 2023 20:49:14 +0300
Labels:       name=mysql-operator
              pod-template-hash=757cc7584c
Annotations:  <none>
Status:       Running
IP:           10.112.129.33
IPs:
  IP:           10.112.129.33
Controlled By:  ReplicaSet/mysql-operator-757cc7584c
Containers:
  mysql-operator:
    Container ID:  containerd://6cb1fec2e16165ee33a8ab71c517838ddba5e79975708d5b82b4cfe58e0a5dab
    Image:         container-registry.oracle.com/mysql/community-operator:8.1.0-2.1.0
    Image ID:      container-registry.oracle.com/mysql/community-operator@sha256:d647c087961b2e729460306181fc488d14b7f515945aa816e41ac44d52d08d62
    Port:          <none>
    Host Port:     <none>
    Args:
      mysqlsh
      --log-level=@INFO
      --pym
      mysqloperator
      operator
    State:          Running
      Started:      Thu, 12 Oct 2023 20:49:15 +0300
    Ready:          True
    Restart Count:  0
    Environment:
      MYSQLSH_USER_CONFIG_HOME:                 /mysqlsh
      MYSQLSH_CREDENTIAL_STORE_SAVE_PASSWORDS:  never
    Mounts:
      /mysqlsh from mysqlsh-home (rw)
      /tmp from tmpdir (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-vpfdc (ro)
Conditions:
  Type              Status
  Initialized       True
  Ready             True
  ContainersReady   True
  PodScheduled      True
Volumes:
  mysqlsh-home:
    Type:       EmptyDir (a temporary directory that shares a pod's lifetime)
    Medium:
    SizeLimit:  <unset>
  tmpdir:
    Type:       EmptyDir (a temporary directory that shares a pod's lifetime)
    Medium:
    SizeLimit:  <unset>
  kube-api-access-vpfdc:
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
  Normal  Scheduled  28s   default-scheduler  Successfully assigned default/mysql-operator-757cc7584c-nc5p2 to cl1n1ndloj5k8np6mrai-iriq
  Normal  Pulled     27s   kubelet            Container image "container-registry.oracle.com/mysql/community-operator:8.1.0-2.1.0" already present on machine
  Normal  Created    27s   kubelet            Created container mysql-operator
  Normal  Started    27s   kubelet            Started container mysql-operator
