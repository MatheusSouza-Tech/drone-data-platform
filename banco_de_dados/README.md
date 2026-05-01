# drone-data-platform

Sistema de banco de dados para gerenciamento de drones, missões, telemetria e eventos em tempo real.

---

## Diagrama do Banco de Dados

 O diagrama completo está disponível em PDF:

 `banco_de_dados/diagrama_bd.pdf`

---

## Estrutura do Banco de Dados

### Tabela: `drones`

Armazena os drones cadastrados no sistema.

| Campo          | Descrição                                      |
|----------------|-----------------------------------------------|
| id             | Identificador único do drone                  |
| model          | Modelo do drone                              |
| status         | Estado atual (ativo, desligado, voando)      |
| battery_level  | Nível de bateria (%)                         |

---

### Tabela: `missoes`

Armazena as missões realizadas pelos drones.

| Campo       | Descrição                     |
|------------|-----------------------------|
| id         | Identificador da missão     |
| drone_id   | Drone responsável           |
| start_time | Início da missão            |
| end_time   | Fim da missão               |
| status     | Status da missão            |

 **Relação:**  
Um drone pode ter várias missões (**1:N**)

---

### Tabela: `telemetria`

Armazena dados de localização e movimento em tempo real.

| Campo      | Descrição                  |
|-----------|---------------------------|
| drone_id  | Drone monitorado          |
| timestamp | Momento da coleta         |
| latitude  | Latitude                  |
| longitude | Longitude                 |
| altitude  | Altura                    |
| speed     | Velocidade                |

**Observação:**  
Tabela com grande volume de dados (logs contínuos)

---

### Tabela: `eventos`

Registra alertas e problemas dos drones.

| Campo       | Descrição                |
|------------|-------------------------|
| drone_id   | Drone afetado           |
| type       | Tipo do evento          |
| timestamp  | Quando ocorreu          |
| description| Descrição do problema   |

**Exemplos:**
- Bateria baixa  
- Falha de sistema  

---

### Tabela: `arquivos`

Armazena arquivos gerados durante as missões.

| Campo       | Descrição                        |
|------------|---------------------------------|
| drone_id   | Drone relacionado               |
| mission_id | Missão associada                |
| url        | Caminho do arquivo              |
| type       | Tipo (imagem, vídeo, log)       |
| timestamp  | Momento do registro             |

---

### Tabela: `manutencao`

Histórico de manutenção dos drones.

| Campo       | Descrição               |
|------------|------------------------|
| drone_id   | Drone                  |
| description| Serviço realizado      |
| date       | Data da manutenção     |

---

## Relacionamentos

- Drone → Missões (**1:N**)  
- Drone → Telemetria (**1:N**)  
- Drone → Eventos (**1:N**)  
- Drone → Manutenção (**1:N**)  
- Missão → Arquivos (**1:N**)  

---

## Visão Geral

```DRONE
── MISSÕES 
 └── ARQUIVOS
├── TELEMETRIA
├── EVENTOS
└── MANUTENÇÃO
```
