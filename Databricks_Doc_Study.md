
# 📚 Databricks - Certificação Data Engineer Associate

###### 19/07

## ✅ O que é o Databricks?

- Plataforma **Lakehouse** **multicloud** baseada no **Apache Spark**.
- Combina o melhor de **Data Lakes** e **Data Warehouses** em um único ambiente.
- Disponível nas principais clouds:
  - ✅ **Azure**
  - ✅ **AWS**
  - ✅ **Google Cloud**

---

## 🏞️ O que é Lakehouse?

| Conceito          | Descrição |
|-------------------|-----------|
| **Data Lake**     | Armazena grandes volumes de dados brutos (estruturados, semi-estruturados e não estruturados). |
| **Data Warehouse**| Armazena dados estruturados, otimizados para consultas analíticas. |
| **Lakehouse**     | Combina a flexibilidade do Data Lake com a performance analítica do Data Warehouse, possibilitando ingestão, processamento, análise e machine learning em um único ambiente. |

---

## ⚙️ Como funciona o Apache Spark?

- Motor de processamento distribuído em **cluster**, utilizado para processar grandes volumes de dados.
- Suporta diferentes formas de processamento:
  - ✅ **Batch Processing** → Processamento em lote.
  - ✅ **Stream Processing** → Processamento em tempo real (streaming).
- Possui integração nativa com linguagens como **Python (PySpark)**, **SQL**, **Scala** e **R**.

---

## 📊 Tipos de Dados

- **Dados Estruturados** → Ex: tabelas relacionais (SQL), arquivos **CSV**.
- **Dados Semi-estruturados** → Ex: **JSON**, **XML**, **Parquet**, **Avro**.
- **Dados Não Estruturados** → Ex: imagens, vídeos, áudios, textos livres.

---

## 📂 Databricks File System (DBFS)

- Sistema de arquivos virtual do Databricks que facilita o armazenamento e o acesso a dados no workspace.
- Abstrai o armazenamento em nuvem:
  - ✅ **S3 (AWS)**
  - ✅ **ADLS (Azure)**
  - ✅ **GCS (Google Cloud)**
- Acesso direto via `/dbfs/` nos notebooks.


# 💎 Delta Lakev

## O que é Delta Lake?

- **Delta Lake** é uma camada de armazenamento transacional sobre arquivos **Parquet**.
- Estrutura base:
  - ✅ **Delta Table** = Arquivos **Parquet** + **Transaction Log** (`_delta_log`).
- Benefícios principais:
  - ✅ Transações **ACID**
  - ✅ Controle de versões (**Time Travel**)
  - ✅ Governança de dados

---

## 🎁 Vantagens do Delta Lake

- ✅ **Transações ACID** → Consistência garantida mesmo em operações paralelas.
- ✅ **Time Travel** → Consultas em versões anteriores da tabela.
- ✅ **Schema Enforcement** → Mantém a integridade do esquema.
- ✅ **Schema Evolution** → Evolui o esquema sem quebra de processos.
- ✅ **Performance otimizada** com **OPTIMIZE** e **ZORDER**.
- ✅ Suporte nativo para **batch** e **streaming**.

---

## ⏳ O que é Time Travel?

- Recurso que permite consultar versões anteriores de uma tabela Delta.
- Permite análise histórica, auditoria ou recuperação de dados.
- **Exemplos**:
```sql
SELECT * FROM tabela_delta VERSION AS OF 10;
SELECT * FROM tabela_delta TIMESTAMP AS OF '2024-06-01';
```

---

## 🚀 O que é OPTIMIZE e ZORDER?

- **OPTIMIZE**: 
  - Compacta arquivos pequenos em arquivos maiores, reduzindo a quantidade de arquivos e melhorando a performance de leitura.
  - Exemplo:
    ```sql
    OPTIMIZE tabela_delta;
    ```

- **ZORDER**: 
  - Técnica utilizada com **OPTIMIZE** para reorganizar fisicamente os dados em disco, priorizando colunas mais utilizadas em filtros e melhorando a performance de consultas seletivas.
  - Exemplo:
    ```sql
    OPTIMIZE tabela_delta ZORDER BY (coluna_importante);
    ```

---

## 🧹 Como funciona o comando VACUUM?

- **VACUUM** remove arquivos antigos não referenciados pelo Delta Lake, liberando espaço em disco.
- Padrão de retenção:
  - ✅ 7 dias (168 horas), para garantir **Time Travel**.
- Exemplo de comando:
```sql
VACUUM tabela_delta RETAIN 168 HOURS;
```
- ⚠️ **Atenção**: reduzir o tempo de retenção pode impedir consultas em versões anteriores.

---

✅ **Dica Final**: sempre valide se as configurações de otimização e retenção estão alinhadas com as necessidades do seu negócio para evitar perda de dados históricos.

## Entidades Relacionais

Managed Tables
External tables 

![alt text](image.png)

--- 
###### 21/07

## CTAS

**O que é CTAS em Databricks?**  
CTAS (Create Table As Select) é um comando SQL que cria uma nova tabela com base no resultado de uma consulta. Em Databricks, ele é usado para criar tabelas a partir de transformações de dados existentes.

**Por que usá-lo?**  
- Cria tabelas de forma eficiente a partir de consultas complexas  
- Permite otimizar o armazenamento de dados processados  
- Útil para criar tabelas derivadas ou snapshots de dados  

## Constraint 

**Quando adicionar constraints?**  
Constraints são adicionados para:  
- Garantir integridade dos dados  
- Definir relações entre tabelas  
- Validar dados antes da inserção  

**Vantagens:**  
- Melhora a qualidade dos dados  
- Documenta regras de negócio no esquema  
- Otimiza algumas consultas (em alguns sistemas)  

**Desvantagens:**  
- Pode impactar performance em operações DML  
- Algumas restrições não são aplicadas em Delta Lake  
- Requer validação adicional durante ETL  

## Clonning

#### Deep vs Shadow Clone  
**Vantagens de cada um:**  
- **Deep Clone:**  
  - Cria cópia completa dos dados e metadados  
  - Independente do original  
  - Útil para backups ou testes com dados reais  

- **Shadow Clone:**  
  - Copia apenas a estrutura (metadados)  
  - Mais rápido e econômico  
  - Ideal para desenvolvimento de esquemas  

**Quando usar cada um?**  
- Use **Deep Clone** quando precisar de uma cópia fiel dos dados  
- Use **Shadow Clone** para testes de estrutura sem necessidade dos dados reais  

## Views

1. **Stored Views**  
   - Persistidas no catálogo  
   - Visíveis para todos os usuários com permissão  

2. **Temporary Views**  
   - Existem apenas durante a sessão atual  
   - Visíveis apenas para a sessão que as criou  

3. **Global Temporary Views**  
   - Persistem enquanto o cluster estiver ativo  
   - Visíveis para todas as sessões no mesmo cluster  

#### Quando a sessão do Spark é reiniciada?  
- Quando o cluster é reiniciado  
- Após timeout de inatividade configurado  
- Quando explicitamente terminada pelo usuário  

![Diagrama de Views](image-2.png)

# Query Files
