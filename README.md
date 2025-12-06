A seguir está um **README profissional**, completo, organizado, sem expressões associadas à autoria por IA, sem emojis, com tabelas e com foco em clareza, reprodutibilidade e credibilidade técnica.
Todo o conteúdo foi construído com base direta no documento enviado .

---

# Unsupervised Learning on Country Data

### Clusterização, Validação e Similaridade em Séries Temporais

Este repositório apresenta um projeto completo de análise e clusterização utilizando dados socioeconômicos de países, além do desenvolvimento de uma medida personalizada de similaridade para séries temporais. O trabalho inclui todas as etapas solicitadas em um ambiente acadêmico profissional: infraestrutura, pré-processamento, validação de modelos, comparação de algoritmos e desenvolvimento de metodologias avançadas.

---

## 1. Estrutura do Projeto

| Diretório / Arquivo    | Descrição                                                                    |
| ---------------------- | ---------------------------------------------------------------------------- |
| `analise.ipynb`        | Notebook principal contendo toda a análise, gráficos e execução dos modelos. |
| `requirements.txt`     | Lista completa das bibliotecas utilizadas no ambiente virtual.               |
| `/img/`                | Capturas de tela comprovando ambiente local, versões, execução e gráficos.   |
| `README.md`            | Documento que descreve o projeto e orienta a reprodução.                     |
| Código-fonte adicional | Incluído no notebook e descrito neste README.                                |

---

## 2. Infraestrutura Utilizada

O projeto foi desenvolvido seguindo boas práticas de isolamento de ambiente e versionamento.

| Item             | Status                              |
| ---------------- | ----------------------------------- |
| Versão do Python | Python 3.9+                         |
| Ambiente virtual | Conda (Anaconda)                    |
| IDE utilizada    | Visual Studio Code                  |
| Execução local   | Notebook Jupyter rodando localmente |
| Evidências       | Capturas incluídas no repositório   |

Todas as bibliotecas foram instaladas no ambiente virtual e exportadas via:

```
pip freeze > requirements.txt
```

