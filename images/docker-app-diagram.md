# Docker app diagram

```mermaid
stateDiagram 
    direction LR1
    [*] --> LOADBALANCER: request
    LOADBALANCER --> APP_SERVER1
    LOADBALANCER --> APP_SERVER2
```
