# 📊 Case PROART - Análise de Pesquisa Organizacional

## 📋 Descrição

Este projeto realiza a leitura, organização e tratamento de dados de uma pesquisa organizacional (PROART) conduzida em uma empresa industrial. A análise busca compreender o clima organizacional, os estilos de gestão, e os impactos do ambiente de trabalho na saúde física, mental e social dos colaboradores.

A pesquisa abrange diferentes áreas funcionais (Operação, Manutenção, Logística, RH, Segurança do Trabalho e Administrativo) e níveis hierárquicos, fornecendo uma perspectiva holística da realidade organizacional.

## 🎯 Objetivos

- **Limpeza e organização** dos dados brutos provenientes da pesquisa organizacional
- **Transformação de variáveis** numéricas em categóricas para facilitar interpretação
- **Validação de integridade** dos dados (detecção de duplicatas, valores nulos e inconsistências)
- **Mapeamento estruturado** das questões da Escala PROART por fatores de análise
- **Preparação de dados** para análises avançadas em ferramentas de Business Intelligence (PowerBI)
- **Estruturação** de metadados para facilitar análises multidimensionais

## 📊 Dataset

### Origem dos Dados
Os dados foram coletados através de um formulário de pesquisa disponibilizado aos colaboradores da organização, capturando informações sobre perfil demográfico, características profissionais e percepções sobre o ambiente organizacional.

### Dimensão dos Dados
- **Registros totais**: 473 respostas
- **Respostas completas**: 418 registros (utilizados na análise)
- **Respostas incompletas**: 55 registros (filtradas)
- **Variáveis**: 19 colunas no dataset processado

### Principais Colunas Analisadas

#### Informações Demográficas
| Coluna | Tipo | Exemplo de Valores |
|--------|------|-------------------|
| ID de resposta | Identificador | ID único para cada respondente |
| Gênero | Categórica | Homem, Mulher, Pessoa não-binária, Prefiro não responder, Outro |
| Faixa etária | Categórica | Menos de 19 anos, 19 a 24 anos, 25 a 34 anos, 35 a 44 anos, 45 a 54 anos, 55 a 64 anos, 65 anos ou mais |
| Cor/raça autodeclarada | Categórica | Branca, Preta, Amarela, Parda, Indígena, Prefiro não responder |
| Escolaridade | Categórica | Ensino fundamental até Pós-graduação completa |

#### Informações Profissionais
| Coluna | Tipo | Exemplo de Valores |
|--------|------|-------------------|
| Turno | Categórica | Administrativo, Turno rotativo |
| Área | Categórica | Operação, Manutenção, Logística, RH, Segurança do Trabalho, Administrativo, Diretoria |
| Cargo | Categórica | Operador(a) de Produção, Técnico(a) de Manutenção, Analista de RH, Gerente Geral, etc. |
| Tempo na empresa | Categórica | Menos de 1 ano até Mais de 15 anos |
| Tempo na função atual | Categórica | Menos de 1 ano até Mais de 15 anos |

#### Status de Resposta
| Coluna | Tipo | Valores |
|--------|------|--------|
| Status de resposta | Categórica | Completed, Incomplete |

### Tipos de Dados Utilizados
- **String/Texto**: Identificadores e respostas categóricas
- **Numérico**: Codificação das opções de resposta
- **Data/Hora**: Timestamp das respostas
- **Escala Likert**: Questões avaliadas em escala de 5 pontos

## 🔧 Metodologia

### 1. Importação de Bibliotecas
```python
import pandas as pd
```
Utiliza a biblioteca Pandas para manipulação e transformação de dados em formato tabular.

### 2. Carregamento dos Dados
- Leitura do arquivo Excel (`dados-brutos-assistente-de-dados.xlsx`)
- Verificação inicial das dimensões: 473 linhas × 20 colunas

### 3. Limpeza e Tratamento de Dados

