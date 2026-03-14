# 💹 Financial Ecosystem Infrastructure

This repository contains the container-based infrastructure foundation for a distributed financial ecosystem, simulating real-time integration between a **Broker** and the **Stock Exchange (B3)**.

---

## 🏗️ Architecture and Technical Decisions

The project was designed following industry best practices for microservices architecture, ensuring domain isolation, resilience, and scalability.

### 1. Relational Persistence Layer (SQL)

- **MySQL 8.0 (Broker):** Used for the `Identity`, `Wallet`, `Order`, and `Reports` domains. This choice is based on its robustness for ACID transactions and broad integration with the Java/Spring ecosystem.
- **PostgreSQL 15 (B3):** Powering the B3 core matching engine. Postgres was selected to diversify the tech stack and leverage its advanced referential integrity features.

### 2. Caching and NoSQL Layer

- **Redis 7.0 (Alpine):** Implemented in isolated instances (`broker-cache` and `b3-market-data` ). This choice is justified by the need for sub-millisecond latency when accessing quotes and balances.
- **MongoDB 6.0:** Centralized instance for unstructured data storage, such as price history (Ticks) and reports, allowing for high schema flexibility.

### 3. Messaging and Events

- **Apache Kafka (KRaft):** erves as the event backbone for the Broker's internal services. Configured in **KRaft** mode to eliminate Zookeeper complexity, streamlining maintenance and performance.
- **RabbitMQ:** Acts as the external communication bridge between the Broker and B3, ensuring full decoupling between systems and reliable order delivery.

### 4. Centralized Observability

- **Prometheus & Grafana:** Unified monitoring stack. Prometheus scrapes metrics from all services, while Grafana provides dashboards for real-time performance and health visualization.

---

## 🚀 Getting Started

### Prerequisites

- Docker Desktop (with WSL 2 if on Windows)
- Docker Compose installed

### Step-by-Step

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/rvneto/trading-docker-infra.git
    cd trading-docker-infra
    ```
2.  **Configure environment variables:**
    - Copy the `.env.example` file to a new file named `.env`.
    - Adjust passwords and ports if necessary.
3.  **Spin up the infrastructure:**
    ```bash
    docker-compose up -d
    ```
4.  **Verify if all 12 containers are running:**
    ```bash
    docker ps
    ```

---

## 🛠️ Ports and Management Interfaces

| Service              | Local Port | Access Interface         |
| :------------------- | :--------- | :----------------------- |
| **MySQL (Identity)** | `3306`     | Database                 |
| **MySQL (Wallet)**   | `3307`     | Database                 |
| **PostgreSQL (B3)**  | `5432`     | Database                 |
| **Kafka UI**         | `8080`     | `http://localhost:8080`  |
| **RabbitMQ UI**      | `15672`    | `http://localhost:15672` |
| **Prometheus**       | `9090`     | `http://localhost:9090`  |
| **Grafana**          | `3000`     | `http://localhost:3000`  |

---

## 📂 Repository Structure

```text
/trading-docker-infra
  ├── config/
  │    └── prometheus/     # Prometheus configurations
  ├── data/                # Persistent volumes (Git ignored)
  ├── .env.example         # Environment variables template
  └── docker-compose.yml   # Container orchestration
```
