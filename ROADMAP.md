# NLP Meeting Intelligence — TOTVS × BERTimbau

> Sistema de Inteligência de Código para análise automática de transcrições de reuniões, identificação de riscos, oportunidades comerciais e geração de insights utilizando BERTimbau + RAG.

---

# Visão Geral

## Objetivo

Construir um MVP capaz de receber uma transcrição de reunião e retornar automaticamente:

- Risco de churn
- Feedback positivo
- Feedback negativo
- Oportunidades de upsell
- Aceite comercial
- Objeções comerciais
- Solicitações de suporte
- Casos de escalation

Exemplo:

```json
{
  "churn_risk": 0.84,
  "feedback_negativo": 0.91,
  "upsell_oportunidade": 0.27
}
```

---

# Arquitetura Final

```text
Transcrição
      ↓
Pré-processamento
      ↓
Segmentação
      ↓
Classificação Multi-label (BERTimbau)
      ↓
RAG (FAISS + Documentação TOTVS)
      ↓
Insights Contextualizados
      ↓
API REST (FastAPI)
```

---

# Stack Tecnológica

## Linguagem

- Python 3.11+

## Manipulação de Dados

- Pandas
- Numpy

## NLP

- Transformers (Hugging Face)
- Datasets
- Evaluate

## Deep Learning

- PyTorch

## API

- FastAPI
- Uvicorn

## RAG

- LangChain
- FAISS
- Sentence Transformers

## Versionamento

- Git
- GitHub

---

# Roadmap de Estudos

## Fase 1 — Fundamentos de Python

### Objetivo

Aprender programação suficiente para manipular os dados.

### Tópicos

- Variáveis
- Funções
- Listas
- Dicionários
- Loops
- Condicionais
- Leitura de arquivos

### Meta

Conseguir executar:

```python
import json
import pandas as pd

df = pd.read_json("transcricao.json")
print(df.head())
```

### Tempo Estimado

2 semanas

---

## Fase 2 — Engenharia de Dados

### Objetivo

Aprender a limpar e preparar transcrições.

### Tópicos

- Pandas
- Regex
- Manipulação de JSON
- Limpeza de texto
- Deduplicação
- Exportação CSV

### Meta

Transformar:

```text
[LOCUTOR 3]: Boa tarde
[LOCUTOR 2]: Tudo bem?
```

em:

```json
{
  "speaker": "LOCUTOR_3",
  "text": "Boa tarde"
}
```

### Tempo Estimado

2 semanas

---

## Fase 3 — NLP Básico

### Objetivo

Entender como texto vira vetor.

### Tópicos

- Tokenização
- Embeddings
- Transformers
- BERT
- BERTimbau

### Meta

Carregar o BERTimbau localmente.

```python
from transformers import AutoTokenizer

tokenizer = AutoTokenizer.from_pretrained(
    "neuralmind/bert-base-portuguese-cased"
)
```

### Tempo Estimado

1 semana

---

## Fase 4 — Fine-Tuning

### Objetivo

Treinar o modelo para classificação multi-label.

### Tópicos

- Hugging Face Trainer
- BCEWithLogitsLoss
- Multi-label Classification
- Train / Validation Split

### Labels

- churn_risk
- feedback_positivo
- feedback_negativo
- aceite_comercial
- objecao_comercial
- solicitacao_suporte
- upsell_oportunidade
- escalation_risco

### Meta

Treinar o primeiro classificador.

### Tempo Estimado

2 semanas

---

## Fase 5 — FastAPI

### Objetivo

Transformar o modelo em serviço.

### Endpoint

```http
POST /analisar
```

### Entrada

```json
{
  "texto": "Estamos avaliando trocar de fornecedor."
}
```

### Saída

```json
{
  "churn_risk": 0.91
}
```

### Tempo Estimado

1 semana

---

## Fase 6 — RAG

### Objetivo

Relacionar sinais detectados com soluções TOTVS.

### Base de Conhecimento

- RM
- Meu RH
- ClockIn
- ERP
- Fiscal
- RH
- Documentação TOTVS

### Embeddings

```text
sentence-transformers/paraphrase-multilingual-mpnet-base-v2
```

### Vector Store

```text
FAISS
```

### Tempo Estimado

1 semana

---

## Fase 7 — Dashboard

### Objetivo

Visualizar indicadores.

### Métricas

