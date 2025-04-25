# FIAP - Faculdade de Informática e Administração Paulista

<p align="center">
<a href= "https://www.fiap.com.br/"><img src="assets/logo-fiap.png" alt="FIAP - Faculdade de Informática e Admnistração Paulista" border="0" width=40% height=40%></a>
</p>

<br>

# 🌱 Projeto de Previsão de Produtividade Agrícola com Machine Learning

## Nome do projeto
Fase 6 - Enterprise Challenge - Sprint 2 - Ingredion

## Nome do grupo
Grupo 37

## 👨‍🎓 Integrantes: 
- [Ana Beatriz Duarte Domingues](https://www.linkedin.com/in/)
- [Junior Rodrigues da Silva](https://www.linkedin.com/in/jrsilva051/)
- [Carlos Emilio Castillo Estrada](https://www.linkedin.com/in/)

## 👩‍🏫 Professores:
### Tutor(a)
- [Lucas Gomes Moreira](https://www.linkedin.com/company/inova-fusca)
### Coordenador(a)
- [André Godoi Chiovato](https://www.linkedin.com/company/inova-fusca)

---

## 📁 Estrutura de pastas

Dentre os arquivos e pastas presentes na raiz do projeto, definem-se:

- <b>.github</b>: Nesta pasta ficarão os arquivos de configuração específicos do GitHub que ajudam a gerenciar e automatizar processos no repositório.

- <b>assets</b>: aqui estão os arquivos relacionados a elementos não-estruturados deste repositório, como imagens.

- <b>config</b>: Posicione aqui arquivos de configuração que são usados para definir parâmetros e ajustes do projeto.

- <b>document</b>: aqui estão todos os documentos do projeto que as atividades poderão pedir. Na subpasta "other", adicione documentos complementares e menos importantes.

- <b>scripts</b>: Posicione aqui scripts auxiliares para tarefas específicas do seu projeto. Exemplo: deploy, migrações de banco de dados, backups.

- <b>src</b>: Todo o código fonte criado para o desenvolvimento do projeto ao longo das 7 fases.

- <b>README.md</b>: arquivo que serve como guia e explicação geral sobre o projeto (o mesmo que você está lendo agora).


## 📜 Descrição

Este projeto tem como objetivo desenvolver modelos de Inteligência Artificial capazes de prever a produtividade agrícola (soja e milho) no Brasil, utilizando dados históricos de safras e análise exploratória. Faz parte da Sprint 2 o tratamento de dados, testes com diferentes algoritmos de machine learning e avaliação de desempenho para apoiar a tomada de decisão no agronegócio.

## 🔍 1. Preparação dos Dados

Os dados utilizados contemplam indicadores de **área plantada**, **produtividade por hectare**, **produção total** e suas respectivas variações entre dois períodos: 2023–2024 e 2024–2025. As colunas principais são:

| Coluna | Descrição |
|--------|-----------|
| `REGIAO` | Região do Brasil |
| `UF` | Estado |
| `CULTURA` | Soja ou Milho |
| `AREA_PERIODO_23_24` | Área plantada em mil ha (2023–2024) |
| `AREA_PERIODO_24_25` | Área plantada em mil ha (2024–2025) |
| `AREA_VAR` | Variação percentual da área plantada |
| `PRODU_SAFRA_23_24` | Produtividade em kg/ha (2023–2024) |
| `PRODU_SAFRA_24_25` | Produtividade em kg/ha (2024–2025) |
| `PRODU_VAR` | Variação percentual da produtividade |
| `PRODUC_SAFRA_23_24` | Produção total em mil toneladas |
| `PRODUC_SAFRA_24_25` | Produção total em mil toneladas |
| `PRODUC_VAR` | Variação percentual da produção |

O processo de preparação dos dados incluiu:
- Padronização de colunas e tipos de dados
- Conversão de categorias para variáveis numéricas (one-hot encoding)
- Normalização de variáveis contínuas
- Separação por cultura e período

---

## 📊 2. Análise Exploratória e Justificativa das Variáveis

### Soja:
A análise visual sugere um aumento na produtividade da soja em 2024–2025, com mais áreas na faixa de 3500–4000 kg/ha.

![Distribuição da produtividade da soja](imagens/exploratoria/hist_soja.png)
> *Figura 1 – Distribuição da produtividade da soja nos períodos 2023–2024 e 2024–2025.*

Apesar do aumento geral, houve persistência de regiões com baixa produtividade (< 500 kg/ha), indicando possíveis fatores limitantes como solo ou clima.

### Milho:
No milho, observou-se maior dispersão nos dados de 2024–2025, com presença de produtividades tanto mais baixas quanto mais altas.

![Distribuição da produtividade do milho](imagens/exploratoria/hist_milho.png)
> *Figura 2 – Distribuição da produtividade do milho nos períodos 2023–2024 e 2024–2025.*

A faixa principal de 4500–6000 kg/ha permaneceu, mas surgiram extremos que indicam variabilidade climática ou de manejo.


✅ **Variáveis escolhidas para modelagem**:
- `REGIAO`, `UF`, `CULTURA`
- `AREA_VAR`, `PRODU_VAR`, `PRODUC_VAR`
- Valores absolutos das safras 23/24 como base para previsão da 24/25

Essas variáveis foram escolhidas por apresentarem **forte correlação com os resultados de produção futura**, além de fornecerem uma representação completa do cenário agrícola em diferentes regiões.

---
Testamos três abordagens:

| Modelo              | Vantagens                                    | Desvantagens                               |
|---------------------|----------------------------------------------|--------------------------------------------|
| Regressão Linear    | Simples e interpretável                      | Sujeita a underfitting                      |
| Random Forest       | Boa generalização, robusto contra overfitting| Requer mais processamento                   |
| Gradient Boosting   | Alta precisão em treino e teste              | Risco de overfitting                        |

### 🔧 Ajuste de Hiperparâmetros
- Random Forest: `n_estimators=100`, `max_depth=None`
- Técnicas utilizadas: `GridSearchCV` e `RandomizedSearchCV`.

---

## 📈 5. Avaliação e Resultados

### Resultados – Soja

| Modelo            | R²     | MSE        |
|-------------------|--------|------------|
| Regressão Linear  | 1.00   | 250.70     |
| Random Forest     | 0.95   | 126533.84  |
| Gradient Boosting | 1.00   | 1403.43    |

![Produção real vs predita – Soja](imagens/exploratoria/soja_regressao_linear.png)
> *Figura 3 – Produção real vs prevista para soja (modelo de Regressão Linear).*

---

### Resultados – Milho

| Modelo            | R²     | MSE        |
|-------------------|--------|------------|
| Regressão Linear  | 0.88   | 408753.28  |
| Random Forest     | 0.91   | 312554.16  |
| Gradient Boosting | 0.92   | 275282.05  |

![Produção real vs predita – Milho](imagens/exploratoria/milho_gradient_boosting.png)
> *Figura 4 – Produção real vs prevista para milho (modelo de Gradient Boosting).*

---

### Resultados – Brasil (Todos os Dados)

| Modelo            | R²     | MSE        |
|-------------------|--------|------------|
| Regressão Linear  | 0.94   | 215477.66  |
| Random Forest     | 0.97   | 110145.04  |
| Gradient Boosting | 0.97   | 103784.57  |

![Produção real vs predita – Brasil](imagens/exploratoria/brasil_random_forest.png)
> *Figura 5 – Produção real vs prevista para o Brasil (modelo de Random Forest).*

---

## 📷 6. Prints e Gráficos

Todas as imagens utilizadas neste README estão armazenadas em `imagens/exploratoria/`.

---

## ▶️ 7. Demonstração em Vídeo

🎥 [Clique aqui para assistir à demonstração no YouTube](LINK_DO_VIDEO_AQUI)

---

## ⚙️ 8. Como Executar o Projeto

```bash
# Instalar dependências
pip install -r requirements.txt

# Executar o pré-processamento
python scripts/pre_processamento.py

# Treinar o modelo
python scripts/treina_modelo.py


## 📚 Histórico de Lançamentos

* 0.1.0 - 13/03/2025
    * Primeira versão do projeto com análise exploratória, clusterização e modelos de regressão.

---

## 📋 Licença

<img style="height:22px!important;margin-left:3px;vertical-align:text-bottom;" src="https://mirrors.creativecommons.org/presskit/icons/cc.svg?ref=chooser-v1"><img style="height:22px!important;margin-left:3px;vertical-align:text-bottom;" src="https://mirrors.creativecommons.org/presskit/icons/by.svg?ref=chooser-v1"><p xmlns:cc="http://creativecommons.org/ns#" xmlns:dct="http://purl.org/dc/terms/"><a property="dct:title" rel="cc:attributionURL" href="https://github.com/agodoi/template">MODELO GIT FIAP</a> por <a rel="cc:attributionURL dct:creator" property="cc:attributionName" href="https://fiap.com.br">Fiap</a> está licenciado sobre <a href="http://creativecommons.org/licenses/by/4.0/?ref=chooser-v1" target="_blank" rel="license noopener noreferrer" style="display:inline-block;">Attribution 4.0 International</a>.</p>
