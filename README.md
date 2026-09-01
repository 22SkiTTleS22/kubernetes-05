Задание 1. Volume: обмен данными между контейнерами в поде
Манифесты:
[containers-data-exchange.yaml](https://github.com/22SkiTTleS22/kubernetes-05/blob/main/containers-data-exchange.yaml)
описание пода с контейнерами (kubectl describe pods data-exchange)
```bash
kubectl describe pods data-exchange
Name:             data-exchange-6db4cf4b48-vrdkm
Namespace:        default
Priority:         0
Service Account:  default
Node:             petframe/192.168.0.123
Start Time:       Tue, 01 Sep 2026 11:29:33 +0000
Labels:           app=data-exchange
                  pod-template-hash=6db4cf4b48
Annotations:      cni.projectcalico.org/containerID: 2c766c6e2a831986bc568ccc8ac03b182a12a04089414d3cf49d271154f5678b
                  cni.projectcalico.org/podIP: 10.1.0.215/32
                  cni.projectcalico.org/podIPs: 10.1.0.215/32
Status:           Running
IP:               10.1.0.215
IPs:
  IP:           10.1.0.215
Controlled By:  ReplicaSet/data-exchange-6db4cf4b48
Containers:
  busybox:
    Container ID:  containerd://ce7956c52af0291f0a12014c94a31052c20b57e966319e5431f4acfad3fc6ec2
    Image:         busybox
    Image ID:      docker.io/library/busybox@sha256:dc2d74b28e4cf8984fa52af1f39bc7c3d9c73760b41a74d629f5d11b1ab28616
    Port:          <none>
    Host Port:     <none>
    Command:
      /bin/sh
      -c
    Args:
      while true; do echo $(date) >> /shared/data.txt; sleep 5; done
    State:          Running
      Started:      Tue, 01 Sep 2026 11:29:35 +0000
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /shared from shared-data (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-f5fvt (ro)
  multitool:
    Container ID:  containerd://d63bedc63e9c89516279ca8322bd7a1f68f4d2f801c6dfb54a52c9687ec5b2de
    Image:         wbitt/network-multitool
    Image ID:      docker.io/wbitt/network-multitool@sha256:db2810fe2c8d36db074eab5d98fbf861c8ed55e0786d648d3477b3de9135632e
    Port:          <none>
    Host Port:     <none>
    Command:
      /bin/sh
      -c
    Args:
      tail -f /shared/data.txt
    State:          Running
      Started:      Tue, 01 Sep 2026 11:29:37 +0000
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /shared from shared-data (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-f5fvt (ro)
Conditions:
  Type                        Status
  PodReadyToStartContainers   True
  Initialized                 True
  Ready                       True
  ContainersReady             True
  PodScheduled                True
Volumes:
  shared-data:
    Type:       EmptyDir (a temporary directory that shares a pod's lifetime)
    Medium:
    SizeLimit:  <unset>
  kube-api-access-f5fvt:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    Optional:                false
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
```

<img width="1700" height="316" alt="image" src="https://github.com/user-attachments/assets/49877dce-9029-41e8-9bf9-6d48540e81b8" />

Задание 2. PV, PVC
Манифесты:
[pv-pvc.yaml](https://github.com/22SkiTTleS22/kubernetes-05/blob/main/pv-pvc.yaml)

Скриншоты:

2.
<img width="1697" height="306" alt="image" src="https://github.com/user-attachments/assets/dfe4f6bc-34ea-4737-8e51-bcc3fdeeac82" />
3.
<img width="1698" height="348" alt="image" src="https://github.com/user-attachments/assets/4623ffb6-2d4b-4e07-b349-e8a6fba33875" />
4.
<img width="1677" height="120" alt="image" src="https://github.com/user-attachments/assets/2a134152-da6f-482d-86b4-a22830d90a2e" />
<img width="1700" height="218" alt="image" src="https://github.com/user-attachments/assets/f99fce40-95b6-40ff-8ef6-e78b50516209" />
<img width="1255" height="244" alt="image" src="https://github.com/user-attachments/assets/8e3c04e8-58aa-4072-8a22-69655faaf5e2" />

PV перешёл в статус Released, но не удалился, так как сработала политика persistentVolumeReclaimPolicy: Retain. Защита от случайной потери данных, сохраняется хранилище и данные после удаления PVC

<img width="1257" height="275" alt="image" src="https://github.com/user-attachments/assets/3b06ae35-4770-41d6-8af5-935f7f9ff741" />

Файл остался на диске после удаления PV, так как удаление объекта PV не затрагивает физические данные ноды

Задание 3. StorageClass
Манифесты:
[sc.yaml](https://github.com/22SkiTTleS22/kubernetes-05/blob/main/sc.yaml)

Скриншоты:

<img width="1698" height="709" alt="image" src="https://github.com/user-attachments/assets/d68af70a-8b36-4ed3-92c8-bcfddddec0e7" />

