# Projeto – spark-delta-minio-sqlserver

Este projeto implementa um pipeline de dados baseado em arquitetura de Data Lake,
utilizando Apache Spark, Delta Lake, MinIO e SQL Server.

O objetivo é demonstrar ingestão de dados, persistência transacional e operações
DML sobre tabelas Delta Lake, incluindo histórico e Time Travel.

---

## Arquitetura Geral

- **Origem**: SQL Server (banco relacional)
- **Landing Zone**: MinIO (CSV)
- **Camada Bronze**: Delta Lake (ACID)
- **Processamento**: Apache Spark (PySpark)

---

## Exemplo: Leitura de dados do SQL Server

```python
df_customers = (
    spark.read.format("jdbc")
    .option("url", jdbc_url)
    .option("dbtable", "Customers")
    .option("user", db_user)
    .option("password", db_password)
    .option("driver", "com.microsoft.sqlserver.jdbc.SQLServerDriver")
    .load()
)
```

# Projeto – spark-delta-minio-sqlserver

Este projeto implementa um pipeline de dados baseado em arquitetura de Data Lake,
utilizando Apache Spark, Delta Lake, MinIO e SQL Server.

O objetivo é demonstrar ingestão de dados, persistência transacional e operações
DML sobre tabelas Delta Lake, incluindo histórico e Time Travel.

---

## Arquitetura Geral

- **Origem**: SQL Server (banco relacional)
- **Landing Zone**: MinIO (CSV)
- **Camada Bronze**: Delta Lake (ACID)
- **Processamento**: Apache Spark (PySpark)

---

## Exemplo: Leitura de dados do SQL Server

```python
df_customers = (
    spark.read.format("jdbc")
    .option("url", jdbc_url)
    .option("dbtable", "Customers")
    .option("user", db_user)
    .option("password", db_password)
    .option("driver", "com.microsoft.sqlserver.jdbc.SQLServerDriver")
    .load()
)
```

## Exemplo: Escrita no MinIO (Landing Zone)

```python
df_customers.write \
    .mode("overwrite") \
    .option("header", "true") \
    .csv("s3a://landing-zone/Customers")
```

## Exemplo: Conversão de CSV para Delta Lake (Bronze)

```python

df_csv = spark.read \
    .option("header", "true") \
    .csv("s3a://landing-zone/Customers")

df_csv.write \
    .format("delta") \
    .mode("overwrite") \
    .save("s3a://bronze/Customers")
```

## Exemplo: INSERT em Delta Lake (Spark SQL)

```python
spark.sql("""
    INSERT INTO Customers VALUES
    (11, 'Lucas', 'Ferreira', '1991-03-12',
     'lucas.ferreira@email.com', '555-1121', 'Rua das Flores, 100')
""")
```

## Exemplo: UPDATE em Delta Lake (DeltaTable API)

```python
from delta.tables import DeltaTable
from pyspark.sql.functions import lit

dt_customers = DeltaTable.forPath(
    spark, "s3a://bronze/Customers"
)

dt_customers.update(
    condition="customer_id = 11",
    set={
        "first_name": lit("Lucas Henrique"),
        "email": lit("lucas.h.ferreira@email.com")
    }
)
```

## Exemplo: DELETE em Delta Lake

```python
dt_customers.delete("customer_id = 13")
```

## Exemplo: Histórico da Tabela (Delta Lake)

```python
dt_customers.history() \
    .select("version", "timestamp", "operation") \
    .show(truncate=False)
```

## Exemplo: Histórico via Spark SQL

```python
spark.sql("""
    DESCRIBE HISTORY Customers
""").select("version", "timestamp", "operation").show(truncate=False)
```

## Exemplo: Time Travel (versionAsOf)

```python
df_v0 = spark.read \
    .format("delta") \
    .option("versionAsOf", 0) \
    .load("s3a://bronze/Customers")

df_v0.show()
```

## Comparação: Versão Original vs Atual

```python
df_original = spark.read \
    .format("delta") \
    .option("versionAsOf", 0) \
    .load("s3a://bronze/Customers")

df_atual = spark.read \
    .format("delta") \
    .load("s3a://bronze/Customers")

print(f"Versão 0: {df_original.count()} registros")
print(f"Atual:    {df_atual.count()} registros")

df_atual.subtract(df_original).show()
```