Repositório público:
[https://github.com/BrunoBersan/unsupervised_learning_validation](https://github.com/BrunoBersan/unsupervised_learning_validation)

---

## 3. Base de Dados

A base utilizada contém indicadores socioeconômicos e de saúde de países, disponibilizada no Kaggle:

[https://www.kaggle.com/datasets/rohan0301/unsupervised-learning-on-country-data](https://www.kaggle.com/datasets/rohan0301/unsupervised-learning-on-country-data)

| Característica       | Descrição                                                                                                                                                                                                                                                                                                |
| -------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Número de países     | 167                                                                                                                                                                                                                                                                                                      |
| Variáveis principais | child_mort, exports, health, imports, income, inflation, life_expec, total_fer, gdpp                                                                                                                                                                                                                     |
| Justificativa        | A base é adequada para estudos de clusterização por possuir múltiplas métricas heterogêneas que representam diferentes dimensões do desenvolvimento dos países. A diversidade dos dados, presença de outliers e escalas distintas permite avaliar de forma robusta diferentes algoritmos de agrupamento. |

---

## 4. Análise Exploratória e Pré-processamento

A análise exploratória identificou:

* Variáveis em escalas muito diferentes
* Presença significativa de outliers
* Distribuições não normalizadas
* Alta assimetria em algumas métricas econômicas

### Principais etapas executadas

| Etapa                 | Ação                                                            |
| --------------------- | --------------------------------------------------------------- |
| Limpeza               | Verificação de inconsistências e valores duplicados             |
| Normalização          | `StandardScaler` aplicado às variáveis numéricas                |
| Avaliação de outliers | Feita por meio de boxplots individuais por variável             |
| Preparação final      | Dados padronizados para entrada nos algoritmos de clusterização |

---

## 5. Modelagem de Clusterização

### Algoritmo 1: K-Means

* Busca do número ótimo de clusters entre 2 e 10.
* Avaliação pelo índice de Silhueta e método do Cotovelo.
* Resultado: **k = 5** apresentou melhor equilíbrio entre compacidade e separação.

### Algoritmo 2: DBSCAN

* Avaliação de valores de `eps` entre 1 e 3.
* Análise do gráfico k-distance com inclinação acentuada entre 2.5 e 3.0.
* Melhor separação ocorreu em **eps = 1.2**, revelando clusters densos e identificando outliers.

---

## 6. Métricas de Validação

Além da silhueta, foram utilizadas duas métricas amplamente aceitas:

| Métrica           | Interpretação                              | Resultado                |
| ----------------- | ------------------------------------------ | ------------------------ |
| Calinski–Harabasz | Razão entre dispersão inter e intracluster | Pico secundário em k = 5 |
| Davies–Bouldin    | Avalia similaridade entre clusters         | Mínimo em k = 5          |

Todas convergiram para o mesmo valor ótimo obtido pela silhueta no K-Means.

### Observação sobre silhueta para DBSCAN

A silhueta **não é apropriada** para determinar parâmetros de DBSCAN, pois:

* DBSCAN não trabalha com número de clusters pré-definido.
* Parte dos pontos é tratada como ruído.
* Formas arbitrárias de cluster prejudicam a métrica.

A escolha adequada é o **gráfico k-distance**.

---

## 7. Comparação entre K-Means e DBSCAN

| Aspecto              | K-Means                      | DBSCAN                                |
| -------------------- | ---------------------------- | ------------------------------------- |
| Número de clusters   | Pré-definido                 | Determinado pela densidade            |
| Formato dos clusters | Esféricos                    | Arbitrários                           |
| Outliers             | Não tratados                 | Rotulados como -1                     |
| Sensibilidade        | Inicialização, escala        | eps e min_samples                     |
| Melhor uso           | Dados homogêneos e escalados | Dados com ruído ou formas irregulares |

A comparação mostrou que os algoritmos são complementares:
K-Means fornece estruturação clara e DBSCAN revela padrões densos e ruído real.

---

## 8. Medidas de Similaridade em Séries Temporais

### Abordagem 1: Correlação Cruzada (projeto principal)

Passos principais:

1. Pré-processamento (estacionariedade, normalização).
2. Cálculo da CCF para todos os pares.
3. Extração do valor máximo por par.
4. Construção de matriz de similaridade e transformação em distância.
5. Clusterização hierárquica.
6. Corte do dendrograma para 3 grupos.
7. Validação visual e estatística.

### Caso de uso sugerido

Agrupamento de sensores industriais monitorando uma mesma máquina, onde atrasos temporais entre sinais são comuns.

---

## 9. Estratégia Alternativa de Similaridade

### Dynamic Time Warping (DTW)

Passos principais:

* Pré-processamento e normalização.
* Cálculo da distância DTW entre todas as séries.
* Construção da matriz de distâncias.
* Clusterização hierárquica ou k-medoids.
* Validação dos grupos e análise de warping paths.

DTW é adequado quando séries possuem padrões semelhantes em ritmos diferentes.

---

## 10. Como Reproduzir

1. Clonar o repositório

```
git clone https://github.com/BrunoBersan/unsupervised_learning_validation
```

2. Criar ambiente Conda

```
conda create -n country_cluster_env python=3.9
conda activate country_cluster_env
```

3. Instalar dependências

```
pip install -r requirements.txt
```

4. Executar o Jupyter Notebook

```
jupyter notebook analise.ipynb
```

---

## 11. Conclusões

O projeto demonstra um fluxo completo de análise não supervisionada, incluindo:

* Tratamento de dados reais
* Avaliação crítica de algoritmos
* Validação com múltiplas métricas
* Uso de métodos avançados de similaridade temporal
* Documentação reprodutível e profissional

A abordagem entrega uma visão robusta para segmentação de países e oferece bases sólidas para aplicações analíticas e estudos posteriores.

---

Se desejar, posso gerar também:

* Um PDF formatado para apresentação
* Uma versão curta do README
* Uma versão em inglês para o GitHub
