'''graph TD
    subgraph "Frontend Layer"
        A1[Web Application]
        A2[Mobile Application]
        A3[Admin Dashboard]
    end

    subgraph "API Gateway"
        B1[API Gateway/Load Balancer]
        B2[Authentication Service]
        B3[Rate Limiting]
    end

    subgraph "Core Services"
        C1[Plate Registry Service]
        C2[Invoice Service]
        C3[Payment Service]
        C4[Event Consumer Service]
    end

    subgraph "External Services"
        D1[Payment Provider API]
        D2[Notification Service]
        D3[Vehicle Registration API]
    end

    subgraph "Data Layer"
        E1[PostgreSQL - Main DB]
        E2[Redis - Cache]
        E3[Message Queue - Kafka/RabbitMQ]
    end

    subgraph "Infrastructure"
        F1[Monitoring - Prometheus/Grafana]
        F2[Logging - ELK Stack]
        F3[Container Orchestration - K8s]
    end

    A1 --> B1
    A2 --> B1
    A3 --> B1
    
    B1 --> B2
    B1 --> B3
    B1 --> C1
    B1 --> C2
    B1 --> C3
    
    C1 --> E1
    C1 --> E2
    C2 --> E1
    C2 --> E2
    C3 --> E1
    C3 --> E3
    
    C4 --> E3
    C4 --> C2
    C4 --> C3
    
    C3 --> D1
    C4 --> D2
    C1 --> D3
    
    C1 --> F1
    C2 --> F1
    C3 --> F1
    C4 --> F1
    
    E1 --> F2
    E2 --> F2
    E3 --> F2