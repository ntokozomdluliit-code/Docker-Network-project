# 🐳 Docker Network Home Lab

Welcome to my Docker infrastructure documentation.

<style>
/* Futuristic Cyberpunk Theme */
body {
    background: linear-gradient(135deg, #0a0e27 0%, #1a1040 100%);
    color: #e0e0ff;
    font-family: 'Segoe UI', 'Roboto', system-ui, sans-serif;
}

h1, h2, h3 {
    color: #00ffff;
    text-shadow: 0 0 10px #00ffff66, 0 0 20px #0066ff44;
    border-bottom: 1px solid #00ffff33;
    padding-bottom: 10px;
}

h1 {
    background: linear-gradient(90deg, #00ffff, #ff00ff);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    text-shadow: none;
    border-bottom: 2px solid #ff00ff66;
}

table {
    background: #0d1128cc;
    border: 1px solid #00ffff33;
    backdrop-filter: blur(10px);
    box-shadow: 0 0 30px #0066ff22, inset 0 0 10px #00ffff11;
    border-radius: 12px;
    overflow: hidden;
}

th {
    background: linear-gradient(180deg, #1a1a4e, #0f0f2d);
    color: #00ffff;
    text-transform: uppercase;
    letter-spacing: 1px;
    font-weight: 600;
    border-bottom: 2px solid #ff00ff66 !important;
}

td {
    background: #0a0e27aa;
    color: #b0b0ff;
    border-color: #1a1a4e !important;
}

tr:hover td {
    background: #1a1a4ecc;
    color: #ffffff;
    transition: all 0.3s ease;
}

.mermaid {
    background: #0d1128dd;
    backdrop-filter: blur(10px);
    border: 1px solid #00ffff44;
    border-radius: 16px;
    padding: 30px;
    margin: 30px 0;
    box-shadow: 0 0 50px #0066ff33, inset 0 0 30px #ff00ff11, 0 10px 30px #00000066;
    position: relative;
    overflow: hidden;
}

.mermaid::before {
    content: "";
    position: absolute;
    top: -50%;
    left: -50%;
    width: 200%;
    height: 200%;
    background: radial-gradient(circle, #00ffff11 0%, transparent 70%);
    animation: pulse 8s infinite;
    pointer-events: none;
}

@keyframes pulse {
    0%, 100% { opacity: 0.3; transform: scale(1); }
    50% { opacity: 0.6; transform: scale(1.05); }
}

pre {
    background: #0a0e27;
    border: 1px solid #ff00ff66;
    border-radius: 12px;
    padding: 20px;
    box-shadow: 0 0 30px #ff00ff22, inset 0 0 20px #00000088;
    position: relative;
    overflow: hidden;
}

pre::before {
    content: "$>";
    position: absolute;
    top: 10px;
    left: 15px;
    color: #ff00ff;
    opacity: 0.5;
    font-weight: bold;
}

code {
    color: #00ff88 !important;
    text-shadow: 0 0 5px #00ff8866;
    font-family: 'Fira Code', 'Consolas', monospace;
}

a {
    color: #ff00ff;
    text-decoration: none;
    text-shadow: 0 0 8px #ff00ff66;
    transition: all 0.3s ease;
}

a:hover {
    color: #00ffff;
    text-shadow: 0 0 15px #00ffff;
}

/* Neon grid background */
body::before {
    content: "";
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-image: 
        linear-gradient(#00ffff11 1px, transparent 1px),
        linear-gradient(90deg, #00ffff11 1px, transparent 1px);
    background-size: 50px 50px;
    pointer-events: none;
    z-index: -1;
}

/* Glowing scrollbar */
::-webkit-scrollbar {
    width: 8px;
    height: 8px;
}

::-webkit-scrollbar-track {
    background: #0a0e27;
}

::-webkit-scrollbar-thumb {
    background: linear-gradient(180deg, #ff00ff, #00ffff);
    border-radius: 4px;
}

::-webkit-scrollbar-thumb:hover {
    background: linear-gradient(180deg, #00ffff, #ff00ff);
}
</style>

<script src="https://cdn.jsdelivr.net/npm/mermaid/dist/mermaid.min.js"></script>
<script>
mermaid.initialize({
    startOnLoad: true,
    theme: 'base',
    themeVariables: {
        // Futuristic neon theme for Mermaid diagrams
        'primaryColor': '#1a1a4e',
        'primaryBorderColor': '#00ffff',
        'primaryTextColor': '#e0e0ff',
        'lineColor': '#ff00ff',
        'secondaryColor': '#0f0f2d',
        'tertiaryColor': '#0a0e27',
        'background': '#0d1128',
        'mainBkg': '#1a1a4e',
        'secondBkg': '#0f0f2d',
        'border1': '#00ffff',
        'border2': '#ff00ff',
        'arrowheadColor': '#ff00ff',
        'fontFamily': 'Segoe UI, Roboto, sans-serif',
        'fontSize': '16px',
        'labelBackground': '#1a1a4ecc',
        'textColor': '#e0e0ff',
        'nodeBorder': '#00ffff',
        'nodeTextColor': '#e0e0ff',
        'clusterBkg': '#0d1128dd',
        'clusterBorder': '#ff00ff66',
        'titleColor': '#00ffff',
        'edgeLabelBackground': '#1a1a4ecc'
    },
    flowchart: {
        curve: 'basis',
        padding: 20,
        nodeSpacing: 80,
        rankSpacing: 100,
        htmlLabels: true,
        useMaxWidth: true
    }
});
</script>

## 🌐 Network Overview

<div class="mermaid">
graph TB
    subgraph External
        IN["🌍 Internet"]
        CF["☁️ Cloudflare Tunnel"]
    end
    
    subgraph "🖥️ Docker Host"
        RP["🔀 Traefik Reverse Proxy"]
        subgraph "🎯 Frontend Network"
            NG["🌐 Nginx"]
            GRAF["📊 Grafana"]
        end
        subgraph "⚙️ Backend Network"
            API["🚀 API Service"]
            DB["🗄️ PostgreSQL"]
            REDIS["⚡ Redis Cache"]
        end
        subgraph "📡 Monitoring Network"
            PROM["📈 Prometheus"]
            NODE["🖥️ Node Exporter"]
            CADV["📦 cAdvisor"]
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
    
    style IN fill:#1a1040,stroke:#ff00ff,stroke-width:3px,color:#fff
    style CF fill:#1a1a4e,stroke:#00ffff,stroke-width:3px,color:#fff
    style RP fill:#0f0f2d,stroke:#ff00ff,stroke-width:3px,color:#fff
    style NG fill:#1a1a4e,stroke:#00ffff,stroke-width:2px,color:#fff
    style GRAF fill:#1a1a4e,stroke:#00ffff,stroke-width:2px,color:#fff
    style API fill:#1a1a4e,stroke:#00ffff,stroke-width:2px,color:#fff
    style DB fill:#0f0f2d,stroke:#ff00ff,stroke-width:2px,color:#fff
    style REDIS fill:#0f0f2d,stroke:#ff00ff,stroke-width:2px,color:#fff
    style PROM fill:#1a1a4e,stroke:#00ffff,stroke-width:2px,color:#fff
    style NODE fill:#1a1a4e,stroke:#00ffff,stroke-width:2px,color:#fff
    style CADV fill:#1a1a4e,stroke:#00ffff,stroke-width:2px,color:#fff
</div>

## 🔮 Network Configuration

| 🌐 Network | 📡 Subnet | 🚪 Gateway | 🎯 Purpose |
|-----------|----------|-----------|----------|
| **frontend** | `172.20.0.0/16` | `172.20.0.1` | Public services |
| **backend** | `172.21.0.0/16` | `172.21.0.1` | Databases/Cache |
| **monitoring** | `172.22.0.0/16` | `172.22.0.1` | Observability |

## ⚡ Docker Commands

```bash
# 📋 List all networks
docker network ls

# 🔍 Inspect network details
docker network inspect frontend

# 🚀 Create new network
docker network create --driver bridge --subnet 172.23.0.0/16 new-network

# 🔗 Connect container to network
docker network connect frontend container-name
