---
layout: default
title: Production Network
---

<script src="https://cdn.jsdelivr.net/npm/mermaid@10/dist/mermaid.min.js"></script>
<script>
  mermaid.initialize({ 
    startOnLoad: true, 
    theme: 'default',
    securityLevel: 'loose'
  });
</script>

# Production Docker Network

## Network Topology

<div class="mermaid">
flowchart LR
    subgraph "Docker Host"
        subgraph "Network: traefik-public"
            TRAEFIK[Traefik<br/>:80, :443]
        end
        
        subgraph "Network: frontend"
            WWW[www<br/>:3000]
            API[api<br/>:8080]
        end
        
        subgraph "Network: backend"
            DB[(PostgreSQL<br/>:5432)]
            CACHE[(Redis<br/>:6379)]
            MQ[(RabbitMQ<br/>:5672)]
        end
    end
    
    TRAEFIK --> WWW
    TRAEFIK --> API
    WWW --> API
    API --> DB
    API --> CACHE
    API --> MQ
</div>

## Docker Compose Configuration

```yaml
version: '3.8'

networks:
  traefik-public:
    driver: bridge
    ipam:
      config:
        - subnet: 172.25.0.0/16
  frontend:
    driver: bridge
    ipam:
      config:
        - subnet: 172.20.0.0/16
  backend:
    driver: bridge
    internal: true
    ipam:
      config:
        - subnet: 172.21.0.0/16

services:
  traefik:
    image: traefik:v2.10
    container_name: traefik
    networks:
      - traefik-public
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock:ro
    labels:
      - "traefik.enable=true"

  postgres:
    image: postgres:15-alpine
    container_name: postgres
    networks:
      - backend
    volumes:
      - postgres_data:/var/lib/postgresql/data
    environment:
      POSTGRES_PASSWORD: ${DB_PASSWORD}

  redis:
    image: redis:7-alpine
    container_name: redis
    networks:
      - backend
    command: redis-server --appendonly yes

volumes:
  postgres_data:
