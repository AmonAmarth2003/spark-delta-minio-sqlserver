# Pipeline de Dados

## Etapas do Pipeline

1. **Carga inicial**
   - Arquivos CSV carregados no SQL Server

2. **Extração**
   - Leitura das tabelas do SQL Server
   - Escrita dos dados no MinIO (bucket `landing-zone`) em formato CSV

3. **Conversão**
   - Leitura dos CSVs do MinIO
   - Escrita no formato Delta Lake (bucket `bronze`)

4. **DML em Delta Lake**
   - INSERT
   - UPDATE
   - DELETE
   - Uso de Spark SQL e DeltaTable API

5. **Versionamento**
   - Consulta de histórico (`DESCRIBE HISTORY`)
   - Time Travel (`versionAsOf`)
   - Comparação entre versões
