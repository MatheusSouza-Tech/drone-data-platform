# Onde guardar:
S3 (ou MinIO)

# Por que não usar banco?
Porque:

arquivos são grandes
banco ficaria lento

# Como conectar com banco?
Exemplo:

JSON
{
  "file_id": "1",
  "drone_id": "D1",
  "mission_id": "M1",
  "url": "s3://bucket/img.jpg"
}
