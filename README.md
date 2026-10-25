# Arquitetura de Coleta e Armazenamento de Dados — MBA UNIPÊ
Projeto prático desenvolvido para a disciplina de Arquitetura de Coleta e Armazenamento de Dados, implementando um pipeline analítico ponta a ponta baseado na **Arquitetura Medalhão** (Bronze, Silver e Gold) em ambiente local.
---
## 👤 Autoria
* **Discente:** Giovana Yngrid
* **Instituição:** Centro Universitário de João Pessoa (UNIPÊ)
* **Curso:** MBA em Engenharia e Análise de Dados
---

## 🏛️ Visão Geral da Arquitetura
O ambiente utiliza armazenamento de objetos compatível com S3, tabelas abertas de alta performance e um banco relacional para consumo analítico:
* **Landing Zone (MinIO):** Ingestão de ficheiros brutos (JSON, CSV e Parquet) via API compatível com S3.
* **Camada Bronze (Apache Iceberg):** Persistência integral dos dados brutos com rastreabilidade de linhagem (`_ingest_ts`, `_source_file`)[cite: 3].
* **Camada Silver (Apache Iceberg):** Limpeza, tipagem uniforme de dados, deduplicação e verificações de integridade.
* **Camada Gold (Apache Iceberg):** Construção de modelos dimensionais agregados (`vendas_diarias`, `clientes_360`, `avaliacoes_resumo`).
* **Serving Layer (PostgreSQL):** Disponibilização dos dados da camada Gold via JDBC para visualização de painéis no **Metabase**.

---

## 🛠️ Tecnologias Utilizadas

* **Linguagem & Processamento:** Python 3.10[cite: 1, 2], PySpark 3.5
* **Formato de Tabela (Lakehouse):** Apache Iceberg 1.6.1[cite: 3]
* **Object Storage:** MinIO (Docker)[cite: 1]
* **Base de Dados Relacional:** PostgreSQL 16 (Docker)[cite: 5]
* **Visualização Analítica:** Metabase (Docker)
* **Gestão de Infraestrutura:** Docker Compose

---

## 📁 Estrutura do Repositório

```text
├── common/             # Funções de suporte, ligação e regras de qualidade
├── data/               # Scripts de geração e lotes de dados brutos
├── docker/             # Ficheiro docker-compose.yml e scripts de inicialização
├── notebooks/          # Cadernos interativos do pipeline
│   ├── 01_bronze.ipynb # Carga da Landing Zone e gravação na camada Bronze
│   ├── 02_silver.ipynb # Tratamento, limpeza e persistência na camada Silver
│   └── 03_gold.ipynb   # Agregações analíticas e publicação no PostgreSQL
├── scripts/            # Scripts de automação (criar_buckets.py, preparar_jars.py)
├── .gitignore          # Regras de exclusão de ficheiros locais e temporários
└── requirements.txt    # Lista de dependências Python do projeto
