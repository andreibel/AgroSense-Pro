```mermaid
graph TD
    %% High-Contrast Styling
    classDef storage fill:#ffffff,stroke:#333,stroke-width:2px,color:#000;
    classDef compute fill:#f0f7ff,stroke:#333,stroke-width:1px,color:#000;
    classDef layer fill:#fdfdfd,stroke:#bbb,stroke-dasharray: 5 5;

    %% Layer 1: Field/Edge
    subgraph FIELD ["Field Layer (Edge)"]
        direction LR
        Sensors([Environmental Sensors])
        Camera([High-Res Camera])
        Gateway[Local Gateway]
        
        Sensors --> Gateway
        Camera --> Gateway
    end

    %% Layer 2: Cloud Core
    subgraph CLOUD ["Cloud Infrastructure"]
        direction TB
        
        API_G[API Gateway]
        Logic(Core Backend Logic)
        
        subgraph ANALYTICS ["Analysis & AI"]
            direction LR
            AI_Eng[[AI Inference Engine]]:::compute
            Weather[[External Weather API]]
        end
        
        subgraph STORAGE ["Data Storage Tier"]
            direction LR
            Img_Store[("Object Storage (Images)")]:::storage
            Data_DB[("Structured Database")]:::storage
        end
        
        %% Internal Cloud Connections
        API_G --> Logic
        Logic <--> AI_Eng
        Weather -.-> Logic
        Logic --> Img_Store
        Logic --> Data_DB
    end

    %% Layer 3: Application/User
    subgraph APP ["Client Layer"]
        direction LR
        Dashboard([Pro Dashboard])
        Alerts([Alert Service])
    end

    %% Main Data Flow
    Gateway -->|Secure Uplink| API_G
    Logic --> Dashboard
    Logic --> Alerts
