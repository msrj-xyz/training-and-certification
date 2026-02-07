# Domain 4: Storage (10%)

## Task 4.1: Implement Storage Classes and Dynamic Provisioning ⭐

### Storage Class

```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: fast-storage
provisioner: kubernetes.io/aws-ebs
parameters:
  type: gp3
  fsType: ext4
reclaimPolicy: Delete
allowVolumeExpansion: true
volumeBindingMode: WaitForFirstConsumer
```

### Storage Class Parameters

| Parameter | Description |
|-----------|-------------|
| **provisioner** | Storage backend (CSI driver) |
| **reclaimPolicy** | Delete or Retain |
| **volumeBindingMode** | Immediate or WaitForFirstConsumer |
| **allowVolumeExpansion** | Enable volume resize |

### Common Provisioners

| Provisioner | Platform |
|-------------|----------|
| **kubernetes.io/aws-ebs** | AWS EBS |
| **kubernetes.io/gce-pd** | GCE Persistent Disk |
| **kubernetes.io/azure-disk** | Azure Disk |
| **kubernetes.io/no-provisioner** | Local volumes |

### Get Storage Classes

```bash
# List storage classes
kubectl get storageclass

# Get default storage class
kubectl get sc -o wide

# Describe storage class
kubectl describe sc fast-storage
```

---

## Task 4.2: Configure Volume Types ⭐

### Volume Types Overview

| Type | Description | Persistence |
|------|-------------|-------------|
| **emptyDir** | Temporary pod-level storage | Lost when pod deleted |
| **hostPath** | Node filesystem path | Node-dependent |
| **persistentVolumeClaim** | Abstracted storage | Persistent |
| **configMap** | Config data as volume | ConfigMap-based |
| **secret** | Secret data as volume | Secret-based |
| **nfs** | Network file system | External NFS |

### emptyDir Volume

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: emptydir-pod
spec:
  containers:
  - name: app
    image: nginx
    volumeMounts:
    - name: cache
      mountPath: /cache
  volumes:
  - name: cache
    emptyDir:
      sizeLimit: 500Mi
      medium: Memory  # Optional: use RAM
```

### hostPath Volume

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: hostpath-pod
spec:
  containers:
  - name: app
    image: nginx
    volumeMounts:
    - name: data
      mountPath: /data
  volumes:
  - name: data
    hostPath:
      path: /var/data
      type: DirectoryOrCreate
```

### hostPath Types

| Type | Behavior |
|------|----------|
| **DirectoryOrCreate** | Create dir if not exists |
| **Directory** | Dir must exist |
| **FileOrCreate** | Create file if not exists |
| **File** | File must exist |

---

## Task 4.3: Access Modes and Reclaim Policies

### Access Modes

| Mode | Abbreviation | Description |
|------|--------------|-------------|
| **ReadWriteOnce** | RWO | Single node read-write |
| **ReadOnlyMany** | ROX | Multiple nodes read-only |
| **ReadWriteMany** | RWX | Multiple nodes read-write |
| **ReadWriteOncePod** | RWOP | Single pod read-write |

### Reclaim Policies

| Policy | Description |
|--------|-------------|
| **Retain** | Keep PV after PVC deleted (manual cleanup) |
| **Delete** | Delete PV when PVC deleted |
| **Recycle** | (Deprecated) Basic scrub |

---

## Task 4.4: Persistent Volumes and Claims ⭐⭐

### Persistent Volume (PV)

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-nfs
spec:
  capacity:
    storage: 10Gi
  accessModes:
    - ReadWriteMany
  persistentVolumeReclaimPolicy: Retain
  storageClassName: nfs-storage
  nfs:
    server: nfs-server.example.com
    path: /exports/data
```

### Persistent Volume Claim (PVC)

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 5Gi
  storageClassName: fast-storage
```

### Pod Using PVC

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pvc-pod
spec:
  containers:
  - name: app
    image: nginx
    volumeMounts:
    - name: data
      mountPath: /usr/share/nginx/html
  volumes:
  - name: data
    persistentVolumeClaim:
      claimName: my-pvc
```

---

### PV/PVC Binding

| PV State | Description |
|----------|-------------|
| **Available** | Ready for binding |
| **Bound** | Bound to a PVC |
| **Released** | PVC deleted, waiting reclaim |
| **Failed** | Reclaim failed |

### Commands

```bash
# List PVs
kubectl get pv

# List PVCs
kubectl get pvc -A

# Describe PV
kubectl describe pv pv-nfs

# Describe PVC
kubectl describe pvc my-pvc

# Check binding
kubectl get pv,pvc
```

---

## Task 4.5: Expand Persistent Volumes

```bash
# Edit PVC to expand (if allowVolumeExpansion: true)
kubectl patch pvc my-pvc -p '{"spec":{"resources":{"requests":{"storage":"10Gi"}}}}'

# Or edit directly
kubectl edit pvc my-pvc
```

> **Exam Tip:** Not all storage classes support volume expansion!

---

## Task 4.6: Static Provisioning

### Create PV Manually

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: local-pv
spec:
  capacity:
    storage: 10Gi
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  storageClassName: local-storage
  local:
    path: /mnt/disks/ssd1
  nodeAffinity:
    required:
      nodeSelectorTerms:
      - matchExpressions:
        - key: kubernetes.io/hostname
          operator: In
          values:
          - worker-node-1
```

### Bind Specific PV to PVC

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: specific-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 10Gi
  storageClassName: local-storage
  volumeName: local-pv  # Bind to specific PV
```

---

## Task 4.7: Storage in StatefulSets

```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: web
spec:
  serviceName: nginx
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx
        volumeMounts:
        - name: data
          mountPath: /usr/share/nginx/html
  volumeClaimTemplates:
  - metadata:
      name: data
    spec:
      accessModes: ["ReadWriteOnce"]
      storageClassName: fast-storage
      resources:
        requests:
          storage: 1Gi
```

> **Exam Tip:** Each StatefulSet pod gets its own PVC from volumeClaimTemplates!

---

## Exam Tips for Domain 4

1. **StorageClass** - Know provisioners and reclaim policies
2. **Access Modes** - RWO (single), ROX/RWX (multiple nodes)
3. **PV → PVC binding** - Capacity, access mode, storageClass must match
4. **Dynamic provisioning** - StorageClass + PVC (no manual PV)
5. **Static provisioning** - Manual PV creation required
6. **emptyDir** - Temporary, lost when pod deleted
7. **hostPath** - Node-specific, not for production
8. **volumeClaimTemplates** - For StatefulSets
9. **Reclaim policy** - Retain keeps data, Delete removes it
10. **Volume expansion** - Requires allowVolumeExpansion: true