- Clientes com maior risco
- Clientes com maior potencial de upsell
- Volume de reclamações
- Distribuição de labels
- Histórico de reuniões

### Ferramentas

- Streamlit
- Power BI

### Tempo Estimado

1 semana

---

# Roadmap Técnico do Projeto

## Sprint 1 — Exploração dos Dados

### Entregas

- [ ] Ler arquivos JSON
- [ ] Mapear estrutura
- [ ] Identificar locutores
- [ ] Gerar estatísticas básicas

### Resultado

Dataset compreendido.

---

## Sprint 2 — Pré-processamento

### Entregas

- [ ] Remover ruído
- [ ] Normalizar texto
- [ ] Unificar falas consecutivas
- [ ] Exportar dataset limpo

### Resultado

Dataset utilizável.

---

## Sprint 3 — Segmentação

### Entregas

- [ ] Implementar chunks
- [ ] Definir overlap
- [ ] Gerar segmentos

### Configuração Inicial

```python
max_length = 256
overlap = 50
```

### Resultado

Reuniões transformadas em exemplos de treino.

---

## Sprint 4 — Labeling

### Entregas

- [ ] Prompt para LLM
- [ ] Rotulador Temperatura 0
- [ ] Rotulador Temperatura 0.7
- [ ] Detecção de desacordo

### Resultado

Dataset rotulado.

---

## Sprint 5 — Treinamento

### Entregas

- [ ] Dataset final
- [ ] BERTimbau treinado
- [ ] Avaliação

### Configuração

```python
per_device_train_batch_size = 4

gradient_accumulation_steps = 8

fp16 = True

max_length = 256
```

### Resultado

Modelo funcional.

---

## Sprint 6 — Avaliação Real

### Entregas

- [ ] Selecionar 50–100 reuniões reais
- [ ] Comparar previsões
- [ ] Medir F1 por classe

### Métricas

- Precision
- Recall
- F1
- Macro F1

### Resultado

Validação do MVP.

---

## Sprint 7 — API

### Entregas

- [ ] Endpoint REST
- [ ] Upload de texto
- [ ] Retorno de probabilidades

### Resultado

API operacional.

---

## Sprint 8 — RAG

### Entregas

- [ ] Processar PDFs
- [ ] Criar embeddings
- [ ] Criar índice FAISS
- [ ] Buscar documentação relevante

### Resultado

Insights contextualizados.

---

## Sprint 9 — MVP Final

### Entrada

```text
Transcrição completa
```

### Saída

```json
{
  "meeting_score": {
    "risco": 82,
    "oportunidade": 67
  },
  "labels": {
    "feedback_negativo": 0.91,
    "objecao_comercial": 0.87
  },
  "insight": "Cliente demonstrou preocupação com workflows de aprovação. Existe aderência ao módulo Meu RH."
}
```

---

# Estrutura do Repositório

```text
totvs-meeting-intelligence/
│
├── data/
│   ├── raw/
│   ├── processed/
│   └── labeled/
│
├── notebooks/
│
├── src/
│   ├── preprocessing/
│   ├── segmentation/
│   ├── labeling/
│   ├── training/
│   ├── inference/
│   ├── rag/
│   └── api/
│
├── models/
│
├── tests/
│
├── requirements.txt
│
├── README.md
│
└── roadmap.md
```

---

# Critérios de Sucesso do MVP

## Técnicos

- [ ] Fine-tuning executando localmente
- [ ] Inferência via API
- [ ] Tempo de resposta < 5 segundos
- [ ] Sem estouro de VRAM na RTX 3050

## Modelo

- [ ] Macro F1 ≥ 0.75
- [ ] Recall ≥ 0.80 para churn_risk
- [ ] Recall ≥ 0.80 para escalation_risco

## Negócio

- [ ] Detectar oportunidades comerciais
- [ ] Identificar riscos de churn
- [ ] Apoiar equipes de CS e Vendas
- [ ] Gerar insights contextualizados via RAG

---

# Visão de Longo Prazo

## V2

- Detecção automática de papel do locutor
- Classificação por participante
- Resumo executivo automático
- Detecção de objeções em tempo real

## V3

- Dashboard corporativo
- Monitoramento contínuo de contas
- Alertas automáticos para Customer Success
- Integração com CRM TOTVS

---

Autor: Gabriel Augusto Gonçalves Pereira
Projeto: NLP Meeting Intelligence — TOTVS × BERTimbau
Status: Em Desenvolvimento
