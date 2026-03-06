# NorteSaúde - Inteligência na Prevenção de Internações (Norte de Minas)

![Hackathon Status](https://img.shields.io/badge/Status-Hackathon--Escola--de--Ver%C3%A3o-orange)
![Power BI](https://img.shields.io/badge/Tools-Power%20BI%20%7C%20Power%20Query%20%7C%20DAX-yellow)
![Data](https://img.shields.io/badge/Data%20Source-SIH%20%7C%20SCNES%20%7C%20IBGE-blue)

Este projeto foi desenvolvido pela equipe **Knights of Ni** durante o **Hackathon Escola de Verão (06 de Março de 2026)**. O objetivo é fornecer uma ferramenta de suporte à decisão para gestores de saúde, focada na identificação e redução de **Internações por Condições Sensíveis à Atenção Primária (ICSAP)** no Norte de Minas Gerais.

---

## 📌 Contexto e Problema
Doenças evitáveis que poderiam ser tratadas na Atenção Primária (UBS) frequentemente evoluem para estados avançados, gerando internações desnecessárias. Isso causa a superlotação de hospitais polo, como os de **Montes Claros**, aumenta o deslocamento de pacientes de cidades satélites e eleva drasticamente os custos públicos.

O dashboard permite identificar quais municípios e diagnósticos demandam intervenção estratégica imediata, otimizando a alocação de recursos e profissionais.

---

## 🛠️ Tratamento e Engenharia de Dados

O processo de ETL (Extração, Transformação e Carga) foi realizado integralmente no **Power Query**, integrando bases de dados do sistema de saúde e dados geográficos.

### 1. Integração de Bases
As seguintes tabelas foram relacionadas para compor o modelo:
*   **SIH_EIXO_1:** Base principal contendo o fluxo de internações hospitalares.
*   **CID_10_CATEGORIAS:** Tabela de referência para a classificação internacional de doenças (categorias).
*   **ISCAP:** Lista Brasileira de Internações por Condições Sensíveis à Atenção Primária.
*   **IBGE:** Base de códigos de municípios de Minas Gerais ([Link oficial](https://www.ibge.gov.br/explica/codigos-dos-municipios.php)).

### 2. Normalização de Códigos (Chaves de Ligação)
Para garantir o relacionamento correto entre as fontes, aplicamos as seguintes regras:
*   **Ajuste CID (3 vs 4 caracteres):** A tabela de categorias do CID-10 possui códigos de 3 dígitos, enquanto a base SIH possui códigos de 4 caracteres. Criamos a coluna **`cid_manipulado`** na base SIH, removendo o quarto dígito para permitir o cruzamento perfeito dos diagnósticos.
*   **Ajuste Municípios (6 vs 7 dígitos):** O IBGE utiliza códigos de 7 dígitos (incluindo dígito verificador), enquanto o padrão SIH utiliza 6 dígitos. Removemos o último dígito dos códigos IBGE para compatibilizar as bases geográfica e hospitalar.

### 3. Colunas Calculadas e Regras de Negócio
Foram criadas colunas específicas para enriquecer a análise:
*   **Booleanas:** 
    *   `Evitavel`: Identifica se a internação pertence à lista ISCAP.
    *   `Morreu`: Identifica se o desfecho da internação foi óbito.
*   **Dimensionais:**
    *   `Idade`: Calculada a partir da data de nascimento do paciente.
    *   `Origem do Paciente`: Classifica o fluxo entre residentes de Montes Claros ou de cidades satélites.
    *   `Data da Internação`: Conversão para o formato *DateTime* para análises temporais granulares.
    *   `Tempo de Internação`: Calculado em dias (permanência hospitalar).
    *   `Nome do Diagnóstico`: Atribuição textual do CID para facilitar a leitura gerencial.

---

## 🚀 Funcionalidades do Dashboard
*   **Métricas de Prioridade:** Uso do *Score de Prioridade* (Volume x % Evitabilidade) para destacar municípios críticos.
*   **Árvore de Decomposição:** Navegação hierárquica por origem, município e diagnóstico.
*   **Análise de Custos:** Identificação do impacto financeiro direto causado por doenças que poderiam ser evitadas.
*   **Série Temporal:** Acompanhamento mensal da evolução de internações evitáveis vs. totais.

---

## 👥 Equipe: Knights of Ni
*   **Ivanderlei Mendes Silva Filho** - Ciência da Computação
*   **Maicon Pedro Macedo Leles** - Ciência da Computação
*   **Thiago Rodrigues Farias** - Engenharia Elétrica

---
*Desenvolvido para o Hackathon Escola de Verão - 06 de Março de 2026.*
