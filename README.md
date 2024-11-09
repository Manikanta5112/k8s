# Kubernetes Worker Node and Traffic Flow Diagrams

## Worker Node Structure
```
+-------------------------------------+
|           Worker Node               |
|                                     |
|  +-----------------------------+    |
|  |         Kubelet             |    |
|  +-----------------------------+    |
|  |      Container Runtime      |    |
|  +-----------------------------+    |
|  |        Kube-proxy           |    |
|  +-----------------------------+    |
|                                     |
|  +-----------+     +-----------+    |
|  |  Pod 1    |     |  Pod 2    |    |
|  | (App A)   |     | (App B)   |    |
|  +-----------+     +-----------+    |
|  | Container |     | Container |    |
|  +-----------+     +-----------+    |
|                                     |
+-------------------------------------+
```

## Traffic Flow from Internet to Pod via NodePort
```
              ┌──────────────┐
              │   Internet   │
              └──────────────┘
                     │
      HTTP Request (NodeIP:30007)
                     │
                     ▼
              ┌──────────────┐
              │  Worker Node │
              │ (NodePort: 30007) │
              └──────────────┘
                     │
                     │
    Routes traffic to `Service: port 80`
                     │
                     ▼
              ┌────────────────┐
              │   Service      │
              │ (port: 80)     │
              └────────────────┘
                     │
                     │
      Forwards traffic to `targetPort: 8080` in Pod
                     │
                     ▼
              ┌────────────────┐
              │     Pod        │
              │  ┌──────────┐  │
              │  │ Container │ │
              │  │ (App on   │ │
              │  │ port: 8080) │
              │  └──────────┘  │
              └────────────────┘
                     │
         Processes request and
         sends response back to user
                     │
                     │
                     ▼
              ┌──────────────┐
              │   Internet   │
              └──────────────┘
```
