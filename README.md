Aqui está o **README** atualizado com base no vídeo do link fornecido, no conteúdo do desafio, nas alterações realizadas no chat, e com a adição do print da tela salvo como PDF:

---

# **Star Schema - Cenário de Vendas**

Este projeto apresenta a criação de um **Star Schema** (Esquema em Estrela) para um cenário de análise de vendas utilizando o Power BI. O objetivo foi construir uma estrutura dimensional que permita realizar análises detalhadas e eficientes, integrando tabelas fato e dimensões, e implementando relacionamentos no Power BI.

---

## **Objetivo do Projeto**

O objetivo principal foi criar um modelo dimensional para análise de vendas, com foco em:
- Desenvolver um **Star Schema** a partir de tabelas relacionais.
- Criar tabelas fato e dimensões bem estruturadas.
- Estabelecer relações e realizar transformações no Power BI usando DAX e Power Query.
- Conectar tabelas de vendas e detalhes utilizando colunas calculadas e chaves compostas.

---

## **Descrição do Modelo**

### **Tabela Fato**
**f_Vendas**
- Contém os dados principais das vendas realizadas.
- Principais colunas:
  - `Date` (DATA): Data da venda.
  - `Country` (TEXTO): País onde ocorreu a venda.
  - `Segment` (TEXTO): Segmento de mercado.
  - `Sales` (NÚMERO): Valor total das vendas.
  - `Units Sold` (NÚMERO): Quantidade de unidades vendidas.

---

### **Tabelas Dimensão**

1. **d_Detalhes**
   - Contém informações detalhadas sobre os produtos vendidos.
   - Principais colunas:
     - `Country` (TEXTO): País de venda.
     - `Segment` (TEXTO): Segmento de mercado.
     - `Sales` (NÚMERO): Valor total de vendas para o segmento.

2. **d_Calendar**
   - Criada para facilitar análises temporais.
   - Colunas geradas:
     - `Date` (DATA): Data única para cada linha.
     - `Year` (NÚMERO): Ano correspondente à data.
     - `Month Name` (TEXTO): Nome do mês.
     - `Month Number` (NÚMERO): Número do mês.
     - `Quarter` (TEXTO): Trimestre (Q1, Q2, etc.).
   - Fórmula DAX utilizada para gerar a tabela:
     ```DAX
     Calendar = 
     ADDCOLUMNS(
         CALENDAR(MIN(f_Vendas[Date]), MAX(f_Vendas[Date])),
         "Year", YEAR([Date]),
         "Month Name", FORMAT([Date], "MMMM"),
         "Month Number", MONTH([Date]),
         "Quarter", SWITCH(TRUE(), 
             [Month Number] <= 3, "Q1",
             [Month Number] <= 6, "Q2",
             [Month Number] <= 9, "Q3",
             "Q4"
         )
     )
     ```

---

### **Relacionamentos**

1. **d_Detalhes e f_Vendas**
   - Criada uma coluna chamada `Chave_Conexão` para conectar as tabelas com base nas colunas `Country`, `Segment` e `Sales`.
   - Fórmula Power Query para a coluna:
     ```powerquery
     [Country] & "|" & [Segment] & "|" & Text.From([Sales])
     ```
   - Configurado um relacionamento de **muitos para um**, com d_Detalhes filtrando f_Vendas.

2. **d_Calendar e f_Vendas**
   - Relacionamento estabelecido entre as colunas `Date` de ambas as tabelas.
   - Direção de filtro configurada como **unidirecional** para garantir a consistência do modelo.

---

## **Passos Realizados no Power BI**

1. **Transformação e Limpeza de Dados**
   - Tipos de dados foram corrigidos nas tabelas d_Detalhes e f_Vendas.
   - Colunas calculadas foram criadas para gerar chaves compostas e relacionar tabelas.

2. **Criação da Tabela Calendar**
   - Foi utilizada a função DAX `CALENDAR()` para gerar datas entre o menor e o maior valor presente na tabela fato.

3. **Relacionamento das Tabelas**
   - Conexões estabelecidas com base em chaves compostas e colunas comuns.
   - Relacionamentos otimizados para análise em visuais.

---

## **Imagem de Referência**

Abaixo está uma captura de tela do modelo criado no Power BI, mostrando os relacionamentos entre as tabelas e a estrutura do Star Schema:

![Diagrama do Star Schema](images/PowerBI_Screenshot.pdf)

---

## **Benefícios do Modelo**

- **Análises Temporais**: A tabela Calendar permite explorar vendas por períodos, como anos, meses e trimestres.
- **Relatórios Consistentes**: O uso de dimensões bem definidas e relações otimizadas garante visualizações claras e rápidas.
- **Flexibilidade**: A estrutura em estrela é adaptável para incluir novas dimensões ou medidas.

---

## **Como Reproduzir**

1. **Pré-requisitos**:
   - Instale o Power BI Desktop.
   - Baixe os arquivos necessários do repositório.

2. **Passos**:
   - Carregue os dados (tabelas d_Detalhes e f_Vendas).
   - Siga as fórmulas e relacionamentos descritos para criar o modelo.
   - Utilize os visuais do Power BI para explorar as análises.

---

## **Referências**
Este projeto foi desenvolvido com base no desafio do curso ["Modelagem e Transformação de Dados com DAX e Power BI"](https://web.dio.me/lab/modelagem-e-transformacao-de-dados-com-dax-com-power-bi/learning/b1c8ea6c-0b78-4d47-a873-9d911d79660b?back=/track/coding-future-suzano-analise-dados).

---

Se precisar de ajustes ou mais informações, é só avisar!
