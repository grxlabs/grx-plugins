# grx-intelligence

[English](#english) · [Português](#português)

---

## English

The Gym Rank X Business Intelligence plugin. An HR manager imports spreadsheets, connects HR system APIs and webhooks, fixes sources, asks questions about the data and builds dashboards, cohorts and alerts, all in conversation, through the Gym Rank X Business MCP server.

**Install on Claude, ChatGPT, Codex and other apps:** see the [installation guide](https://github.com/grxlabs/grx-plugins#install).

### What it contains

- `.mcp.json`: the Gym Rank X Business MCP server (`https://srv01.gymrankx.com/api/business/mcp`, OAuth sign-in).
- `skills/intelligence/`: the `intelligence` skill. `SKILL.md` holds the routing and the rules that always apply; `references/` holds one short checklist per job, each linking the documentation.
- `agents/`: six specialist agents, for apps with subagents (Claude Code and Cowork):
  - `explore-file`: reads a workbook (sheets, layout, existing bases), read only.
  - `setup-spreadsheet`: maps one file and saves it as a new base.
  - `explore-api`: finds out how an API works without saving anything.
  - `setup-api`: sets up and saves one API source.
  - `label-data`: writes the data dictionary.
  - `find-insights`: tests hypotheses and proposes findings, each with a chart; never builds.

### Prerequisites

- A Gym Rank X Business account in a company with Intelligence enabled. Until Gym Rank X enables it, every Intelligence tool answers `intel_disabled`.
- The console manager role for anything that writes (imports, sources, mappings, dashboards, alerts, labels). A console viewer can only read.

### Documentation

Every procedure and rule lives in the documentation for agents: https://business.gymrankx.com/docs/intelligence/index.md (all pages: https://business.gymrankx.com/docs/intelligence/llms.txt).

---

## Português

O plugin da Inteligência do Gym Rank X Business. A pessoa de RH importa planilhas, conecta APIs e webhooks do sistema de RH, corrige fontes, faz perguntas sobre os dados e monta dashboards, coortes e alertas, tudo conversando, pelo servidor MCP do Gym Rank X Business.

**Instalar no Claude, ChatGPT, Codex e outros apps:** veja o [guia de instalação](https://github.com/grxlabs/grx-plugins#instalar).

### O que tem dentro

- `.mcp.json`: o servidor MCP do Gym Rank X Business (`https://srv01.gymrankx.com/api/business/mcp`, login por OAuth).
- `skills/intelligence/`: a skill `intelligence`. O `SKILL.md` tem o roteamento e as regras que valem sempre; `references/` tem um checklist curto por tarefa, cada um com link para a documentação.
- `agents/`: seis agentes especialistas, para apps com subagentes (Claude Code e Cowork):
  - `explore-file`: lê uma planilha (abas, formato, bases que já existem), sem gravar nada.
  - `setup-spreadsheet`: mapeia um arquivo e salva como base nova.
  - `explore-api`: descobre como uma API funciona sem salvar nada.
  - `setup-api`: configura e salva uma fonte de API.
  - `label-data`: escreve o dicionário de dados.
  - `find-insights`: testa hipóteses e propõe achados, cada um com um gráfico; nunca monta nada.

### Pré-requisitos

- Uma conta do Gym Rank X Business numa empresa com a Inteligência ligada. Até o Gym Rank X ligar, toda ferramenta da Inteligência responde `intel_disabled`.
- O papel de gestor do console para tudo que grava (importações, fontes, mapeamentos, dashboards, alertas, rótulos). O papel de leitura só consulta.

### Documentação

Todo procedimento e toda regra estão na documentação para agentes (em inglês): https://business.gymrankx.com/docs/intelligence/index.md (todas as páginas: https://business.gymrankx.com/docs/intelligence/llms.txt).
