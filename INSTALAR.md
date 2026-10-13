# Instalar o CRM da Profills

Esta página é para você, agente, que recebeu o pedido de instalar o CRM da Profills para o vendedor. Siga os passos em ordem. O vendedor não conhece os passos: você conduz e diz a ele só o que ele precisa clicar.

A instalação é o plugin `crm-profills`, do marketplace `profillsbrasil/profills-skills`. Ele traz o servidor do CRM (`https://mcp.profills.com/mcp`) e a skill `crm-profills-conexao`, que cuida do login vencido depois.

O login no CRM e o clique em permitir são sempre do vendedor. Você não pede a senha dele. Você também não clica na conta do claude.ai por ele: ali você diz o caminho e espera.

## 1. Descubra o app

| Você está em | Siga |
|---|---|
| claude.ai, Claude Desktop ou Claude Code | [Família Claude](#família-claude) |
| Codex: CLI ou aba Codex do app do ChatGPT | [Codex](#codex) |
| Chat do ChatGPT | [Chat do ChatGPT](#chat-do-chatgpt) |

Se não souber, rode `command -v claude` e `command -v codex`. Um app de chat sem terminal é o claude.ai, o Claude Desktop ou o ChatGPT.

## 2. Veja se o CRM já está ligado

- No Claude Code, rode `claude mcp list`. No Codex, rode `codex mcp list`.
- Procure uma linha com `https://mcp.profills.com/mcp`.
- Com `✔ Connected`, ou no Codex sem `Not logged in`, o CRM já funciona. Pule para [Confira](#confira).
- Com `! Needs authentication` ou `Not logged in`, falta só o login. Pule para o passo de login do seu app.
- Num app de chat, chame `minha_agenda`. Se o tool existe e responde, pule para [Confira](#confira).

## Família Claude

Uma instalação na conta do claude.ai vale no claude.ai, no Claude Desktop e no Claude Code logado com a mesma conta. Plugin na conta existe nos planos Pro, Max, Team e Enterprise. No plano Free, siga [Plano Free](#plano-free).

1. Peça ao vendedor que abra `https://claude.ai/customize/plugins` (no Desktop: **Personalização** e **Plugins**).
2. Ele clica em **Adicionar**, depois em **Adicionar marketplace**, cola `profillsbrasil/profills-skills` e confirma. Os menus podem estar em inglês: **Customize**, **Plugins**, **Add** e **Add marketplace**.
3. Na lista, ele clica em **Adicionar** na linha do **Crm profills**.
4. Ele abre o plugin, vai na aba **Conectores** e olha o **CRM Profills**. Com **Conectado**, está pronto. Senão, ele clica em **Conectar**, entra com a conta do CRM e clica em permitir. Nos planos Team e Enterprise, só o Owner adiciona o conector à organização. Se o botão não aparecer para o vendedor, ele pede ao Owner e depois clica em **Conectar**.
5. No Claude Code, o plugin chega na sessão seguinte como `crm-profills@synced`, ou na sessão aberta com `/reload-plugins`. O Claude Code pede um login próprio: o vendedor digita `/mcp`, escolhe `plugin:crm-profills:crm-profills`, entra no CRM e clica em permitir. Você não faz esse login pelo terminal, porque o comando exige um terminal interativo.

Um aviso pode aparecer no passo 2: "A sincronização automática exige que o Claude GitHub App tenha acesso a este repositório". Ele trata só de receber versões novas do plugin sozinho. O vendedor pode liberar ou ignorar, e a instalação funciona dos dois jeitos.

O plugin não chega da conta ao Claude Code quando o Claude Code entra por chave de API, Bedrock ou outro provedor de nuvem. Nesse caso, instale direto no Claude Code. O plugin fica só naquela máquina:

```bash
claude plugin install crm-profills --marketplace profillsbrasil/profills-skills
```

Depois, o mesmo login do passo 5.

### Plano Free

O plano Free não tem plugin na conta, mas aceita 1 conector personalizado. Mande ao vendedor este link, que abre o formulário já preenchido:

`https://claude.ai/customize/connectors?modal=add-custom-connector&connectorName=CRM%20Profills&connectorUrl=https%3A%2F%2Fmcp.profills.com%2Fmcp`

Ele confere, clica em adicionar, depois em **Conectar**, entra com a conta do CRM e clica em permitir. O conector aparece também no Claude Desktop e no Claude Code logado com a mesma conta.

## Codex

A instalação vale no Codex CLI da máquina. A aba Codex do app do ChatGPT lê o mesmo `~/.codex`, então deve mostrar o plugin depois que o app é fechado e aberto de novo. Você roda os comandos:

```bash
codex plugin marketplace add profillsbrasil/profills-skills
codex plugin add crm-profills@profills-skills
codex mcp login crm-profills
```

O `codex mcp login` imprime uma URL e tenta abrir o navegador. Mostre a URL ao vendedor. Ele entra com a conta do CRM e clica em permitir. Se ele já estiver logado no CRM nesse navegador, o login pode terminar sozinho, com `Successfully logged in`. Sem navegador na máquina, use `codex mcp login --no-browser crm-profills`: o vendedor abre a URL no computador dele e cola no terminal a URL para onde o navegador voltou, que começa com `http://127.0.0.1`.

Os tools do CRM aparecem na sessão seguinte. No app do ChatGPT, feche e abra o app. Se a aba Codex não mostrar os tools do CRM, use o Codex CLI.

## Chat do ChatGPT

Nos planos Plus e Pro, o chat do ChatGPT não grava no CRM por servidor próprio. A OpenAI libera escrita por servidor próprio só nos planos Business, Enterprise e Edu, e o Pro só lê. Diga ao vendedor que use o Codex, no terminal ou na aba Codex do app do ChatGPT, e cole lá este pedido:

> Instale o CRM da Profills seguindo https://raw.githubusercontent.com/profillsbrasil/crm-profills-mcp-docs/main/INSTALAR.md

## Confira

Chame `minha_agenda` sem parâmetros. Uma lista de compromissos ou uma lista vazia provam que a conexão funciona. Diga ao vendedor que o CRM está pronto e dê um exemplo de pedido, como "Mostre a minha agenda desta semana".

Se o tool responder com erro, ou os tools do CRM aparecerem duas vezes, siga a skill [crm-profills-conexao](skills/crm-profills-conexao/SKILL.md). Para usar o CRM depois da instalação, leia [docs/agentes.md](docs/agentes.md).
