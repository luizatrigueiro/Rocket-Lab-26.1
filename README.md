# 🚀 Visagio | Rocket Lab 2026 - Desafio de Engenharia de Dados (Arquitetura Medalhão)

Este repositório contém a solução completa para o desafio de Engenharia de Dados do **Rocket Lab 2026** da Visagio. O objetivo principal deste projeto foi construir um pipeline de ETL/ELT robusto, estruturando dados brutos de um e-commerce em uma arquitetura Data Lakehouse no Databricks.

## 🎯 Objetivos Alcançados
- **Ingestão de Dados:** Carga de arquivos CSV (Kaggle Olist Dataset) e consumo de API (Cotação do Dólar - Banco Central).
- **Arquitetura Medalhão:** Implementação completa das camadas Bronze, Silver e Gold.
- **Governança e Qualidade:** Deduplicação sênior via Window Functions, tratamento de nulos, tipagem estrita e tolerância a falhas.
- **Modelagem de Dados:** Construção de Data Marts e agregação de métricas de negócio (Receita, Ticket Médio e Satisfação).
- **Orquestração:** Automação de pipelines com Databricks Workflows.

---

## 🛠️ Tecnologias Utilizadas
- **Plataforma:** Databricks
- **Processamento:** Apache Spark (PySpark) e Spark SQL
- **Armazenamento:** Delta Lake (Transações ACID, Time Travel e Otimização `ZORDER`)
- **Orquestração:** Databricks Jobs (Workflows)
- **Linguagem:** Python

---

## 📂 Estrutura do Repositório

* `notebooks/bronze.ipynb`: Notebook responsável pela ingestão bruta (arquivos e API) e criação do `timestamp_ingestion`.
* `notebooks/silver.ipynb`: Notebook focado em limpeza, traduções (inglês para PT-BR), deduplicações avançadas e preenchimento de série temporal (cotação do dólar).
* `notebooks/gold.ipynb`: Notebook com a modelagem final (Data Marts), agregações de negócio e geração de rankings (exibição via `display()`).
* `jobs/job.yaml`: Arquivo de configuração exportado do Job no Databricks.

---

## 🏗️ Arquitetura e Processamento (Camadas)

### 🥉 Camada Bronze 
- **Fonte:** CSVs do dataset Olist (Volumes Databricks) + API do Banco Central.
- **Processo:** Leitura automatizada, adição do metadado `timestamp_ingestion` e gravação direta no formato Delta (sem alterações estruturais ou limpeza).

### 🥈 Camada Silver 
- **Limpeza:** Padronização para português (colunas e status) e aplicação de letras maiúsculas.
- **Qualidade:** Deduplicação de registros de clientes, produtos e vendedores utilizando *Window Functions* ordenadas pelo momento da ingestão.
- **Série Temporal:** Criação de um calendário ininterrupto para a cotação do dólar, preenchendo finais de semana com a cotação da última sexta-feira útil.
- **Otimização:** Uso de `try_cast` e `try_to_timestamp` para tolerância a falhas e execução do comando `OPTIMIZE ZORDER` na tabela Fato principal.

### 🥇 Camada Gold 
Tabelas consolidadas para consumo de negócio:
1. **`fat_vendas_comercial`:** Visão analítica de receitas e volume por ano, mês e categoria (BRL e USD).
2. **`fat_avaliacoes_clientes`:** Indicadores de qualidade, taxa de satisfação e média de avaliações por vendedor, estado e categoria.

---

## ⚙️ Orquestração (Databricks Workflows)

O pipeline foi automatizado através de um Job no Databricks, garantindo a execução sequencial das tarefas todos os dias às 13:00.

### Dependências do Pipeline:
`bronze` ➔ `silver` ➔ `gold`

### Print da Execução com Sucesso:

> **Instrução:** ![WhatsApp Image 2026-04-04 at 16 29 26](https://github.com/user-attachments/assets/0b4dbe09-ceef-48f1-bf1c-614c7de4b25a)


---

## 📌 Autora
Desenvolvido por **Luiza Trigueiro**
* [https://www.linkedin.com/in/luiza-trigueiro/]
* [https://github.com/luizatrigueiro]
