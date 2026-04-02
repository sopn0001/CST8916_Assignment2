# CST8916 Assigment 2

**Student Name**: IDRIS JOVIAL SOP NWABO

**Student ID**: 041199877

**Course**: CST8916 Remote Data and Real Time Application

**Semester**: Winter 2026


---

## Demo Video

🎥 [Watch Demo Video](https://www.youtube.com/watch?v=v6r1V-HwIVE)

---

## Technical Explanations

- [GitHub](https://github.com/sopn0001/26W_CST8916_Week10-Event-Hubs-Lab)

Architecture diagram showing how data flows from the store to the dashboard through Stream Analytics
Design decisions — why you chose your approach for enriching events and connecting Stream Analytics output to the dashboard
Setup instructions — how to run your solution (environment variables, Azure resources needed, etc.)

## Architecture

```mermaid
flowchart LR
    subgraph AAS["☁️ Azure App Service"]
        direction TB
        Store["🛍️ client.html\nDemo Store\n(Event Producer)"]
        Dashboard["📊 dashboard.html\nLive Dashboard\n(Event Consumer View)"]

        subgraph Flask["⚙️ app.py — Flask Application"]
            direction TB
            Track["/track\nsend_to_event_hubs()"]
            API["/api/events\n_event_buffer"]
            Consumer["Background Consumer Thread\nEventHubConsumerClient"]
        end

        Store -->|"POST /track"| Track
        API -->|"JSON response"| Dashboard
        Dashboard -->|"polls every 2 s\nGET /api/events"| API
    end

    subgraph EH["☁️ Azure Event Hubs"]
        direction TB
        NS["Namespace: shopstream"]
        Hub["Event Hub: clickstream"]
        P0["Partition 0"]
        P1["Partition 1"]
        NS --> Hub
        Hub --> P0
        Hub --> P1
    end

    Track -->|"azure-eventhub SDK\npublish event"| EH
    EH -->|"read events\ncontinuously"| Consumer
    Consumer -->|"append to buffer"| API

    %% ── Colour palette ──────────────────────────────────────────────
    %% Frontend (warm amber)
    style Store     fill:#FFF3E0,stroke:#E65100,color:#212121,stroke-width:2px
    style Dashboard fill:#FFF3E0,stroke:#E65100,color:#212121,stroke-width:2px

    %% Flask internals (cool blue)
    style Flask     fill:#E3F2FD,stroke:#1565C0,color:#212121,stroke-width:2px
    style Track     fill:#BBDEFB,stroke:#1565C0,color:#212121,stroke-width:1.5px
    style API       fill:#BBDEFB,stroke:#1565C0,color:#212121,stroke-width:1.5px
    style Consumer  fill:#BBDEFB,stroke:#1565C0,color:#212121,stroke-width:1.5px

    %% App Service container (light grey)
    style AAS       fill:#F5F5F5,stroke:#616161,color:#212121,stroke-width:2px

    %% Event Hubs (purple / Microsoft brand)
    style EH        fill:#F3E5F5,stroke:#6A1B9A,color:#212121,stroke-width:2px
    style NS        fill:#E1BEE7,stroke:#6A1B9A,color:#212121,stroke-width:1.5px
    style Hub       fill:#CE93D8,stroke:#6A1B9A,color:#212121,stroke-width:1.5px
    style P0        fill:#BA68C8,stroke:#6A1B9A,color:#FFFFFF,stroke-width:1.5px
    style P1        fill:#BA68C8,stroke:#6A1B9A,color:#FFFFFF,stroke-width:1.5px
```
---

## Challenges and Learnings (Optional)


---

## Acknowledgments

[Optional: Credit any resources, documentation, or people who helped you]
