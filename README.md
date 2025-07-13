# Arquitetura do Sistema de Pagamento de Estacionamento

Este documento descreve a arquitetura de um sistema projetado para solucionar problemas de fila e pagamento de tickets de estacionamento. O sistema é baseado em uma arquitetura de microsserviços para garantir escalabilidade, resiliência e manutenibilidade.

## 1. Visão Geral da Arquitetura

O sistema é composto por um conjunto de serviços independentes que se comunicam através de APIs REST e um barramento de eventos. A arquitetura visa ser flexível e permitir a integração com diferentes parceiros de pagamento e sistemas de controle de acesso de estacionamento.

```mermaid
graph TD
    subgraph "Usuário Final"
        A[Dispositivo Móvel / Web App]
    end

    subgraph "Gateway de API"
        B[API Gateway]
    end

    subgraph "Serviços Principais"
        C[Serviço de Estacionamento]
        D[Serviço de Faturamento]
        E[Serviço de Usuários]
    end

    subgraph "Integrações Externas"
        F[Parceiro de Pagamento Externo]
        G[Câmeras de Reconhecimento de Placa (LPR)]
        H[Sistema de Notificações (Push/Email)]
    end

    subgraph "Barramento de Eventos"
        I[Kafka / RabbitMQ]
    end

    subgraph "Bancos de Dados"
        J[Banco de Dados - Estacionamento (PostgreSQL)]
        K[Banco de Dados - Faturamento (PostgreSQL)]
        L[Banco de Dados - Usuários (MongoDB)]
    end

    A -- Requisições HTTP --> B
    B -- Roteamento --> C
    B -- Roteamento --> D
    B -- Roteamento --> E

    C -- "Registra Entrada/Saída" --> J
    C -- "Publica Evento de Saída" --> I

    D -- "Consome Evento de Saída" --> I
    D -- "Gera Fatura" --> K
    D -- "Inicia Processo de Pagamento" --> F
    D -- "Publica Evento de Pagamento" --> I

    E -- "Gerencia Dados do Usuário" --> L

    F -- "Webhook de Confirmação de Pagamento" --> B
    B -- Roteamento Webhook --> D

    G -- "Envia Placa Lida" --> C

    C -- "Solicita Notificação" --> H
    D -- "Solicita Notificação" --> H
