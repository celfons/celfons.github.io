```mermaid
graph TD
    subgraph Frontend
        A1[App Web/Mobile]
    end

    subgraph Backend
        B1[API Gateway]
        B2[Serviço de Registro de Placas]
        B3[Serviço de Faturas]
        B4[Serviço de Pagamentos]
        B5[Consumer de Eventos / Webhook]
    end

    subgraph Infraestrutura
        C1[Banco de Dados SQL]
        C2[Fila de Mensagens (Kafka/SQS)]
        C3[Cache (Redis)]
        C4[Monitoramento e Logs]
    end

    A1 --> B1
    B1 --> B2
    B1 --> B3
    B1 --> B4
    B2 --> C1
    B3 --> C1
    B3 --> C3
    B4 --> C1
    B4 --> C2
    B5 --> C2
    B5 --> B3
    B5 --> C4
