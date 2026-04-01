# dataflow-langchain
Pipeline multi-agente autônomo com LangChain e padrão ReAct — 5 agentes especializados (Ingestão, Validação, Transformação, Análise e Relatório) com ferramentas customizadas BaseTool e memória conversacional.
# 🦜 DataFlow AI — LangChain Multi-Agent Pipeline

![LangChain](https://img.shields.io/badge/LangChain-0.3-1C3C3C?style=for-the-badge&logo=langchain&logoColor=white)
![Python](https://img.shields.io/badge/Python-3.11-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-2.0-150458?style=for-the-badge&logo=pandas)
![Status](https://img.shields.io/badge/Status-Concluído-brightgreen?style=for-the-badge)

> Sistema multi-agente autônomo construído com **LangChain**, usando o padrão **ReAct** (Reason + Act), ferramentas customizadas (**BaseTool**) e memória conversacional (**ConversationBufferMemory**) — do dado bruto ao relatório, sem intervenção humana.

---

## 🧠 Arquitetura LangChain

```
                    ┌──────────────────────────────┐
                    │     LangChainOrchestrator     │
                    │   (coordena AgentExecutors)   │
                    └────────────┬─────────────────┘
                                 │
          ┌──────────────────────┼──────────────────────┐
          │                      │                      │
  ┌───────▼──────┐      ┌────────▼──────┐      ┌───────▼──────┐
  │IngestionAgent│      │ValidationAgent│      │ Transforma-  │
  │              │      │               │      │ tionAgent    │
  │ ReAct Loop   │      │ ReAct Loop    │      │ ReAct Loop   │
  │ + Memory     │      │ + Memory      │      │ + Memory     │
  │              │      │               │      │              │
  │ BaseTool:    │      │ BaseTool:     │      │ BaseTool:    │
  │ data_        │      │ data_         │      │ data_        │
  │ ingestion    │      │ validation    │      │ transformation│
  └──────────────┘      └───────────────┘      └──────────────┘
          └──────────────────────┬──────────────────────┘
                        ┌────────▼──────┐
                        │ AnalysisAgent │
                        │               │
                        │ ReAct Loop    │
                        │ + Memory      │
                        │               │
                        │ BaseTool:     │
                        │ data_analysis │
                        └───────┬───────┘
                        ┌───────▼───────┐
                        │  ReportAgent  │
                        │               │
                        │ ReAct Loop    │
                        │ + Memory      │
                        │               │
                        │ BaseTool:     │
                        │report_genera- │
                        │     tor       │
                        └───────────────┘
```

---

## ⚡ Execução Real do Pipeline

```
╔══════════════════════════════════════════════════════════╗
║       DataFlow AI — LangChain Multi-Agent Pipeline       ║
║  🦜 LangChain | 🤖 ReAct Agents | 🔧 Custom Tools       ║
╚══════════════════════════════════════════════════════════╝

⚙️  Modo demo (MockLLM) — conecte OPENAI_API_KEY para LLM real

[AgentFactory] ✅ IngestionAgent      | ferramentas: ['data_ingestion']
[AgentFactory] ✅ ValidationAgent     | ferramentas: ['data_validation']
[AgentFactory] ✅ TransformationAgent | ferramentas: ['data_transformation']
[AgentFactory] ✅ AnalysisAgent       | ferramentas: ['data_analysis']
[AgentFactory] ✅ ReportAgent         | ferramentas: ['report_generator']

🚀 Pipeline LangChain iniciado | run_id=run_langchain_001
🦜 Framework: LangChain | Padrão: ReAct

▶ IngestionAgent    → ✅ 100,000 registros ingeridos          (0.09s)
▶ ValidationAgent   → ✅ Score: 90.0% | 9/10 regras           (0.05s)
▶ TransformationAgent → ✅ 17 features criadas                (0.05s)
▶ AnalysisAgent     → ✅ 7 insights gerados | 0 anomalias     (0.05s)
▶ ReportAgent       → ✅ Relatório HTML salvo                  (0.00s)

📊 RESUMO: 5/5 agentes | 0 falhas | 0.24s total

📊 KPIs FINAIS:
   Corridas processadas : 96,103
   Receita total        : $1,735,316.57
   Ticket médio         : $18.06
   Distância média      : 5.45 km
   % com gorjeta        : 75.9%
   Cartão de crédito    : 80.0%

💡 INSIGHTS:
   → Horário de pico: 16h com 4,048 corridas
   → Melhor hora de receita: 12h — $74,052.30
   → Melhor horário para gorjetas: 7h (média $2.11)
   → Cartão de crédito: 80.03% das corridas
```

---

## 🔧 Componentes LangChain Utilizados

| Componente | Uso no Projeto |
|---|---|
| `BaseTool` | 5 ferramentas customizadas de dados |
| `AgentExecutor` | Execução de cada agente especializado |
| `create_react_agent` | Padrão ReAct (Reason + Act) |
| `PromptTemplate` | Prompts especializados por agente |
| `ConversationBufferMemory` | Memória conversacional por agente |
| `BaseLLM` | Interface para OpenAI / Anthropic / Mock |

---

## 🤖 Agentes e Ferramentas

### IngestionAgent → `DataIngestionTool`
- Suporta CSV, Parquet, JSON, XLSX
- Detecção automática de schema (dtype, nulos, min/max)
- Logging de metadados e memória utilizada

### ValidationAgent → `DataValidationTool`
- 10 regras de qualidade (completude, duplicatas, ranges, consistência, outliers)
- Score de qualidade calculado por execução
- Auto-fix: remove outliers, imputa nulos com mediana

### TransformationAgent → `DataTransformationTool`
- 17 features criadas automaticamente
- Features temporais: hora, turno, fim de semana
- Métricas: velocidade média, receita/km, taxa de gorjeta
- Flags: aeroporto, horário de pico, madrugada

### AnalysisAgent → `DataAnalysisTool`
- 11 KPIs operacionais calculados
- 7 insights gerados automaticamente
- Detecção de anomalias estatísticas

### ReportAgent → `ReportGeneratorTool`
- Relatório HTML gerado automaticamente
- Dashboard visual com KPIs, qualidade e insights
- Nomeado por run_id para rastreabilidade

---

## 📁 Estrutura do Projeto

```
dataflow-langchain/
├── main.py                        # Ponto de entrada
├── requirements.txt
├── core/
│   └── orchestrator.py            # LangChainOrchestrator
├── agents/
│   ├── __init__.py
│   └── agent_factory.py           # build_agent() + prompts ReAct
├── tools/
│   ├── __init__.py
│   └── data_tools.py              # 5 BaseTool customizadas
└── data/
    ├── raw/                       # Dados brutos
    └── reports/                   # Relatórios HTML gerados
```

---

## 🚀 Como Executar

```bash
# Clone o repositório
git clone https://github.com/Engcarlosrodrigo-dev/dataflow-langchain.git
cd dataflow-langchain

# Instale as dependências
pip install -r requirements.txt

# Execute sem LLM (modo demo)
python main.py

# Execute com OpenAI (modo produção)
export OPENAI_API_KEY="sk-..."
python main.py

# Execute com Anthropic Claude
export ANTHROPIC_API_KEY="sk-ant-..."
python main.py
```

---

## 🛠️ Stack Técnica

| Tecnologia | Versão | Uso |
|---|---|---|
| LangChain | 0.3.x | Framework principal de agentes |
| langchain-core | 0.3.x | BaseTool, PromptTemplate, BaseLLM |
| Python | 3.11 | Linguagem principal |
| Pandas | 2.0+ | Manipulação de dados |
| NumPy | 1.24+ | Cálculos e detecção de outliers |
| OpenAI / Anthropic | - | LLM opcional (substitui MockLLM) |

---

## 📐 Padrões de Engenharia

- **ReAct Pattern** — cada agente raciocina antes de agir (Reason + Act)
- **BaseTool** — ferramentas com schema Pydantic validado automaticamente
- **ConversationBufferMemory** — cada agente mantém histórico da sua execução
- **AgentExecutor** — controla iterações, erros e intermediate steps
- **MockLLM** — permite execução e teste sem custo de API
- **Fault Tolerance** — agentes críticos interrompem o pipeline; demais continuam

---

## 🔭 Roadmap

- [ ] Substituir MockLLM por GPT-4o com function calling real
- [ ] Adicionar LangGraph para workflows condicionais entre agentes
- [ ] Implementar VectorStore (FAISS/Chroma) para memória de longo prazo
- [ ] Criar MLAgent com predição de demanda via scikit-learn
- [ ] Integrar LangSmith para observabilidade e tracing
- [ ] Containerizar com Docker + agendamento via Airflow

---

*Projeto desenvolvido para demonstração de habilidades em Engenharia de Dados com IA, LangChain, Agentes Autônomos e boas práticas de Python.*
