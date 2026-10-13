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
| `plugin:crm-profills:crm-profills` | Este plugin no Claude Code, vindo da conta do claude.ai (`crm-profills@synced` no `claude plugin list`) ou do `claude plugin install` |
| `crm-profills` no Codex | Este plugin, ou um `codex mcp add` com o mesmo nome |
| Outro nome | Servidor adicionado à mão com `claude mcp add` ou `codex mcp add` |

No Claude Code, a ordem é servidor manual, depois plugin, depois conector do claude.ai, e só o primeiro aparece. Com o plugin instalado, a linha do conector do claude.ai some e o plugin pede um login próprio: isso é esperado, não é falha. Com um servidor manual de mesma URL, a linha do plugin some e o manual é o que está em uso.

## 2. Resolva pelo estado

Com uma linha só, siga o estado dela. Com um servidor manual ou um plugin instalado duas vezes, resolva isso primeiro.

- **`✔ Connected`, ou Codex sem `Not logged in`.** A conexão funciona. Prove com `minha_agenda` sem parâmetros, que só lê. Uma lista de compromissos ou uma lista vazia confirmam.
- **`! Needs authentication`, `Not logged in`, só os tools `authenticate` e `complete_authentication`, ou a mensagem "Sessão sem usuário do CRM. Reconecte o conector."** Falta o login, ou a conexão venceu (ela dura 90 dias). Refaça só o login, sem adicionar o servidor de novo:
  - Claude Code numa sessão interativa: o vendedor digita `/mcp`, escolhe o servidor da lista e entra no navegador.
  - Claude Code fora dela, como no `claude -p`: o vendedor roda `claude mcp login "<nome da lista>"` no terminal dele.
  - Codex: o vendedor roda `codex mcp login <nome da lista>`.
  - Máquina sem navegador, como numa sessão SSH: o mesmo comando com `--no-browser` num terminal interativo (`ssh -t`). O vendedor abre a URL impressa no computador dele e cola no terminal a URL para onde o navegador voltou.
  - Conector do claude.ai: o vendedor clica em **Connect** no conector, em **Customize** e **Connectors**.
- **Nenhuma linha.** O CRM não está instalado. Siga [INSTALAR.md](../../INSTALAR.md).
- **Servidor manual na lista.** Um nome fora da tabela, com a URL do CRM, vem de um `claude mcp add` ou `codex mcp add` antigo. Fica o plugin. Diga ao vendedor que remova o manual com `claude mcp remove <nome>` ou `codex mcp remove <nome>`, e siga o login do plugin.
- **Plugin instalado duas vezes no Claude Code.** O `claude plugin list` mostra `crm-profills@synced` e `crm-profills@profills-skills`. Fica o da conta, que vale também no claude.ai e no Desktop. Sai o outro com `claude plugin uninstall crm-profills@profills-skills`.
- **Outro estado, como falha ao conectar.** O servidor não respondeu. Avise o vendedor e pare.

Depois de um login ou de uma remoção, se os tools do CRM não aparecem na sessão aberta, peça ao vendedor que abra uma sessão nova. A conexão está pronta quando `minha_agenda` devolve dados do CRM.

Um app fora do Claude e do ChatGPT recebe do CRM a página "Não foi possível conectar". Diga ao vendedor que o CRM só aceita o login do Claude e do ChatGPT.

## Depois da conexão

As regras de uso (qual tool chamar, o que confirmar antes de gravar, datas, paginação e erros) estão em [docs/agentes.md](../../docs/agentes.md). Leia essa página antes do primeiro pedido do vendedor.
