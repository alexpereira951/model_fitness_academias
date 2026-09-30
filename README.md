# Model Fitness — Análise de Churn e Segmentação de Clientes

Projeto de análise de dados desenvolvido para a **Model Fitness**, com foco na identificação de clientes com maior risco de cancelamento e na compreensão dos diferentes perfis de alunos da academia.

A análise combina **exploração de dados, modelos de classificação e técnicas de agrupamento** para transformar os dados de clientes em informações úteis para ações de retenção e engajamento.

## 🎯 Objetivo do Projeto

O principal objetivo é identificar sinais associados ao **churn (cancelamento)** e estimar quais clientes apresentam maior risco de evasão no mês seguinte.

Para isso, o projeto:

- realiza a inspeção e análise exploratória da base de clientes;
- compara variáveis entre clientes que permaneceram e clientes que cancelaram;
- treina modelos de classificação para prever churn;
- compara **Regressão Logística** e **Random Forest**;
- utiliza **agrupamento hierárquico** e **K-Means** para segmentar os clientes;
- analisa a taxa de cancelamento dos diferentes segmentos;
- propõe ações de retenção baseadas nos padrões observados.

---

## 🧠 Abordagem Técnica

### 1. Carregamento e inspeção dos dados

A base `gym_churn_us.csv` é carregada com **Pandas**. O notebook realiza verificações iniciais sobre:

- dimensões da base;
- tipos de dados;
- amostras dos registros;
- estatísticas descritivas;
- médias das variáveis por situação de churn.

O notebook também possui uma lógica para localizar a base em diferentes caminhos, facilitando sua execução a partir de diferentes diretórios.

### 2. Análise exploratória

Foram analisadas variáveis relacionadas ao perfil e ao comportamento dos clientes, incluindo:

- idade;
- tempo de contrato;
- meses restantes até o término do contrato;
- tempo de relacionamento com a academia;
- frequência média de visitas;
- frequência média no mês atual;
- gastos adicionais médios.

As distribuições são comparadas entre clientes com e sem churn, além de ser calculada a correlação das variáveis com a variável-alvo `Churn`.

### 3. Modelagem preditiva

A variável `Churn` é utilizada como variável-alvo.

A base é dividida em:

- **80% para treinamento**;
- **20% para teste**;
- divisão estratificada para preservar a proporção de churn.

Foram avaliados dois modelos:

**Regressão Logística**
- utiliza `StandardScaler` para normalização dos atributos;
- `max_iter=1000`;
- `random_state=42`.

**Random Forest**
- utiliza `200` árvores;
- `random_state=42`;
- processamento paralelo com `n_jobs=-1`.

A avaliação utiliza:

- acurácia;
- precisão;
- recall;
- F1-score;
- matriz de confusão;
- relatório de classificação.

### 4. Segmentação de clientes

Para compreender os diferentes perfis de clientes, os atributos são padronizados com `StandardScaler`.

Em seguida, são utilizados:

- **Agrupamento Hierárquico**, com método `Ward`, para construção do dendrograma;
- **K-Means**, configurado com `5` clusters, `n_init=10` e `random_state=42`.

Após a criação dos grupos, são analisados:

- quantidade de clientes por segmento;
- perfil médio de cada cluster;
- taxa de churn por segmento.

---

## 📊 Resultados

### Desempenho dos modelos

No conjunto de teste, composto por **800 registros**, os modelos apresentaram os seguintes resultados:

| Modelo | Acurácia |
|---|---:|
| Regressão Logística | **93%** |
| Random Forest | **92%** |

Para a classe de churn (`Churn = 1`), ambos os modelos apresentaram:

- **Precisão:** 0,88
- **Recall:** 0,83
- **F1-score:** 0,85

A matriz de confusão da Regressão Logística foi:

```text
[[564  24]
 [ 36 176]]
```

Isso representa **564 verdadeiros negativos, 24 falsos positivos, 36 falsos negativos e 176 verdadeiros positivos** no conjunto de teste.

### Segmentação dos clientes

O K-Means identificou **5 segmentos**:

| Cluster | Clientes | Taxa de churn |
|---:|---:|---:|
| 0 | 544 | 45,04% |
| 1 | 936 | 2,24% |
| 2 | 646 | 24,61% |
| 3 | 1.107 | 52,66% |
| 4 | 767 | 6,91% |

Os resultados mostram diferenças relevantes nas taxas de cancelamento entre os segmentos. Os clusters devem ser interpretados em conjunto com seus perfis médios e características comportamentais observadas na análise exploratória.

