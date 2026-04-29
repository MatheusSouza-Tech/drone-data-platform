# Tecnologia

Kafka é usado porque:

suporta milhões de eventos
é distribuído
tolerante a falhas

# Ordenação
Partition key: drone_id

Isso garante:

dados do mesmo drone em ordem

# Retenção
7 dias

Por quê?

reprocessamento se der erro

# Retry
Consumer tenta novamente
Falhou → vai pra DLQ (Dead Letter Queue)
