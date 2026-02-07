# CKAD Troubleshooting Flowcharts

## Pod Troubleshooting

```mermaid
flowchart TD
    A[Pod Issue] --> B{Pod Status?}
    
    B -->|Pending| C[Check Scheduling]
    C --> C1{Resources<br>Available?}
    C1 -->|No| C2[Adjust requests/limits]
    C1 -->|Yes| C3{PVC Bound?}
    C3 -->|No| C4[Check PVC and StorageClass]
    C3 -->|Yes| C5{Node Selector<br>Match?}
    C5 -->|No| C6[Fix nodeSelector/affinity]
    
    B -->|ImagePullBackOff| D[Check Image]
    D --> D1{Image Exists?}
    D1 -->|No| D2[Fix image:tag]
    D1 -->|Yes| D3{Private Registry?}
    D3 -->|Yes| D4[Add imagePullSecrets]
    
    B -->|CrashLoopBackOff| E[Check Container]
    E --> E1[kubectl logs pod --previous]
    E1 --> E2{Liveness Probe<br>Failing?}
    E2 -->|Yes| E3[Adjust probe settings<br>initialDelaySeconds]
    E2 -->|No| E4{App Error?}
    E4 -->|Yes| E5[Fix application code]
    E4 -->|No| E6{OOM Killed?}
    E6 -->|Yes| E7[Increase memory limit]
    
    B -->|Running but<br>not working| F[Check Probes]
    F --> F1{Readiness OK?}
    F1 -->|No| F2[Fix readiness probe<br>or app startup]
    F1 -->|Yes| F3[Check service endpoints]
    
    style A fill:#ff6b6b
    style C2 fill:#51cf66
    style C4 fill:#51cf66
    style C6 fill:#51cf66
    style D2 fill:#51cf66
    style D4 fill:#51cf66
    style E3 fill:#51cf66
    style E5 fill:#51cf66
    style E7 fill:#51cf66
    style F2 fill:#51cf66
    style F3 fill:#74c0fc
```

---

## Deployment Troubleshooting

```mermaid
flowchart TD
    A[Deployment Issue] --> B{Rollout Status?}
    
    B -->|Progressing| C[Check Progress]
    C --> C1{Stuck?}
    C1 -->|Yes| C2[kubectl describe deployment<br>Check Events]
    C1 -->|No| C3[Wait for completion]
    
    B -->|Failed| D[Check Reason]
    D --> D1{Image Issue?}
    D1 -->|Yes| D2[Fix image reference]
    D1 -->|No| D3{Resource Quota<br>Exceeded?}
    D3 -->|Yes| D4[Increase quota or<br>reduce requests]
    D3 -->|No| D5{Pod Failures?}
    D5 -->|Yes| D6[Check pod logs<br>and events]
    
    B -->|Wrong Version| E[Rollback]
    E --> E1[kubectl rollout undo deployment/name]
    
    B -->|Not Scaling| F[Check HPA]
    F --> F1{Metrics Server<br>Running?}
    F1 -->|No| F2[Install metrics-server]
    F1 -->|Yes| F3{Max Replicas<br>Reached?}
    F3 -->|Yes| F4[Increase maxReplicas]
    
    style A fill:#ff6b6b
    style C2 fill:#74c0fc
    style C3 fill:#51cf66
    style D2 fill:#51cf66
    style D4 fill:#51cf66
    style D6 fill:#74c0fc
    style E1 fill:#51cf66
    style F2 fill:#51cf66
    style F4 fill:#51cf66
```

---

## Service Troubleshooting

```mermaid
flowchart TD
    A[Service Not Working] --> B{Endpoints Exist?}
    
    B -->|No| C[Check Selector]
    C --> C1{Pod Labels<br>Match?}
    C1 -->|No| C2[Fix labels or selector]
    C1 -->|Yes| C3{Pods Ready?}
    C3 -->|No| C4[Fix pod readiness]
    
    B -->|Yes| D{Connection Works?}
    D -->|No| E[Check Port Config]
    E --> E1{targetPort<br>Correct?}
    E1 -->|No| E2[Fix targetPort]
    E1 -->|Yes| E3{App Listening?}
    E3 -->|No| E4[Fix app to listen<br>on correct port]
    
    D -->|Yes| F{DNS Works?}
    F -->|No| G[Check CoreDNS]
    G --> G1[kubectl get pods -n kube-system -l k8s-app=kube-dns]
    G1 --> G2{CoreDNS Running?}
    G2 -->|No| G3[Fix CoreDNS pods]
    
    F -->|Yes| H[Service Working!]
    
    style A fill:#ff6b6b
    style C2 fill:#51cf66
    style C4 fill:#51cf66
    style E2 fill:#51cf66
    style E4 fill:#51cf66
    style G3 fill:#51cf66
    style H fill:#51cf66
```

