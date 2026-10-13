---
name: crm-profills-conexao
description: Conexão do CRM Profills (https://mcp.profills.com/mcp) no Claude Code e no Codex. Use quando os tools do CRM faltam ou só aparecem authenticate e complete_authentication, quando o servidor mostra Needs authentication ou Not logged in, quando um tool responde "Reconecte o conector", quando os tools do CRM aparecem duas vezes, ou quando o vendedor pede para conectar ou reconectar o CRM.
---

# Conexão do CRM Profills

O servidor do CRM é `https://mcp.profills.com/mcp`. O login no CRM e o clique em permitir são sempre do vendedor. Você diz o passo e espera. A senha dele nunca passa por você.

## 1. Liste o que está ligado

Rode `claude mcp list` no Claude Code ou `codex mcp list` no Codex. Anote cada linha com a URL `https://mcp.profills.com/mcp`. A origem sai do nome:

| Nome na lista | Origem |
|---|---|
| `claude.ai CRM Profills`, ou `claude.ai` mais o nome que o vendedor deu | Conector da conta no claude.ai. Vale também no claude.ai e no Claude Desktop |
| `plugin:crm-profills:crm-profills` | Este plugin, no Claude Code |
| `crm-profills` no Codex | Este plugin, ou um `codex mcp add` com o mesmo nome |
| Outro nome | Servidor adicionado à mão com `claude mcp add` ou `codex mcp add` |

O Claude Code tira da lista o servidor do plugin quando um servidor manual tem a mesma URL. Com o plugin instalado e só a linha do manual na lista, o manual é o que está em uso.

## 2. Resolva pelo estado

Com uma linha só, siga o estado dela. Com duas ou mais, resolva a duplicata primeiro.

- **`✔ Connected`, ou Codex sem `Not logged in`.** A conexão funciona. Prove com `minha_agenda` sem parâmetros, que só lê. Uma lista de compromissos ou uma lista vazia confirmam.
- **`! Needs authentication`, `Not logged in`, só os tools `authenticate` e `complete_authentication`, ou a mensagem "Sessão sem usuário do CRM. Reconecte o conector."** Falta o login, ou a conexão venceu (ela dura 90 dias). Refaça só o login, sem adicionar o servidor de novo:
  - Claude Code numa sessão interativa: o vendedor digita `/mcp`, escolhe o servidor da lista e entra no navegador.
  - Claude Code fora dela, como no `claude -p`: o vendedor roda `claude mcp login "<nome da lista>"` no terminal dele.
  - Codex: o vendedor roda `codex mcp login <nome da lista>`.
  - Máquina sem navegador, como numa sessão SSH: o mesmo comando com `--no-browser` num terminal interativo (`ssh -t`). O vendedor abre a URL impressa no computador dele e cola no terminal a URL para onde o navegador voltou.
  - Conector do claude.ai: o vendedor clica em **Connect** no conector, em **Customize** e **Connectors**.
- **Nenhuma linha.** Instale o plugin e depois siga a linha do login.
  - Claude Code: `claude plugin marketplace add profillsbrasil/profills-skills` e `claude plugin install crm-profills@profills-skills`.
  - Codex: `codex plugin marketplace add profillsbrasil/profills-skills` e `codex plugin add crm-profills@profills-skills`.
- **Duas linhas ou mais com a mesma URL.** O agente recebe cada tool do CRM duas vezes. Fica uma linha só. Diga ao vendedor qual remover, nesta ordem:
  1. Com o conector do claude.ai na lista, ele fica, porque já cobre o claude.ai, o Claude Desktop e o Claude Code. Sai o plugin (`claude plugin uninstall crm-profills@profills-skills`) e sai o servidor manual.
  2. Sem o conector, fica o plugin. Sai o servidor manual com `claude mcp remove <nome>` ou `codex mcp remove <nome>`.
- **Outro estado, como falha ao conectar.** O servidor não respondeu. Avise o vendedor e pare.

Depois de um login ou de uma remoção, se os tools do CRM não aparecem na sessão aberta, peça ao vendedor que abra uma sessão nova. A conexão está pronta quando `minha_agenda` devolve dados do CRM.

Um app fora do Claude e do ChatGPT recebe do CRM a página "Não foi possível conectar". Diga ao vendedor que o CRM só aceita o login do Claude e do ChatGPT.

## Depois da conexão

As regras de uso (qual tool chamar, o que confirmar antes de gravar, datas, paginação e erros) estão em [docs/agentes.md](../../docs/agentes.md). Leia essa página antes do primeiro pedido do vendedor.