#### 3.1 Transformação de Variáveis Numéricas em Categóricas
Implementação de dicionários de mapeamento para as seguintes variáveis:
- **Turno**: Conversão de codes (1, 2) em categorias (Administrativo, Turno rotativo)
- **Gênero**: Conversão de 5 categorias em descrições descritivas
- **Cor/raça autodeclarada**: Mapeamento de 6 categorias de auto-identificação
- **Faixa etária**: Transformação de 7 faixas numéricas em intervalos etários
- **Escolaridade**: Conversão em 7 níveis educacionais
- **Tempo na empresa**: Mapeamento em 6 períodos de permanência
- **Tempo na função**: Mapeamento em 6 períodos de permanência

#### 3.2 Limpeza de Valores
- Remoção de formatação numérica (`.0` dos valores convertidos)
- Remoção de espaços em branco nas extremidades das strings

#### 3.3 Criação de Colunas Derivadas
- Decomposição do código único de cargo/área em duas colunas separadas
- **Coluna "Área"**: Agrupamento de cargos em 7 áreas funcionais
- **Coluna "Cargo"**: Identificação de 19 posições específicas

#### 3.4 Tratamento de Dados Faltantes
- Verificação de duplicatas: 0 registros duplicados
- Detecção de valores nulos: Identificação seletiva
- Remoção de coluna irrelevante: Exclusão da coluna "Outro" (100% nulos)
- Filtragem de respostas completas: Redução a 418 registros válidos

### 4. Análise Exploratória

#### 4.1 Distribuição de Status de Resposta
- **Completed**: 418 respostas (88.4%)
- **Incomplete**: 55 respostas (11.6%)
- Decisão: Utilização apenas de respostas completas para garantir integridade analítica

#### 4.2 Reorganização de Estrutura
- Posicionamento de colunas categóricas no início do dataframe
- Posicionamento de colunas numéricas ao final
- ID de resposta como primeira coluna para referência

### 5. Construção de Indicadores

#### 5.1 Mapeamento da Escala PROART
Estruturação de 9 fatores organizacionais a partir de 89 questões:

| Fator | Questões | Descrição |
|-------|----------|-----------|
| Organização Prescrita - Divisão de Tarefas | 7 | Adequação de recursos, equipamentos, espaço físico e ritmo |
| Organização Prescrita - Divisão Social | 12 | Comunicação, autonomia, flexibilidade e clareza nas tarefas |
| Estilos de Gestão - Individualista | 10 | Centralização, controle e hierarquia na gestão |
| Estilos de Gestão - Coletivista | 11 | Trabalho em equipe, valorização e inovação |
| Sofrimento - Falta de Sentido | 9 | Significado, relevância e motivação no trabalho |
| Sofrimento - Esgotamento Mental | 8 | Desgaste, cansaço e insatisfação |
| Sofrimento - Falta de Reconhecimento | 11 | Liberdade de expressão, reconhecimento e relacionamento |
| Danos - Psicológicos | 6 | Impactos emocionais negativos |
| Danos - Sociais | 8 | Impactos nas relações interpessoais |
| Danos - Físicos | 9 | Manifestações somáticas do mal-estar |

**Total: 89 questões mapeadas em 9 fatores estruturais**

#### 5.2 Escala de Resposta PROART
Implementação de escala Likert de 5 pontos:
1. **Nunca**
2. **Raramente**
3. **Às vezes**
4. **Frequentemente**
5. **Sempre**

### 6. Cálculos Estatísticos

#### 6.1 Identificação de Faltantes
- Cálculo de contagem de valores nulos por coluna
- Ordenação decrescente para priorização de tratamento
- Validação de ausência de dados nulos após filtragem

#### 6.2 Distribuição de Respondentes
- Contagem de respostas por status
- Cálculo de proporções e percentuais
- Validação de integridade dimensional

### 7. Visualizações

O notebook gera visualizações em HTML e texto plano que possibilitam:
- Exibição estruturada dos primeiros registros (`head()`)
- Visualização de tabelas formatadas do pandas
- Output de print statements para validações

### 8. Principais Análises Realizadas

#### 8.1 Análise de Qualidade dos Dados
- Verificação de integridade estrutural (duplicatas, nulos)
- Validação de mapeamento de códigos
- Confirmação de filtros de completude

