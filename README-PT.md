# 💹 Financial Ecosystem Infrastructure

Este repositório contém a fundação de infraestrutura baseada em containers para um ecossistema financeiro distribuído, simulando a integração em tempo real entre uma **Corretora (Broker)** e a **Bolsa de Valores (B3)**.

---

## 🏗️ Arquitetura e Decisões Técnicas

O projeto foi desenhado seguindo as melhores práticas de mercado para arquitetura de microserviços, garantindo isolamento de domínios, resiliência e escalabilidade.

### 1. Camada de Persistência Relacional (SQL)

- **MySQL 8.0 (Broker):** Utilizado para os domínios de `Identity`, `Wallet`, `Order` e `Portfolio`. A escolha baseia-se na sua robustez para transações ACID e ampla integração com o ecossistema Java/Spring.
- **PostgreSQL 15 (B3):** Utilizado no núcleo da B3 para o motor de matching. O Postgres foi selecionado para diversificar a stack tecnológica e aproveitar seus recursos avançados de integridade referencial.

### 2. Camada de Cache e NoSQL

- **Redis 7.0 (Alpine):** Implementado em instâncias isoladas (`market-data`, `wallet` e `b3-market`). A escolha justifica-se pela necessidade de latência sub-milissegundo no acesso a cotações e saldos.
- **MongoDB 6.0:** Instância centralizada para armazenamento de dados não estruturados, como históricos de preços (Ticks) e relatórios, permitindo alta flexibilidade de esquema.

### 3. Mensageria e Eventos

- **Apache Kafka (KRaft):** Utilizado como o backbone de eventos internos do Broker. Configurado no modo **KRaft** para eliminar a complexidade do Zookeeper, facilitando a manutenção e performance.
- **RabbitMQ:** Atua como a ponte de comunicação externa (Bridge) entre a Corretora e a B3, garantindo o desacoplamento total entre os sistemas e a entrega confiável de ordens de compra/venda.

### 4. Observabilidade Centralizada

- **Prometheus & Grafana:** Stack de monitoramento unificada. O Prometheus realiza o scrape de métricas de todos os serviços, enquanto o Grafana fornece dashboards para visualização de performance e saúde do sistema em tempo real.

---

## 🚀 Como Executar

### Pré-requisitos

- Docker Desktop (com WSL 2 se estiver no Windows)
- Docker Compose instalado

### Passo a Passo

1.  **Clone o repositório:**
    ```bash
    git clone [https://github.com/rvneto/finance-infra.git](https://github.com/rvneto/finance-infra.git)
    cd finance-infra
    ```
2.  **Configure as variáveis de ambiente:**
    - Copie o arquivo `.env.example` para um novo arquivo chamado `.env`.
    - Ajuste as senhas e portas se necessário.
3.  **Inicie a infraestrutura:**
    ```bash
    docker-compose up -d
    ```
4.  **Verifique se os 14 containers estão rodando:**
    ```bash
    docker ps
    ```

---

## 🛠️ Portas e Interfaces de Gestão

| Serviço              | Porta Local | Interface de Acesso      |
| :------------------- | :---------- | :----------------------- |
| **MySQL (Identity)** | `3306`      | Banco de Dados           |
| **MySQL (Wallet)**   | `3307`      | Banco de Dados           |
| **PostgreSQL (B3)**  | `5432`      | Banco de Dados           |
| **Kafka UI**         | `8080`      | `http://localhost:8080`  |
| **RabbitMQ UI**      | `15672`     | `http://localhost:15672` |
| **Prometheus**       | `9090`      | `http://localhost:9090`  |
| **Grafana**          | `3000`      | `http://localhost:3000`  |

---

## 📂 Estrutura do Repositório

```text
/finance-infra
  ├── config/
  │    └── prometheus/     # Configurações do Prometheus
  ├── data/                # Volumes persistentes (ignorado no Git)
  ├── .env.example         # Template de variáveis de ambiente
  └── docker-compose.yml   # Orquestração dos containers
```
