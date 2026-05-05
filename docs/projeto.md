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
)# Projeto – spark-delta-minio-sqlserver

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

##

