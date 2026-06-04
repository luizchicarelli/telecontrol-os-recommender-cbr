🤖 Agente Inteligente Telecontrol
O Agente Inteligente Telecontrol é um sistema de suporte à decisão. Usa Machine Learning para classificar relatos de clientes e Raciocínio Baseado em Casos (CBR) para recomendar soluções com base no histórico. Ele otimiza tempo e custos de manutenção, exibindo a solução mais assertiva via FastAPI e interface web.

🎯 Principais Funcionalidades
Processamento de Linguagem Natural (NLP): Traduz o problema relatado pelo cliente (texto livre) para um diagnóstico técnico padronizado utilizando TF-IDF e Support Vector Classification (LinearSVC).

Raciocínio Baseado em Casos (CBR): Busca casos históricos de manutenções similares baseando-se no produto, tipo de contrato, defeito reclamado e defeito previsto pela IA.

Motor de Regras de Negócio: Filtra e ranqueia as soluções viáveis considerando o limite de custo e a probabilidade de sucesso (taxa de incidência histórica).

API Integrada (FastAPI): Backend ágil que processa os dados em tempo real e se comunica via formato JSON.

Interface Amigável (Web UI): Frontend interativo construído com HTML, JavaScript e Bootstrap, desenhado para a operação real do atendente.

🏗️ Arquitetura e Pipeline (Os Épicos)
O desenvolvimento deste sistema foi dividido em fases estruturadas (Épicos) dentro de um ambiente Jupyter Notebook, garantindo rastreabilidade desde os dados brutos até a API em produção:

Épico 2 (Pré-processamento): Limpeza de dicionários, normalização de strings e construção do DataFrame mestre consolidando Ordens de Serviço (OS), defeitos e soluções.

Épico 3 (Classificador ML): Treinamento do modelo LinearSVC utilizando mapeamento semântico de linguagem natural para defeitos técnicos constatados.

Épico 4 (CBR): Implementação do algoritmo de similaridade (match exato ponderado) para resgatar a solução historicamente mais bem-sucedida.

Épico 5 (Regras de Negócio): Aplicação de thresholds de custo máximo (R$ 1.500) e taxa de sucesso histórico.

Épico 7 (Backend & API): Empacotamento do modelo e das regras em rotas FastAPI para consumo web.

🛠️ Tecnologias Utilizadas
Linguagem: Python e JavaScript

Machine Learning & Dados: scikit-learn, pandas, numpy

Backend: fastapi, uvicorn, pydantic

Frontend: HTML5, CSS3, Bootstrap 5

Ambiente de Desenvolvimento: Jupyter Notebook / Google Colab / VS Code

🚀 Como Executar o Projeto Localmente
Pré-requisitos
Certifique-se de ter o Python 3.10+ instalado e clone este repositório:

Bash
git clone https://github.com/seu-usuario/telecontrol-ai-agent.git
cd telecontrol-ai-agent
1. Instale as dependências
Abra o seu terminal e instale as bibliotecas necessárias:

Bash
pip install pandas numpy scikit-learn fastapi uvicorn pydantic nest-asyncio
2. Prepare o Modelo de IA
Abra o arquivo FPA_3_termo_V1_9_9.ipynb no Jupyter Notebook ou VS Code.

Execute as células referentes aos Épicos 2, 3, 4 e 5.

Isso carregará os dados CSV, treinará o modelo de linguagem natural e criará as funções de similaridade na memória.

3. Inicie o Servidor da API
Ainda no notebook, execute a célula correspondente ao Épico 7 (FastAPI).

Você verá no terminal a mensagem: ✅ SERVIDOR API INICIADO NA PORTA 8000!.

O servidor ficará ativo escutando as requisições.

4. Acesse o Frontend
Com a API rodando, navegue até a pasta do projeto no seu explorador de arquivos e dê um clique duplo no arquivo index.html.

O sistema abrirá no seu navegador padrão.

Insira os dados, digite um problema na linguagem do cliente e clique em Analisar com IA.

📂 Estrutura de Arquivos Recomendada
Plaintext
telecontrol-ai-agent/
│
├── data/                                 # CSVs originais exportados do sistema
├── output/                               # CSVs limpos após o Épico 2
├── FPA_3_termo_V1_9_9.ipynb              # Notebook principal (Pipeline + API)
├── index.html                            # Frontend da aplicação
└── README.md                             # Documentação do projeto
Desenvolvido como solução inovadora para otimização de suporte técnico e manutenção.
