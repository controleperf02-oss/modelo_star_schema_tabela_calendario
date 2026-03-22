# 📊 Desafio de Projeto: Modelagem Dimensional com Financial Sample

Este projeto consiste na estruturação de um modelo **Star Schema** a partir de uma tabela única (flat table) de amostras financeiras (*Financial Sample*). O objetivo é aplicar técnicas de transformação de dados e modelagem multidimensional.

---

## 🎯 Objetivo
Criar as tabelas dimensão e fato do nosso modelo baseado em star schema. O processo consiste na criação das tabelas com base na tabela original, selecionando as colunas que compõem a visão de cada nova entidade.

---

## 📂 Estrutura do Modelo

### 📑 Tabelas do Projeto:
* **Financials_origem**: Tabela original mantida em modo oculto como backup.
* **D_Produtos**: Contém métricas agrupadas de produtos.
* **D_Produtos_Detalhes**: Detalhamento técnico e de precificação dos produtos.
* **D_Descontos**: Informações sobre as faixas e valores de descontos.
* **D_Detalhes**: Informações complementares sobre as vendas.
* **D_Calendário**: Tabela de tempo criada via DAX.
* **F_Vendas**: Tabela fato com os registros de transações e chaves para as dimensões.

* ## 🖼️ Visualização do Modelo
* 📊 [Clique aqui para visualizar o Modelo Estrela](DOCS/modelo_estrela_power_bi.png)
---

## 🛠️ Etapas do Processo de Construção

### 1. 📦 Criação da tabela D_Produtos
* Duplicada a tabela `financials`.
* **Agrupar por**: Coluna `produtos`.
* **Colunas criadas**: Soma de unidades vendidas, Valor mínimo de venda, Valor máximo de venda, Média do valor de vendas, Mediana do valor de vendas e Média da manufatura.
* **Adicionar coluna**: Coluna de Índice.
* **Coluna Condicional**: Criada a coluna `ID_Produto`.

### 2. 💸 Criação da tabela D_Descontos
* Duplicada a tabela `financials`.
* **Colunas Removidas**: Segment, Country, Units Sold, Manufacturing Price, Sale Price, Gross Sales, Sales, COGS, Profit, Date, Month Number, Month Name, Year, Índice.
* **Coluna Condicional**: Criada a coluna `ID_Produto`.
* **Limpeza**: Removida coluna produto e removidas as duplicatas.

### 3. 🏷️ Criação da tabela D_Categoria
* Duplicada a tabela `financials`.
* **Colunas Removidas**: Product, Discount Band, Units Sold, Manufacturing Price, Sale Price, Gross Sales, Discounts, Sales, COGS, Profit, Date, Month Number, Month Name, Year, Índice.
* **Limpeza**: Removida duplicatas e adicionada coluna de Índice.
* *Obs: Tabela deletada posteriormente pois as informações já constavam na D_Detalhes.*

### 4. 💰 Criação da tabela F_Vendas
* Duplicada a tabela `financials`.
* **Coluna Condicional**: Criada a coluna `ID_Produto`.
* **Limpeza**: Removida duplicatas e removidas as colunas Manufacturing Price, COGS, Month Number, Discounts, Gross Sales, Índice.1.
* **Indexação**: Criada coluna de Índice renomeada para `SK_ID`.
* **Data**: Definida como *Short Date* e criada hierarquia de Year e Month Name.

### 5. 🔍 Criação da tabela D_Produtos_Detalhes
* Duplicada a tabela `financials`.
* **Colunas Removidas**: Segment, Country, Gross Sales, Discounts, Sales, COGS, Profit, Date, Month Number, Month Name, Year, Índice.
* **Transformação**: Adicionada coluna de Índice e coluna condicional `ID_Produto`.
* **Limpeza**: Removida duplicatas.

### 6. 📝 Criação da tabela D_Detalhes
* Duplicada a tabela `financials` para conter as informações remanescentes que detalham as vendas.

### 7. 🗄️ Backup
* Tabela `financials` renomeada para `financials_origem`.

---

## 📅 Criação da Tabela Calendário (DAX)

A tabela de calendário foi criada para suportar a inteligência de tempo no modelo.

### 📑 Colunas e Fórmulas:

* **Tabela base**: `Calendário = CALENDARAUTO(12)`
* **Ano**: `Year = YEAR('Calendário'[Date])`
* **Número da Semana**: `Week Number = WEEKNUM('Calendário'[Date])`
* **Número do Mês**: `Month Number = MONTH('Calendário'[Date])`
* **Nome do Mês**: `Month Name = FORMAT(DATE(1,'Calendário'[Month Number],1), "MMM")`
* **Dia da Semana**: `Day of the week = WEEKDAY('Calendário'[Date])`
* **Dia da Semana (Extenso)**: `Day of the week 2 = FORMAT('Calendário'[Date], "DDDD")`

---

## 🚀 Tecnologias Utilizadas
* **Power BI**
* **Power Query** (M)
* **DAX** (Data Analysis Expressions)

---
✨ *Projeto finalizado e pronto para análise de dados financeiros.*
