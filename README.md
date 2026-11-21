# Gym Rank X Business: plugins

Official plugins of [Gym Rank X Business](https://business.gymrankx.com).

[English](#english) · [Português](#português)

---

## English

### grx-intelligence

Bring your HR data into Gym Rank X Business Intelligence and analyze it in conversation, from Claude, ChatGPT, Codex or any assistant that speaks MCP. Import spreadsheets, connect HR system APIs, ask questions, cross absences with exercise, and build dashboards, cohorts and alerts without leaving the chat.

### Before you start

- A Gym Rank X Business account in a company with **Intelligence enabled**. Gym Rank X enables it per company; until then every Intelligence tool answers `intel_disabled`.
- The **console manager** role to import, configure or build. A **console viewer** can only read.
- You sign in with your own Gym Rank X Business account (OAuth) and pick the company. Nothing is shared with other users.

### What you get on each app

| App | Tools (MCP) | Workflow skill | Specialist agents |
|---|---|---|---|
| Claude Code | yes | yes | yes |
| Claude Cowork | yes | yes | yes |
| Claude chat (web, desktop, mobile) | yes | yes | no |
| ChatGPT, Codex, other MCP clients | yes | no | no |

Where the skill does not load, the assistant can follow the same procedures from the [documentation for agents](https://business.gymrankx.com/docs/intelligence/llms.txt).

MCP server: `https://srv01.gymrankx.com/api/business/mcp` (Streamable HTTP, OAuth 2.1).

### Install

#### Claude Code

```
claude plugin install grx-intelligence --marketplace grxlabs/grx-plugins
```

Or, inside a session: `/plugin marketplace add grxlabs/grx-plugins`, then `/plugin install grx-intelligence@gymrankx`. Then type `/mcp`, choose **gym-rank-x** and sign in. A plugin you install on claude.ai also shows up in Claude Code.

#### Claude on the web, desktop and mobile, and Cowork (Free, Pro, Max)

1. Open **Customize > Plugins**, choose **Add marketplace** and enter `grxlabs/grx-plugins`.
2. Install **grx-intelligence**.
3. On the plugin's **Connectors** tab, connect **gym-rank-x** and sign in with your Gym Rank X Business account.

Only want the tools? **Customize > Connectors > Add > Add custom connector**, name it `Gym Rank X` and paste the MCP server URL above.

#### Claude for work (Team, Enterprise)

- An **Owner** adds the connector once for everyone: **Organization settings > Connectors > Add > Custom**, type **Web**, name `Gym Rank X`, the MCP server URL above, then **Add**.
- Each member connects with their own account: **Customize > Connectors**, find **Gym Rank X** (label **Custom**) and click **Connect**.
- For the skill and the agents, each member adds the marketplace in **Customize > Plugins**, as above, if the organization allows personal marketplaces.

#### ChatGPT (web, desktop, Business and Enterprise workspaces)

1. Open [chatgpt.com/plugins](https://chatgpt.com/plugins), click **+** and choose **Add custom MCP server**.
2. Name it `Gym Rank X` and paste the MCP server URL above as the **Server URL**.
3. Authentication: **OAuth**. Accept the warning and choose **Create as a plugin**.
4. Install the plugin and sign in with your Gym Rank X Business account.

In Business and Enterprise workspaces, admins decide who can add custom MCP servers; if you don't see the option, ask your workspace admin. ChatGPT asks for confirmation before tools that change data.

#### Codex (CLI, IDE extension and app)

```
codex mcp add gym-rank-x --url https://srv01.gymrankx.com/api/business/mcp
codex mcp login gym-rank-x
```

Check with `codex mcp list` or `/mcp`. In the IDE extension and the app, the server shows an **Authenticate** button until you sign in.

#### Any other MCP client

Add the MCP server URL above with OAuth. If the client has no OAuth, a console manager can create an API token in the console (**Configurações > Integrações**) and the client sends it as `Authorization: Bearer grxb_...`. Keep the token secret: it acts as you in that company.

### Try it

- "Import this spreadsheet of medical certificates."
- "Which teams had the most absences last quarter?"
- "Do people who work out in the app take fewer sick days?"
- "Build a dashboard with absences per month and per department."

### Privacy

CPF, e-mail and phone numbers are masked before they reach the assistant; the data stored in Gym Rank X stays real. More in [Connect to the Intelligence](https://business.gymrankx.com/docs/intelligence/connect.md#masking) and in the [privacy policy](https://gymrankx.com/terms/privacy).

### Documentation

- For agents (Markdown): https://business.gymrankx.com/docs/intelligence/llms.txt
- Start page: https://business.gymrankx.com/docs/intelligence/index.md

---

## Português

### grx-intelligence

Leve os dados de RH para a Inteligência do Gym Rank X Business e analise conversando, pelo Claude, ChatGPT, Codex ou qualquer assistente que fale MCP. Importe planilhas, conecte APIs do sistema de RH, faça perguntas, cruze afastamentos com exercício e monte dashboards, coortes e alertas sem sair do chat.

### Antes de começar

- Uma conta do Gym Rank X Business numa empresa com a **Inteligência ligada**. O Gym Rank X liga por empresa; até lá toda ferramenta da Inteligência responde `intel_disabled`.
- O papel de **gestor do console** para importar, configurar ou montar. O papel de **leitura** só consulta.
- Você entra com a sua própria conta do Gym Rank X Business (OAuth) e escolhe a empresa. Nada é compartilhado com outras pessoas.

### O que funciona em cada app

| App | Ferramentas (MCP) | Skill de fluxo | Agentes especialistas |
|---|---|---|---|
| Claude Code | sim | sim | sim |
| Claude Cowork | sim | sim | sim |
| Chat do Claude (web, desktop, celular) | sim | sim | não |
| ChatGPT, Codex, outros clientes MCP | sim | não | não |

Onde a skill não carrega, o assistente segue os mesmos procedimentos pela [documentação para agentes](https://business.gymrankx.com/docs/intelligence/llms.txt).

Servidor MCP: `https://srv01.gymrankx.com/api/business/mcp` (Streamable HTTP, OAuth 2.1).

### Instalar

#### Claude Code

```
claude plugin install grx-intelligence --marketplace grxlabs/grx-plugins
```

Ou, dentro de uma sessão: `/plugin marketplace add grxlabs/grx-plugins` e depois `/plugin install grx-intelligence@gymrankx`. Em seguida digite `/mcp`, escolha **gym-rank-x** e entre com a sua conta. Plugin instalado no claude.ai aparece também no Claude Code.

#### Claude na web, no desktop e no celular, e Cowork (Free, Pro, Max)

1. Abra **Customize > Plugins**, escolha **Add marketplace** e digite `grxlabs/grx-plugins`.
2. Instale o **grx-intelligence**.
3. Na aba **Connectors** do plugin, conecte o **gym-rank-x** e entre com a sua conta do Gym Rank X Business.

Só quer as ferramentas? **Customize > Connectors > Add > Add custom connector**, nome `Gym Rank X` e cole a URL do servidor MCP acima.

#### Claude para empresas (Team, Enterprise)

- Um **Owner** adiciona o conector uma vez para todos: **Organization settings > Connectors > Add > Custom**, tipo **Web**, nome `Gym Rank X`, a URL do servidor MCP acima e **Add**.
- Cada pessoa conecta com a própria conta: **Customize > Connectors**, encontre **Gym Rank X** (etiqueta **Custom**) e clique em **Connect**.
- Para a skill e os agentes, cada pessoa adiciona o marketplace em **Customize > Plugins**, como acima, se a organização permitir marketplaces pessoais.

#### ChatGPT (web, desktop e workspaces Business e Enterprise)

1. Abra [chatgpt.com/plugins](https://chatgpt.com/plugins), clique em **+** e escolha **Add custom MCP server**.
2. Dê o nome `Gym Rank X` e cole a URL do servidor MCP acima em **Server URL**.
3. Autenticação: **OAuth**. Aceite o aviso e escolha **Create as a plugin**.
4. Instale o plugin e entre com a sua conta do Gym Rank X Business.

Nos workspaces Business e Enterprise, quem administra decide quem pode adicionar servidores MCP próprios; se a opção não aparecer, peça ao administrador do workspace. O ChatGPT pede confirmação antes de ferramentas que alteram dados.

#### Codex (CLI, extensão de IDE e app)

```
codex mcp add gym-rank-x --url https://srv01.gymrankx.com/api/business/mcp
codex mcp login gym-rank-x
```

Confira com `codex mcp list` ou `/mcp`. Na extensão de IDE e no app, o servidor mostra o botão **Authenticate** até você entrar.

#### Qualquer outro cliente MCP

Adicione a URL do servidor MCP acima com OAuth. Se o cliente não tiver OAuth, um gestor cria um token de API no console (**Configurações > Integrações**) e o cliente manda `Authorization: Bearer grxb_...`. Guarde o token em segredo: ele age como você naquela empresa.

### Experimente

- "Importe esta planilha de atestados."
- "Quais times tiveram mais afastamentos no último trimestre?"
- "Quem treina no app falta menos por doença?"
- "Monte um dashboard com afastamentos por mês e por setor."

### Privacidade

CPF, e-mail e telefone são mascarados antes de chegar ao assistente; o dado guardado no Gym Rank X continua real. Mais em [Connect to the Intelligence](https://business.gymrankx.com/docs/intelligence/connect.md#masking) e na [política de privacidade](https://gymrankx.com/terms/privacy).

### Documentação

- Para agentes (Markdown, em inglês): https://business.gymrankx.com/docs/intelligence/llms.txt
- Página inicial: https://business.gymrankx.com/docs/intelligence/index.md