#### 8.2 Análise Descritiva
- Contagem de respondentes por status
- Distribuição de áreas e cargos
- Proporção de gênero, faixa etária e escolaridade

#### 8.3 Análise de Fatores Organizacionais
- Estruturação de questões por fatores PROART
- Criação de metadados para análises multidimensionais
- Preparação de dataset para agregações em PowerBI

#### 8.4 Análise de Carreira
- Tempo de permanência na empresa
- Tempo de permanência na função
- Análise de progressão em diferentes áreas

## 💻 Tecnologias Utilizadas

### Linguagem
- **Python 3.x**: Linguagem principal para processamento de dados

### Bibliotecas Python
| Biblioteca | Versão | Finalidade |
|-----------|--------|-----------|
| **Pandas** | Latest | Manipulação, transformação e análise de dados tabulares |
| **OpenPyXL** | Latest | Leitura e escrita de arquivos Excel (.xlsx) |

### Ferramentas
- **Jupyter Notebook**: Ambiente interativo para desenvolvimento e documentação
- **Microsoft Excel**: Formato de armazenamento de dados (`.xlsx`)
- **Power BI**: Destino final para análises interativas (preparado neste projeto)

### Ambiente
- **Windows OS**: Sistema operacional de execução
- **VS Code**: Editor de código utilizado para desenvolvimento

## 📈 Principais Resultados

### Amostra Analisada
- **418 respondentes** com respostas completas
- **89 questões** estruturadas em **9 fatores**
- **7 áreas funcionais** cobertas
- **19 posições diferentes** representadas

### Estrutura de Dados Estabelecida
✅ Todas as variáveis categóricas transformadas de códigos numéricos para descrições intuitivas

✅ Colunas derivadas criadas (Área e Cargo) para facilitar segmentação

✅ Dados duplicados removidos

✅ Valores nulos identificados e tratados

✅ Dados reorganizados por tipo para melhor compreensão

✅ Metadados estruturados (Fator-Questão) para Power BI

### Indicadores Chave
- **Taxa de Completude**: 88.4% das respostas
- **Cobertura Dimensional**: 7 áreas × 19 cargos × múltiplos perfis demográficos
- **Factores Organizacionais**: 9 construtos principais mapeados
- **Variáveis Demográficas**: 5 dimensões de caracterização

### Entregáveis
1. **Arquivo Excel Processado**: `dados-tratados-assistente-de-dados.xlsx`
   - Aba 1: Dataset completo com 418 registros × 19 colunas
   - Aba 2: Mapping Fator-Questão (89 questões × 2 colunas)
2. **Preparação para PowerBI**: Estrutura pronta para análises multidimensionais

## 📁 Estrutura do Projeto

```
analise-pesquisa-organizacional/
│
├── Case_PROART_Passo_a_Passo.ipynb          # Notebook principal com todo o código
├── README.md                                 # Este arquivo
│
├── data/
│   └── dados-brutos-assistente-de-dados.xlsx # Dados brutos (entrada)
│
└── output/
    └── dados-tratados-assistente-de-dados.xlsx # Dados processados (saída)
        ├── Sheet 1: Dataset tratado (418 × 19)
        └── Sheet 2: Mapping Fator-Questão
```

## 🚀 Como Executar

### Pré-requisitos
- Python 3.7 ou superior instalado
- Jupyter Notebook ou JupyterLab
- Bibliotecas Python necessárias

### Passo 1: Instalação de Dependências
```bash
pip install pandas openpyxl jupyter
```

### Passo 2: Clonar/Baixar o Repositório
```bash
git clone https://github.com/seu-usuario/analise-pesquisa-organizacional.git
cd analise-pesquisa-organizacional
```

### Passo 3: Organizar Arquivos de Entrada
```
Certifique-se de que o arquivo 'dados-brutos-assistente-de-dados.xlsx' 
está na pasta 'data/' antes de executar o notebook
```

### Passo 4: Abrir o Notebook
```bash
jupyter notebook Case_PROART_Passo_a_Passo.ipynb
```

