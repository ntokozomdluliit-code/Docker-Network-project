---
layout: default
title: Docker Network Documentation
---

# 🐳 Docker Network Documentation

Welcome to my Docker infrastructure documentation. This site provides a visual reference of all container networks, services, and their interconnections.

## Quick Navigation

- [Production Network](networks/production.md)
- [Development Network](networks/development.md)
- [Home Lab Setup](networks/home-lab.md)

## Network Overview

```mermaid
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
