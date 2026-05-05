# spark-delta-minio-sqlserver

## Integrantes
- William Manoel Bitencourt Mesquita

## Sobre o Projeto
Este projeto tem como objetivo demonstrar o ciclo de vida e o pipeline de dados utilizando Apache Spark, Delta Lake, MinIO e SQL Server.  

O trabalho consiste na extração de dados de um banco de dados relacional (SQL Server), armazenamento intermediário em arquivos CSV no MinIO (landing zone), conversão desses dados para o formato Delta Lake (bronze layer) e execução de operações DML (INSERT, UPDATE e DELETE) diretamente sobre tabelas Delta.

O repositório foi desenvolvido com base no template fornecido pelo professor, sendo a estrutura do banco de dados uma evolução de um projeto anterior, com melhorias no modelo relacional e na organização das entidades.

## Tecnologias Utilizadas
- **Apache Spark (PySpark) 3.5.3** – processamento distribuído de dados
- **Delta Lake 3.2.0** – camada de armazenamento transacional sobre o data lake
- **MinIO** – armazenamento de objetos compatível com S3
- **SQL Server** – banco de dados relacional de origem
- **Docker & Docker Compose** – orquestração do ambiente
- **Python 3.11 com UV** – linguagem principal do projeto
- **Jupyter Notebook** – execução e documentação das etapas do pipeline

## Estrutura do Projeto
```text
spark-delta-minio-sqlserver/
├── data/
│   ├── Agent_Policies.csv
│   ├── Agents.csv
│   ├── Claims.csv
│   ├── Customers.csv
│   ├── Insurance_Types.csv
│   ├── Payments.csv
│   └── Policies.csv
├── notebook/
│   ├── 00_setup_sqlserver.ipynb         # Setup: CSV → SQL Server
│   ├── 01_sqlserver_to_minio_csv.ipynb  # Extração: SQL Server → MinIO (CSV)
│   ├── 02_csv_to_delta.ipynb            # Conversão: CSV → Delta Lake (bronze)
│   └── 03_dml_delta.ipynb               # DML: INSERT, UPDATE, DELETE + Time Travel
├── docs/
│   ├── ai.md
│   ├── index.md
│   ├── license.md
│   ├── pipeline.md
│   └── projeto.md
├── .env.example
├── docker-compose.yml
├── pyproject.toml
└── README.md
```

## Assistência por Inteligência Artificial

Este projeto contou com o apoio de ferramentas de Inteligência Artificial
durante o seu desenvolvimento.

A IA foi utilizada como **ferramenta de apoio**, principalmente para:
- Esclarecimento de conceitos técnicos
- Auxílio na escrita e organização de código
- Revisão de trechos de documentação
- Sugestões de boas práticas em Apache Spark, Delta Lake e SQL

Todas as decisões de arquitetura, implementação, testes e validações
foram realizadas pelo autor.

O uso de Inteligência Artificial teve caráter **auxiliar e educacional**,
não substituindo o entendimento, a autoria e a responsabilidade pelo
conteúdo apresentado neste repositório.


## Licença

Este projeto é distribuído sob a licença **MIT**.

É permitido:
- Usar, copiar, modificar e distribuir este projeto
- Utilizar o código para fins acadêmicos, pessoais ou profissionais

Não é permitido:
- Responsabilizar o autor por qualquer dano, perda de dados ou problema decorrente do uso deste projeto

Este repositório foi criado **exclusivamente para fins educacionais**.  
O autor **não se responsabiliza** por qualquer uso indevido ou consequências decorrentes da utilização do código.

Consulte o arquivo [LICENSE](LICENSE) para mais detalhes.