# Instalar o CRM da Profills

Esta página é para você, agente, que recebeu o pedido de instalar o CRM da Profills para o vendedor. Siga os passos em ordem. O vendedor não conhece os passos: você conduz e diz a ele só o que ele precisa clicar.

A instalação é o plugin `crm-profills`, do marketplace `profillsbrasil/profills-skills`. Ele traz o servidor do CRM (`https://mcp.profills.com/mcp`) e a skill `crm-profills-conexao`, que cuida do login vencido depois.

O login no CRM e o clique em permitir são sempre do vendedor. Você não pede a senha dele. Você também não clica na conta do claude.ai por ele: ali você diz o caminho e espera.

## 1. Descubra o app

| Você está em | Siga |
|---|---|
| claude.ai, Claude Desktop ou Claude Code | [Família Claude](#família-claude) |
| Codex: CLI, aba Codex do app do ChatGPT ou extensão de IDE | [Codex](#codex) |
| Chat do ChatGPT | [Chat do ChatGPT](#chat-do-chatgpt) |

Se não souber, rode `command -v claude` e `command -v codex`. Um app de chat sem terminal é o claude.ai, o Claude Desktop ou o ChatGPT.

## 2. Veja se o CRM já está ligado

- No Claude Code, rode `claude mcp list`. No Codex, rode `codex mcp list`.
- Procure uma linha com `https://mcp.profills.com/mcp`.
- Com `✔ Connected`, ou no Codex sem `Not logged in`, o CRM já funciona. Pule para [Confira](#confira).
- Com `! Needs authentication` ou `Not logged in`, falta só o login. Pule para o passo de login do seu app.
- Num app de chat, chame `minha_agenda`. Se o tool existe e responde, pule para [Confira](#confira).

## Família Claude

Uma instalação na conta do claude.ai vale no claude.ai, no Claude Desktop e no Claude Code logado com a mesma conta.

1. Peça ao vendedor que abra `https://claude.ai/customize/plugins` (no Desktop: **Personalização** e **Plugins**).
2. Ele clica em **Adicionar**, depois em **Adicionar marketplace**, cola `profillsbrasil/profills-skills` e confirma. Os menus podem estar em inglês: **Customize**, **Plugins**, **Add** e **Add marketplace**.
3. Na lista, ele clica em **Adicionar** na linha do **Crm profills**.
4. Ele abre o plugin, vai na aba **Conectores** e olha o **CRM Profills**. Com **Conectado**, está pronto. Senão, ele clica em **Conectar**, entra com a conta do CRM e clica em permitir.
5. No Claude Code, o plugin chega na sessão seguinte como `crm-profills@synced`, ou na sessão aberta com `/reload-plugins`. O Claude Code pede um login próprio: o vendedor digita `/mcp`, escolhe `plugin:crm-profills:crm-profills`, entra no CRM e clica em permitir. Você não faz esse login pelo terminal, porque o comando exige um terminal interativo.

Um aviso pode aparecer no passo 2: "A sincronização automática exige que o Claude GitHub App tenha acesso a este repositório". Ele trata só de receber versões novas do plugin sozinho. O vendedor pode liberar ou ignorar, e a instalação funciona dos dois jeitos.

O plugin não chega da conta ao Claude Code quando o Claude Code entra por chave de API, Bedrock ou outro provedor de nuvem. Nesse caso, instale direto no Claude Code. O plugin fica só naquela máquina:

```bash
claude plugin install crm-profills --marketplace profillsbrasil/profills-skills
```

Depois, o mesmo login do passo 5.

## Codex

A instalação vale no Codex CLI, na aba Codex do app do ChatGPT e na extensão de IDE da mesma máquina, porque os três leem o mesmo `~/.codex`. Você roda os comandos:

```bash
codex plugin marketplace add profillsbrasil/profills-skills
codex plugin add crm-profills@profills-skills
codex mcp login crm-profills
```

O `codex mcp login` imprime uma URL e espera. Mostre a URL ao vendedor. Ele abre a URL no navegador, entra com a conta do CRM e clica em permitir. Sem navegador na máquina, use `codex mcp login --no-browser crm-profills`: o vendedor abre a URL no computador dele e cola no terminal a URL para onde o navegador voltou, que começa com `http://127.0.0.1`.

Os tools do CRM aparecem na sessão seguinte. No app do ChatGPT, feche e abra o app.

## Chat do ChatGPT

Nos planos Plus e Pro, o chat do ChatGPT não grava no CRM por servidor próprio: o Plus não aceita servidor próprio, e o Pro só lê. Diga ao vendedor que use a aba Codex do app do ChatGPT e cole lá este pedido:

> Instale o CRM da Profills seguindo https://raw.githubusercontent.com/profillsbrasil/crm-profills-mcp-docs/main/INSTALAR.md

## Confira

Chame `minha_agenda` sem parâmetros. Uma lista de compromissos ou uma lista vazia provam que a conexão funciona. Diga ao vendedor que o CRM está pronto e dê um exemplo de pedido, como "Mostre a minha agenda desta semana".

Se o tool responder com erro, ou os tools do CRM aparecerem duas vezes, siga a skill [crm-profills-conexao](skills/crm-profills-conexao/SKILL.md). Para usar o CRM depois da instalação, leia [docs/agentes.md](docs/agentes.md).