---

## 💡 Insights e Diretrizes de Retenção

A análise identificou padrões relacionados à frequência de utilização, duração dos contratos, participação em atividades e origem dos clientes.

### 1. Estimular a participação em atividades coletivas

Membros que frequentam aulas em grupo apresentam maior retenção na análise.

**Ação proposta:** desenvolver um programa de onboarding com sessões de aulas coletivas gratuitas nas primeiras semanas de matrícula, favorecendo o contato social com outros alunos e instrutores.

### 2. Incentivar contratos de maior duração

Contratos mensais concentram maior ocorrência de cancelamentos na análise.

**Ação proposta:** oferecer incentivos para renovações antecipadas e migração para planos de maior duração.

### 3. Monitorar clientes com baixa frequência

A frequência de visitas no mês atual aparece como um importante indicador associado ao churn.

**Ação proposta:** criar alertas para clientes com redução de frequência e realizar contatos personalizados, oferecendo avaliação física ou participação em aulas experimentais.

### 4. Ampliar indicações e parcerias

Clientes provenientes de indicações e convênios corporativos apresentam padrões de maior engajamento na análise.

**Ação proposta:** estruturar um programa de indicação com recompensas para clientes que tragam novos membros ativos.

---

## 📁 Estrutura do Repositório

```text
model_fitness_academias/
│
├── datasets/
│   └── gym_churn_us.csv
│
├── notebooks/
│   └── notebook.ipynb
│
└── requirements.txt
```

### Principais diretórios e arquivos

- `datasets/` — armazena a base de dados utilizada na análise.
- `notebooks/` — contém o Jupyter Notebook com toda a exploração, modelagem e segmentação.
- `requirements.txt` — reúne as dependências necessárias para executar o projeto.

---

## 🛠️ Stack Tecnológica

| Tecnologia | Utilização |
|---|---|
| **Python** | Linguagem principal |
| **Pandas** | Manipulação e análise dos dados |
| **NumPy** | Operações numéricas |
| **Matplotlib** | Visualização dos dados |
| **Scikit-learn** | Pré-processamento, classificação e clustering |
| **SciPy** | Agrupamento hierárquico e dendrograma |
| **Jupyter Notebook** | Desenvolvimento e documentação da análise |

---

## 🚀 Instalação e Execução

### 1. Clone o repositório

```bash
git clone https://github.com/alexpereira951/model_fitness_academias
cd model_fitness_academias
```

### 2. Crie e ative um ambiente virtual

```bash
python -m venv .venv
```

No Windows:

```bash
.venv\Scripts\activate
```

No Linux/macOS:

```bash
source .venv/bin/activate
```

### 3. Instale as dependências

```bash
pip install -r requirements.txt
```

### 4. Execute o Jupyter Notebook

```bash
jupyter notebook
```

Depois, abra:

```text
notebooks/notebook.ipynb
```

> O notebook procura automaticamente o arquivo `gym_churn_us.csv` no diretório `datasets/` quando executado a partir da estrutura apresentada.

---

## ⚠️ Limitações

O projeto apresenta algumas limitações que devem ser consideradas na interpretação dos resultados:

1. **Validação dos modelos:** a avaliação foi realizada a partir de uma única divisão treino/teste de 80/20, sem validação cruzada ou busca sistemática de hiperparâmetros.

2. **Variáveis disponíveis:** as conclusões estão restritas às características presentes na base `gym_churn_us.csv`. Fatores externos ou comportamentais que não estejam registrados nos dados não são considerados.

3. **Segmentação:** o K-Means foi configurado com **5 clusters**. Essa escolha define a estrutura final dos segmentos e pode produzir agrupamentos diferentes caso outro número de clusters ou outra configuração seja utilizada.

4. **Generalização:** os resultados refletem o conjunto de dados analisado e podem não representar o comportamento de clientes de outras academias, períodos ou populações sem uma nova validação.

---

## 📌 Conclusão

O projeto demonstra como técnicas de **Análise Exploratória, Machine Learning e Clusterização** podem ser combinadas para compreender o comportamento dos clientes e apoiar estratégias de retenção.

A modelagem apresentou **93% de acurácia para a Regressão Logística** e **92% para a Random Forest** no conjunto de teste, enquanto a segmentação identificou cinco grupos com taxas de churn bastante distintas.

A combinação entre **previsão de risco** e **segmentação de clientes** fornece uma base analítica para direcionar ações de relacionamento, monitoramento de frequência e retenção de alunos.
