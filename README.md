# 🤖 Agente Inteligente Telecontrol — FPA 3º Termo

Sistema de recomendação de diagnóstico e solução para ordens de serviço (OS) de equipamentos de refrigeração, combinando **Machine Learning (TF-IDF + LinearSVC)** e **Raciocínio Baseado em Casos (CBR)**.

---

## 📋 Visão Geral

O projeto foi desenvolvido como trabalho de 3º termo e implementa um pipeline completo de dados, desde o pré-processamento de bases históricas de OS até uma API com interface web para uso em campo. O agente recebe a descrição do problema relatado pelo cliente e retorna:

- **Diagnóstico técnico previsto** (classificador ML)
- **Solução recomendada** com taxa de sucesso histórico
- **Custo estimado** da intervenção
- **Casos similares** recuperados da base histórica (CBR)

---

## 🗂️ Estrutura do Projeto

```
FPA_3termo/
├── data/                              # Dados brutos exportados do sistema
│   ├── export_os_base.csv             # Base principal de ordens de serviço
│   ├── export_os_defeito_solucao.csv  # Defeitos e soluções por OS
│   ├── export_defeitos_constatados.csv
│   ├── export_defeitos_reclamados.csv
│   ├── export_diagnosticos.csv
│   ├── export_pecas_por_os.csv
│   ├── export_os_sem_pecas.csv
│   ├── export_produtos.csv
│   ├── export_resumo_produto.csv
│   ├── export_solucoes.csv
│   └── export_tipos_atendimento.csv
│
├── output/                            # Dados processados (gerados pelo notebook)
│   ├── df_mestre.csv                  # DataFrame mestre consolidado
│   ├── defeitos_constatados_clean.csv
│   ├── defeitos_reclamados_clean.csv
│   ├── diagnosticos_clean.csv
│   └── solucoes_clean.csv
│
├── FPA_3_termo_V2_+_frontend.ipynb    # Notebook principal (pipeline + API)
└── index.html                         # Frontend web da aplicação
```

---

## ⚙️ Épicos Implementados

### Épico 2 — Pré-processamento (US04)
Construção do **DataFrame Mestre** unificando todas as fontes de dados:
- Limpeza de textos com HTML tags e caracteres especiais
- Normalização de strings (lowercase, remoção de acentos)
- Cap de outliers no campo `tempo_resolucao_horas` (percentil 95)
- Filtragem de OS concluídas e válidas
- Enriquecimento da tabela de diagnósticos com descrições textuais

### Épico 3 — Classificador ML V1.9 (US05–US08)
Classificador que recebe **linguagem natural do cliente** e retorna o diagnóstico técnico:
- Corpus de treino construído via mapeamento manual de 212 defeitos reclamados → 21 classes técnicas
- Pipeline: `TfidfVectorizer (1–3 ngrams)` + `CalibratedClassifierCV(LinearSVC)`
- Avaliação com validação cruzada estratificada (5-fold)
- Cobre categorias: refrigeração, iluminação, estado físico, porta/gaxeta, elétrica, ventilação, sensores, vazamentos, etc.

### Épico 4 — CBR (Case-Based Reasoning)
Recuperação de casos históricos semelhantes ao problema atual:
- Similaridade por match ponderado em variáveis categóricas (produto, tipo de atendimento, defeito reclamado, defeito constatado previsto)
- Pesos configuráveis por dimensão
- Taxa de sucesso calculada pela frequência relativa histórica real

### Épico 5 — Regras de Negócio
Filtros de viabilidade sobre as soluções recomendadas:
- `taxa_min`: frequência mínima histórica (padrão: 10%)
- `custo_max`: custo máximo aceitável (padrão: R$ 1.000)

### Épico 7 — API FastAPI + Frontend
Servidor web executável no **Google Colab**:
- Endpoint `POST /analisar-os` recebe os dados da OS e retorna diagnóstico + recomendações
- Frontend Bootstrap servido pelo próprio endpoint `GET /`
- CORS habilitado para integração flexível

---

## 🚀 Como Executar

### Pré-requisitos

```bash
pip install pandas numpy scikit-learn fastapi uvicorn pydantic
```

> O projeto foi desenvolvido para rodar no **Google Colab**. A célula do Épico 7 usa `google.colab.output.serve_kernel_port_as_window` para expor o servidor.

### Passo a passo

1. Faça upload dos arquivos da pasta `data/` para o ambiente Colab (ou ajuste `DATA_DIR` para o caminho correto).
2. Abra o notebook `FPA_3_termo_V2_+_frontend.ipynb` no Google Colab.
3. Execute as células em ordem:
   - **Épico 2** → gera os arquivos em `output/`
   - **Épico 3** → treina o classificador (objeto `clf` fica em memória)
   - **Épico 4 e 5** → carrega o DataFrame mestre e define as funções de CBR e regras
   - **Épico 7** → sobe o servidor FastAPI; um link para a interface web será exibido
4. Clique no link gerado para abrir o **Agente Inteligente** no navegador.

---

## 🖥️ Interface Web

A tela principal permite:

| Campo | Descrição |
|---|---|
| **ID do Produto** | Identificador numérico do equipamento |
| **Tipo de Atendimento** | Garantia, instalação, fora de garantia etc. |
| **ID do Defeito Reclamado** | Código do defeito (opcional, editável) |
| **Descrição do Problema** | Texto livre com a queixa do cliente |

Após clicar em **"Analisar com IA"**, o sistema exibe:
- Diagnóstico técnico pelo modelo ML
- Solução recomendada com taxa de sucesso e custo estimado
- Tabela com os casos históricos mais similares e seus scores

---

## 📊 Dados

A base histórica utilizada contém **~552 mil ordens de serviço** da Telecontrol, cobrindo equipamentos de refrigeração (freezers, geladeiras, bebedouros, etc.). Os dados são anonimizados (`os_id_anonimo`).

> ⚠️ Os arquivos CSV na pasta `data/` não estão incluídos neste repositório por conterem dados sensíveis. Solicite acesso à equipe responsável.

---

## 🛠️ Tecnologias

- **Python 3** — pandas, numpy, scikit-learn
- **FastAPI** + **Uvicorn** — API REST
- **Bootstrap 5** — Interface web
- **Google Colab** — Ambiente de execução

---

## 👥 Autores

Projeto desenvolvido como trabalho de conclusão do 3º termo — Análise e Desenvolvimento de Sistemas.
