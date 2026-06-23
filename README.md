<div align="center">

# 🛰 Historical Traffic Routing Platform

### Time-aware routing & simulation over real road networks.

*What was the fastest route at 8:15 AM last Tuesday? This platform answers questions a live router can't.*

[![Status](https://img.shields.io/badge/status-Active%20R%26D-F2B705?style=for-the-badge)](#-development-status)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![OpenStreetMap](https://img.shields.io/badge/OpenStreetMap-7EBC6F?style=for-the-badge&logo=openstreetmap&logoColor=white)

</div>

> **Public portfolio repository.** Architecture, design, and roadmap are open; core implementation is private. Code walkthrough available under NDA — **arasghorbani9090@gmail.com**.

---

## 📖 Overview

Most routing engines answer one question: *"What's the fastest way right now?"* But planners, researchers, and logistics teams need a different one: *"What did traffic actually do over time, and how would different routes have performed?"*

The **Historical Traffic Routing Platform** layers **time** onto routing. It replays historical road-network conditions through an OSRM engine built on OpenStreetMap, so you can compute routes *as they would have been* at any point in a time series — then simulate, compare, and analyze them at scale.

**Use cases**
- 📊 **Traffic analysis** — quantify congestion patterns across hours, days, and corridors.
- 🚚 **Fleet & logistics planning** — evaluate routing strategies against real historical conditions.
- 🧪 **Routing research** — A/B route policies on reproducible, time-stamped network states.

---

## ✨ Features

- ⏱ **Historical routing** — compute routes against road-network conditions at a chosen timestamp, not just live state.
- 🔁 **Simulation framework** — replay journeys across time windows to study how route choice and travel time evolve.
- 🗺 **OSM-native** — built directly on OpenStreetMap data, so coverage extends anywhere OSM does.
- 🚀 **OSRM core** — fast shortest-path computation over contracted road graphs.
- 🌐 **Clean FastAPI service** — typed, documented HTTP API with automatic OpenAPI docs.
- 📈 **Analysis outputs** — structured route + timing data ready for notebooks and dashboards.

---

## 🏗 Architecture

```mermaid
flowchart LR
    subgraph Client["Clients"]
        NB["Notebooks / Analysis"]
        APP["Apps / Dashboards"]
    end

    subgraph API["⚡ FastAPI Service"]
        ROUTES["/route /simulate /analyze"]
        VALID["Request validation (Pydantic)"]
        SIM["Simulation orchestrator"]
    end

    subgraph Engine["🧭 Routing Engine"]
        OSRM["OSRM backend"]
        GRAPH["Contracted road graph"]
    end

    subgraph DataLayer["🗄 Data"]
        OSM[("OpenStreetMap extracts")]
        HIST[("Historical traffic time series")]
    end

    NB --> ROUTES
    APP --> ROUTES
    ROUTES --> VALID --> SIM
    SIM --> OSRM
    OSRM --> GRAPH
    GRAPH --> OSM
    SIM -. weight by time .-> HIST
```

### Simulation flow

```mermaid
sequenceDiagram
    participant U as Analyst
    participant API as FastAPI
    participant SIM as Simulator
    participant OSRM as OSRM
    participant H as Historical store

    U->>API: POST /simulate {origin, dest, time window}
    loop each timestep
        API->>SIM: advance(t)
        SIM->>H: conditions at t
        H-->>SIM: edge weights
        SIM->>OSRM: route with weights(t)
        OSRM-->>SIM: path + duration
    end
    SIM-->>API: time series of routes
    API-->>U: comparative analysis
```

---

## 🧱 Tech Stack

| Layer | Technology |
| --- | --- |
| **API** | FastAPI, Pydantic, Uvicorn |
| **Routing** | OSRM (Open Source Routing Machine) |
| **Map data** | OpenStreetMap extracts |
| **Language** | Python 3.11+ |
| **Analysis** | NumPy / Pandas-friendly structured outputs |
| **Packaging** | Docker (OSRM + service) |

---

## 📂 Folder Structure

> Representative — illustrative of organization, not a source dump.

```
historical-traffic-routing/
├── app/
│   ├── api/                # FastAPI routers: route, simulate, analyze
│   ├── core/               # config, settings
│   ├── routing/            # OSRM client + graph weighting
│   ├── simulation/         # time-stepped replay engine
│   └── schemas/            # Pydantic request/response models
├── data/
│   ├── osm/                # OpenStreetMap extracts & build artifacts
│   └── historical/         # time-series traffic conditions
├── notebooks/              # analysis & validation
└── docker/                 # OSRM + service compose
```

---

## 🖼 Screenshots

> Placeholders — add captures to `docs/screenshots/`.

| Route over time | Congestion heatmap | API docs |
| --- | --- | --- |
| ![Routes](docs/screenshots/routes.png) | ![Heatmap](docs/screenshots/heatmap.png) | ![Docs](docs/screenshots/api.png) |

---

## 🗺 Roadmap

- [x] OSRM + OSM routing core
- [x] FastAPI service with typed endpoints
- [x] Time-stepped simulation framework
- [ ] Historical condition ingestion pipeline (batch + streaming)
- [ ] Corridor-level congestion analytics
- [ ] Scenario comparison API (policy A vs B)
- [ ] Caching layer for repeated time-window queries
- [ ] Hosted demo + sample datasets

---

## 📈 Development Status

🟡 **Active R&D** — routing core and simulation framework are functional; current work is on historical-data ingestion and analytics surfaces.

---

## 🤝 Contact

📧 **arasghorbani9090@gmail.com** · 🔗 [LinkedIn](https://www.linkedin.com/in/arasghorbani)

<div align="center"><sub>Public architecture & docs. Implementation is proprietary.</sub></div>
