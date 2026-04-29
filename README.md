# drone-data-platform
Estrutura de dados para drones autônomos

---

## Visão Geral

Este projeto apresenta a arquitetura de dados para uma plataforma de gerenciamento de drones agrícolas operando em larga escala.

A solução foi projetada para suportar até **150 drones simultâneos**, transmitindo dados continuamente como:

- Telemetria (GPS, altitude, velocidade)
- Eventos operacionais
- Logs de missão
- Imagens térmicas e multiespectrais

A arquitetura prioriza:

- Escalabilidade horizontal
- Alta disponibilidade
- Processamento em tempo real
- Integridade de dados críticos

---

## Arquitetura da Solução

<img width="959" height="752" alt="Estruturadedadosdrones drawio" src="https://github.com/user-attachments/assets/76bc7ed6-9b8e-49c6-946d-e5ec88fc1da9" />

## Fluxo de Dados

Drones → Kafka → Processamento → Bancos especializados + Storage

---

## Tecnologias Utilizadas

| Componente | Tecnologia | Motivo |
|----------|--------|--------|
| Ingestão de dados | Apache Kafka | Alta taxa de ingestão e processamento assíncrono |
| Dados críticos | PostgreSQL | Consistência forte (ACID) |
| Telemetria | TimescaleDB | Otimizado para séries temporais |
| Logs/Eventos | Cassandra | Escalabilidade horizontal |
| Arquivos | Amazon S3 | Armazenamento de binários |
| Processamento | FastAPI / Consumers | Integração e persistência |

---

# Decisões Arquiteturais

## Arquitetura orientada a eventos

O uso do Kafka permite desacoplamento entre ingestão e processamento, garantindo resiliência e escalabilidade.

---

## Polyglot Persistence

Cada tipo de dado utiliza o banco mais adequado:

- Telemetria → banco de séries temporais
- Dados críticos → banco relacional
- Logs → banco distribuído
- Arquivos → object storage

---

## Escalabilidade

- Kafka com múltiplas partitions
- Cassandra distribuído
- Storage escalável (S3)

---

## Modelagem de Dados

Entidades principais:

- Drones
- Missões
- Telemetria
- Eventos
- Arquivos
- Logs de manutenção

Relacionamentos:

- Drone → Missões (1:N)
- Drone → Telemetria (1:N)
- Missão → Arquivos (1:N)

---

## Mensageria (Kafka)

- Partition key: `drone_id`
- Retenção: 7 dias
- Replicação: fator 3
- Retry com DLQ (Dead Letter Queue)

Benefícios:

- Ordenação por drone
- Reprocessamento de eventos
- Alta disponibilidade

---

## Armazenamento de Arquivos

- Armazenamento: Amazon S3
- Banco guarda apenas metadados

Exemplo:

json
{
  "file_id": "123",
  "drone_id": "D1",
  "mission_id": "M1",
  "url": "s3://bucket/image.jpg"
}

# Trade-offs Técnicos

```- Consistência vs Disponibilidade:```
- PostgreSQL → Consistência forte
- Kafka/Cassandra → Alta disponibilidade

# Backup e Disaster Recovery

- Replicação multi-zona
- Snapshots periódicos
- Retenção no Kafka para reprocessamento

# Gargalos e Mitigação

```-Problema & Solução```
- Pico de ingestão → Kafka
- Escrita massiva → Cassandra
- Arquivos grandes → S3
- Falhas de processamento → Retry + DLQ

# Escalabilidade

- A arquitetura suporta crescimento da frota sem necessidade de redesign:
- Adição de novos brokers Kafka
- Expansão de nós Cassandra
- Escala automática do storage
