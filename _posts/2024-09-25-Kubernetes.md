---
title: Kubernetes Network Security Deep Dive
date: 2024-09-25 12:34:27 -500
categories: [Kubernetes security]
tags: [detection,kubernetes]
---

## eBPF-Powered Security

```yaml
apiVersion: cilium.io/v1alpha1
kind: CiliumNetworkPolicy
metadata:
  name: http-basic-auth
spec:
  endpointSelector:
    matchLabels:
      app: api-server
  ingress:
  - fromEntities:
    - cluster
    toPorts:
    - ports:
      - port: "80"
      - port: "443"
    layer7:
    - http:
      - method: "POST"
        path: "/auth"
        basicAuth:
          username: "admin"
          passwordSecret:
            name: api-credentials
            key: password
```


## CI/CD Integration

```groovy
pipeline {
  agent any
  
  stages {
    stage('Network Policy Check') {
      steps {
        sh 'kube-score score manifests/ --enable-network-policy-check'
        sh 'checkov -d manifests/ --framework kubernetes'
      }
    }
  }
}
```
