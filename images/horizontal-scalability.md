# Horizontal Scalability

```mermaid
flowchart LR
    direction LR1
    Clients --> |requests| LB[VIRTUAL<br>LOADBALANCER<br>CLUSTER]
    LB --> A1[APP SERVER1]
    LB --> A2[APP SERVER2]
    LB -.-> A3[APP SERVER3]
    style A3 stroke:red
    linkStyle 3 stroke:red
```
