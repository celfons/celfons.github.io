```mermaid
sequenceDiagram
    participant API as API REST
    participant Mongo as MongoDB
    participant ChangeStream as Change Stream Listener
    participant Orchestrator as Orquestrador
    participant Payment as Serviço de Pagamento
    participant Inventory as Serviço de Estoque
    participant Shipping as Serviço de Entrega

    API->>Mongo: Cria novo pedido (status: CREATED)
    Mongo-->>ChangeStream: Evento de inserção detectado
    ChangeStream->>Orchestrator: Notifica novo pedido

    Orchestrator->>Payment: Inicia pagamento
    Payment->>Mongo: Atualiza status para PAID
    Mongo-->>ChangeStream: Evento de atualização detectado
    ChangeStream->>Orchestrator: Notifica status PAID

    Orchestrator->>Inventory: Reserva estoque
    Inventory->>Mongo: Atualiza status para STOCK_RESERVED
    Mongo-->>ChangeStream: Evento de atualização detectado
    ChangeStream->>Orchestrator: Notifica status STOCK_RESERVED

    Orchestrator->>Shipping: Inicia envio
    Shipping->>Mongo: Atualiza status para SHIPPED
    Mongo-->>ChangeStream: Evento de atualização detectado
    ChangeStream->>Orchestrator: Notifica status SHIPPED

    Orchestrator->>Mongo: Atualiza status final para COMPLETED