### Passo 5: Executar as Células
- Executar célula por célula (Shift + Enter) ou
- Executar todo o notebook (Kernel → Restart & Run All)

### Passo 6: Validar Saída
```
O arquivo 'dados-tratados-assistente-de-dados.xlsx' será 
gerado automaticamente na raiz do projeto com duas abas:
1. Dados tratados e limpos
2. Mapping de Fatores e Questões
```

## 📦 Instalação

### Opção 1: Instalação Individual de Pacotes
```bash
pip install pandas
pip install openpyxl
pip install jupyter
```

### Opção 2: Instalação via Requirements
Criar arquivo `requirements.txt`:
```
pandas>=1.3.0
openpyxl>=3.6.0
jupyter>=1.0.0
```

Instalar:
```bash
pip install -r requirements.txt
```

### Opção 3: Instalação via Conda
```bash
conda create -n proart-analysis python=3.9
conda activate proart-analysis
conda install pandas openpyxl jupyter
```

## 📊 Exemplos de Visualizações

### 1. Visualização de Primeiros Registros
```python
df.head()  # Exibe as primeiras 5 linhas do dataset
df_completed.head()  # Exibe dados após filtragem
```
**Resultado**: Tabela formatada HTML/Pandas mostrando estrutura de dados

### 2. Validação de Dimensões
```python
print(f"DIMENSÕES DOS DADOS: Linhas: {df.shape[0]} e Colunas: {df.shape[1]}")
```
**Resultado**: "DIMENSÕES DOS DADOS: Linhas: 473 e Colunas: 20"

### 3. Verificação de Valores Nulos
```python
null_counts = df.isnull().sum().sort_values(ascending=False)
print(null_counts[null_counts > 0])
```
**Resultado**: Coluna "Outro" com 473 nulos (100%)

### 4. Análise de Status de Resposta
```python
print(df['Status de resposta'].value_counts())
```
**Resultado**:
```
Completed    418
Incomplete    55
```

### 5. Tabela de Mapeamento de Fatores
```python
for fator, qs in questoes.items():
    print(f"• {fator}: {len(qs)} questões")
```
**Resultado**: Listagem de 9 fatores com quantidade de questões cada

## 🎓 Aprendizados

### Competências Técnicas Demonstradas

#### 1. **Manipulação de Dados com Pandas**
- Leitura de arquivos Excel
- Transformação de tipos de dados
- Seleção condicional e filtragem
- Reorganização estrutural de dataframes

#### 2. **Limpeza e Preparação de Dados (Data Cleaning)**
- Identificação e tratamento de duplicatas
- Detecção de valores nulos
- Remoção de colunas irrelevantes
- Validação de integridade de dados

#### 3. **Engenharia de Features (Feature Engineering)**
- Criação de colunas derivadas
- Decomposição de variáveis
- Mapeamento de código-descrição
- Transformação de escala numérica em categórica

#### 4. **Modelagem de Dados (Data Modeling)**
- Estruturação de metadados
- Design de tabelas de dimensão (Fator-Questão)
- Normalização de dados
- Preparação multidimensional para BI

#### 5. **Validação e Qualidade (Data Quality)**
- Verificação de dimensionalidade
- Contagem e análise de valores nulos
- Detecção de inconsistências
- Documentação de filtros aplicados

#### 6. **Análise Organizacional (Domain Knowledge)**
- Compreensão de Escala PROART
- Mapeamento de fatores organizacionais
- Categorização de áreas e cargos
- Análise de clima organizacional

#### 7. **Documentação e Comunicação**
- Código bem estruturado e comentado
- Explicação de transformações realizadas
- Justificativa de decisões de filtragem
- Preparação de saídas para stakeholders



## 👤 Autor

**Juliana Lourenço**



---

## 📝 Notas Finais

Este projeto representa uma análise completa de dados organizacionais usando a Escala PROART, estruturando dados brutos em informações preparadas para análises avançadas. O workflow implementado pode servir como template para projetos similares de pesquisa organizacional.

**Última atualização**: Junho 2026

---

