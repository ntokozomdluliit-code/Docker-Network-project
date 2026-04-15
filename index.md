---
layout: default
title: Home
---

# 🐳 Docker Network Home Lab

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

## Networks

| Network | Subnet | Gateway | Purpose |
|---------|--------|---------|---------|
| frontend | 172.20.0.0/16 | 172.20.0.1 | Public services |
| backend | 172.21.0.0/16 | 172.21.0.1 | Databases/Cache |
| monitoring | 172.22.0.0/16 | 172.22.0.1 | Observability |

## Docker Commands

```bash
# List networks
docker network ls

# Inspect network
docker network inspect frontend
