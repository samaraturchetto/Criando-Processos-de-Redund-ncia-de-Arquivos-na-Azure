# 🏭 Criando Processos de Redundância de Arquivos na Azure com Data Factory

Este repositório faz parte do Desafio de Projeto da DIO **"Criando Processos de Redundância de Arquivos na Azure"**, do Bootcamp **"Microsoft AI for Tech - Azure Databricks"**.

---

## 📘 Introdução

Este projeto tem como objetivo criar um pipeline completo de redundância de arquivos utilizando recursos do **Microsoft Azure**. A proposta é guiar o usuário a configurar toda a infraestrutura necessária com **Azure Data Factory**, **SQL Server** (local e na nuvem) e **Azure Data Lake (Blob Storage)**, movimentando dados entre esses ambientes, criando arquivos `.txt` organizados por camadas de dados em containers (`bronze`, `prata`, `ouro`).

---

## ⚙️ Criação de Processos de Redundância de Arquivos na Azure – Data Factory

### 1. Configuração do Ambiente

#### 1.1 Criar Recursos no Azure
- Azure Data Factory (ADF)  
- Azure SQL Database  
- Azure Blob Storage  
  - Criar containers: `bronze`, `prata`, `ouro`

#### 1.2 Preparar o Ambiente On-Premises (local)
- Ter o SQL Server local com banco e tabelas disponíveis  
- Verificar acesso administrativo ao servidor para configurar o Integration Runtime  

---

### 2. Configurar Integration Runtime Self-hosted

- No Azure Data Factory, vá em **Manage > Integration Runtimes > New**
- Escolha **Self-hosted** e siga o assistente:
  - Baixe o instalador (modo express ou manual)
  - Instale no servidor local (on-premises)
  - Configure com a chave gerada pelo ADF
  - Verifique se o status está como “Running”

---

### 3. Criar Linked Services

#### 3.1 Para o SQL Server On-Premises
- Vá em **Manage > Linked Services > New > SQL Server**
- Utilize o Integration Runtime criado
- Insira servidor, banco, autenticação e teste a conexão

#### 3.2 Para o Azure SQL Database
- Criar outro Linked Service apontando para o banco na nuvem

#### 3.3 Para o Azure Blob Storage
- Criar um Linked Service usando a Storage Account
- Inserir a chave de acesso da conta

---

### 4. Criar Datasets

#### 4.1 Dataset de Origem
- Criar um dataset do tipo **SQL Server Table**
- Escolher o Linked Service do SQL Server on-premises
- Definir a tabela que será copiada

#### 4.2 Dataset de Destino
- Criar um dataset do tipo **DelimitedText (.txt)**
- Escolher o Linked Service do Blob Storage
- Apontar para o container/caminho desejado (`bronze`, `prata`, `ouro`)
- Configurar delimitador e nome do arquivo

---

### 5. Criar Pipeline de Cópia

#### 5.1 Construir o Pipeline
- No Data Factory, ir para **Author > Pipelines > New pipeline**
- Adicionar a atividade **Copy Data**
  - **Source (origem):** Dataset do SQL Server local
  - **Sink (destino):** Dataset no Blob Storage
- Nomear e salvar

#### 5.2 Validar e Publicar
- Clique em **Validate** para checar erros
- Clique em **Publish All** para aplicar mudanças

---

### 6. Executar e Monitorar

#### 6.1 Executar Pipeline
- Rodar o pipeline manualmente (**Debug**) ou configurar um trigger (**Trigger Now**)

#### 6.2 Verificar Resultados
- Vá até o container do Azure Blob Storage
- Verifique se o arquivo `.txt` foi gerado com sucesso

#### 6.3 Analisar Performance
- No painel do Data Factory, abra a aba **Monitor**
- Verifique duração da execução, throughput, logs e outras informações

---

## 💡 Boas Práticas

- Configurar o **Integration Runtime** corretamente antes de usar Linked Services on-premises  
- Sempre testar conexões antes de criar datasets  
- Organizar os containers do Blob Storage em camadas (`bronze`, `silver`, `gold`)  
- Validar e publicar mudanças antes de executar  
- Documentar e salvar as chaves do IR  
- Monitorar performance para controle de custos  

---

## 🛠 Tecnologias Utilizadas

- Microsoft Azure (ADF, SQL, Blob Storage)  
- SQL Server  
- Data Factory Integration Runtime  
- Azure Portal  
