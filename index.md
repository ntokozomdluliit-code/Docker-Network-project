---
layout: default
title: Docker Network Documentation
---

# 🐳 Docker Network Documentation

Welcome to my Docker infrastructure documentation.

## Network Overview

<div class="mermaid">
graph TB
    subgraph "External"
        IN[Internet]
        CF[Cloudflare Tunnel]
    end
    
    subgraph "Host: Docker Host"
        RP[Traefik Reverse Proxy]
        subgraph "Network: frontend"
            NG[Nginx]
            GRAF[Grafana]
        end
        subgraph "Network: backend"
            API[API Service]
            DB[(PostgreSQL)]
            REDIS[(Redis Cache)]
        end
        subgraph "Network: monitoring"
            PROM[Prometheus]
            NODE[Node Exporter]
            CADV[cAdvisor]
        end
    end
    
    IN --> CF
    CF --> RP
    RP --> NG
    RP --> API
    RP --> GRAF
    API --> DB
    API --> REDIS
    PROM --> NODE
    PROM --> CADV
    GRAF --> PROM
</div>
