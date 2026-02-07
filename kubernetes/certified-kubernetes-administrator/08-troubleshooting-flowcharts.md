# CKA Troubleshooting Flowcharts

## Cluster / Node Troubleshooting

```mermaid
flowchart TD
    A[Node Issue] --> B{Node Status?}
    
    B -->|NotReady| C[Check kubelet]
    C --> C1[systemctl status kubelet]
    C1 --> C2{Running?}
    C2 -->|No| C3[systemctl start kubelet<br>Check journalctl -u kubelet]
    C2 -->|Yes| C4{Network CNI<br>Working?}
    C4 -->|No| C5[Check CNI pods in kube-system]
    C4 -->|Yes| C6{Certificate<br>Issues?}
    C6 -->|Yes| C7[Renew certs:<br>kubeadm certs renew all]
    
    B -->|SchedulingDisabled| D[Uncordon Node]
    D --> D1[kubectl uncordon node]
    
    B -->|MemoryPressure| E[Check Resources]
    E --> E1[Free up memory<br>or add resources]
    
    B -->|DiskPressure| F[Check Disk]
    F --> F1[Clean up disk<br>or expand storage]
    
    style A fill:#ff6b6b
    style C3 fill:#51cf66
    style C5 fill:#51cf66
    style C7 fill:#51cf66
    style D1 fill:#51cf66
    style E1 fill:#51cf66
    style F1 fill:#51cf66
```

---

## Control Plane Troubleshooting

```mermaid
flowchart TD
    A[Control Plane Issue] --> B{kubectl works?}
    
    B -->|No| C[Check API Server]
    C --> C1[Check manifest:<br>/etc/kubernetes/manifests/kube-apiserver.yaml]
    C1 --> C2{YAML Valid?}
    C2 -->|No| C3[Fix YAML syntax]
    C2 -->|Yes| C4{etcd Running?}
    C4 -->|No| C5[Check etcd manifest<br>and certificates]
    C4 -->|Yes| C6{Certs Valid?}
    C6 -->|No| C7[kubeadm certs renew]
    
    B -->|Yes| D{Pods Pending?}
    D -->|Yes| E[Check Scheduler]
    E --> E1[kubectl logs -n kube-system kube-scheduler-*]
    E1 --> E2{Running?}
    E2 -->|No| E3[Check scheduler manifest]
    
    D -->|No| F{Deployments not updating?}
    F -->|Yes| G[Check Controller Manager]
    G --> G1[kubectl logs -n kube-system kube-controller-manager-*]
    
    style A fill:#ff6b6b
    style C3 fill:#51cf66
    style C5 fill:#51cf66
    style C7 fill:#51cf66
    style E3 fill:#51cf66
    style G1 fill:#74c0fc
```

---

## etcd Troubleshooting

```mermaid
flowchart TD
    A[etcd Issue] --> B{etcd Running?}
    
    B -->|No| C[Check etcd Pod]
    C --> C1[Check /etc/kubernetes/manifests/etcd.yaml]
    C1 --> C2{Paths Correct?}
    C2 -->|No| C3[Fix data-dir and<br>certificate paths]
    C2 -->|Yes| C4{Permissions OK?}
    C4 -->|No| C5[Fix file permissions]
    
    B -->|Yes| D{Data Corruption?}
    D -->|Yes| E[Restore from Backup]
    E --> E1[ETCDCTL_API=3 etcdctl snapshot restore]
    
    D -->|No| F{Cluster Health?}
    F -->|Unhealthy| G[Check Member List]
    G --> G1[etcdctl member list]
    G1 --> G2{All Members<br>Healthy?}
    G2 -->|No| G3[Remove/re-add unhealthy member]
    
    style A fill:#ff6b6b
    style C3 fill:#51cf66
    style C5 fill:#51cf66
    style E1 fill:#51cf66
    style G3 fill:#51cf66
```

---

## Pod Troubleshooting

```mermaid
flowchart TD
    A[Pod Issue] --> B{Pod Status?}
    
    B -->|Pending| C[Check Scheduling]
    C --> C1{Resources?}
    C1 -->|Insufficient| C2[Reduce requests or add nodes]
    C1 -->|OK| C3{PVC Bound?}
    C3 -->|No| C4[Check PV/StorageClass]
    C3 -->|Yes| C5{Taints/Tolerations?}
    C5 -->|Mismatch| C6[Add tolerations]
    
    B -->|ImagePullBackOff| D[Check Image]
    D --> D1{Image exists?}
    D1 -->|No| D2[Fix image name:tag]
    D1 -->|Yes| D3{Private registry?}
    D3 -->|Yes| D4[Add imagePullSecrets]
    
    B -->|CrashLoopBackOff| E[Check Logs]
    E --> E1[kubectl logs pod --previous]
    E1 --> E2{App Error?}
    E2 -->|Yes| E3[Fix application]
    E2 -->|No| E4{OOM?}
    E4 -->|Yes| E5[Increase memory limit]
    
    B -->|Error| F[Describe Pod]
    F --> F1[kubectl describe pod]
    F1 --> F2[Check Events section]
    
    style A fill:#ff6b6b
    style C2 fill:#51cf66
    style C4 fill:#51cf66
    style C6 fill:#51cf66
    style D2 fill:#51cf66
    style D4 fill:#51cf66
    style E3 fill:#51cf66
    style E5 fill:#51cf66
    style F2 fill:#74c0fc
```

---

## Service Troubleshooting

```mermaid
flowchart TD
    A[Service Issue] --> B{Endpoints exist?}
    
    B -->|No| C[Check Selector]
    C --> C1{Labels match?}
    C1 -->|No| C2[Fix pod labels<br>or service selector]
    C1 -->|Yes| C3{Pods Ready?}
    C3 -->|No| C4[Fix pod issues first]
    
    B -->|Yes| D{Can connect?}
    D -->|No| E[Check Port]
    E --> E1{targetPort correct?}
    E1 -->|No| E2[Fix targetPort]
    E1 -->|Yes| E3{App listening?}
    E3 -->|No| E4[Fix app port config]
    
    D -->|Yes| F{External access?}
    F -->|NodePort| G[Check firewall<br>and port range]
    F -->|LoadBalancer| H[Check cloud LB<br>and health checks]
    F -->|Ingress| I[Check ingress controller<br>and rules]
    
    style A fill:#ff6b6b
    style C2 fill:#51cf66
    style C4 fill:#ffd43b
    style E2 fill:#51cf66
    style E4 fill:#51cf66
    style G fill:#51cf66
    style H fill:#51cf66
    style I fill:#51cf66
```

---

## Quick Diagnostic Commands

| Issue | Command |
|-------|---------|
| **Node status** | `kubectl get nodes -o wide` |
| **Node describe** | `kubectl describe node <node>` |
| **kubelet logs** | `journalctl -u kubelet -f` |
| **API server logs** | `kubectl logs -n kube-system kube-apiserver-*` |
| **etcd health** | `ETCDCTL_API=3 etcdctl endpoint health` |
| **Pod events** | `kubectl describe pod <pod>` |
| **Pod logs** | `kubectl logs <pod> --previous` |
| **Service endpoints** | `kubectl get endpoints <svc>` |
| **All events** | `kubectl get events --sort-by='.lastTimestamp'` |
| **Component status** | `kubectl get pods -n kube-system` |