---

## ConfigMap/Secret Troubleshooting

```mermaid
flowchart TD
    A[Config Issue] --> B{Type?}
    
    B -->|ConfigMap| C[Check ConfigMap]
    C --> C1{Exists?}
    C1 -->|No| C2[Create ConfigMap]
    C1 -->|Yes| C3{Mounted Correctly?}
    C3 -->|No| C4[Fix volumeMounts path]
    C3 -->|Yes| C5{Key Correct?}
    C5 -->|No| C6[Fix key reference]
    
    B -->|Secret| D[Check Secret]
    D --> D1{Exists?}
    D1 -->|No| D2[Create Secret]
    D1 -->|Yes| D3{Base64 Encoded?}
    D3 -->|No| D4[Re-encode or use<br>stringData field]
    D3 -->|Yes| D5{Referenced<br>Correctly?}
    D5 -->|No| D6[Fix secretKeyRef<br>or secretRef]
    
    B -->|Env Not Set| E[Check Pod Spec]
    E --> E1{envFrom Used?}
    E1 -->|Yes| E2[Check configMapRef<br>or secretRef name]
    E1 -->|No| E3{valueFrom Used?}
    E3 -->|Yes| E4[Check key name]
    
    style A fill:#ff6b6b
    style C2 fill:#51cf66
    style C4 fill:#51cf66
    style C6 fill:#51cf66
    style D2 fill:#51cf66
    style D4 fill:#51cf66
    style D6 fill:#51cf66
    style E2 fill:#51cf66
    style E4 fill:#51cf66
```

---

## Ingress Troubleshooting

```mermaid
flowchart TD
    A[Ingress Not Working] --> B{Controller Installed?}
    
    B -->|No| C[Install Controller]
    C --> C1[Deploy nginx-ingress<br>or similar]
    
    B -->|Yes| D{IngressClass Set?}
    D -->|No| E[Add ingressClassName]
    D -->|Yes| F{Rules Correct?}
    F -->|No| G[Fix path and backend]
    F -->|Yes| H{Backend Service<br>Working?}
    H -->|No| I[Fix service first]
    H -->|Yes| J{TLS Issue?}
    J -->|Yes| K[Check TLS Secret]
    K --> K1{Secret Exists?}
    K1 -->|No| K2[Create TLS secret]
    K1 -->|Yes| K3{Contains tls.crt<br>and tls.key?}
    K3 -->|No| K4[Re-create with<br>correct keys]
    
    J -->|No| L[Check DNS<br>and firewall]
    
    style A fill:#ff6b6b
    style C1 fill:#51cf66
    style E fill:#51cf66
    style G fill:#51cf66
    style I fill:#ffd43b
    style K2 fill:#51cf66
    style K4 fill:#51cf66
    style L fill:#74c0fc
```

---

## Quick Debug Commands

| Issue | Command |
|-------|---------|
| **Pod status** | `kubectl get pod <pod> -o wide` |
| **Pod describe** | `kubectl describe pod <pod>` |
| **Pod logs** | `kubectl logs <pod> -c <container>` |
| **Previous logs** | `kubectl logs <pod> --previous` |
| **Exec into pod** | `kubectl exec -it <pod> -- sh` |
| **Port forward** | `kubectl port-forward <pod> 8080:80` |
| **Service endpoints** | `kubectl get endpoints <svc>` |
| **Test DNS** | `kubectl run test --rm -it --image=busybox -- nslookup <svc>` |
| **Test HTTP** | `kubectl run test --rm -it --image=busybox -- wget -O- <svc>` |
| **Rollout status** | `kubectl rollout status deployment/<name>` |
