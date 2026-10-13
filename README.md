# CRM Profills MCP

![Fichas de cliente, proposta, agenda e gráfico ligadas a um núcleo central](docs/img/capa.png)

O conector liga o seu app de IA ao CRM da Profills. O agente busca clientes, lê propostas, agenda e o Desempenho, e registra atendimentos, compromissos e contatos em seu nome, com as mesmas permissões da sua tela. Ele também manda propostas para aprovação e marca propostas como enviadas. O conector só serve a quem tem conta ativa no CRM.

**Agente: comece por [docs/guia-para-agentes.md](docs/guia-para-agentes.md).**

## Como o pedido chega ao CRM

<img src="docs/img/caminho.svg" alt="O app de IA manda o pedido com o login do vendedor ao conector, que pede ao CRM em nome dele. O CRM devolve só o que o vendedor vê na tela." width="100%">

## Instalar

Endereço: `https://mcp.profills.com/mcp`. Você faz o login no CRM e clica em permitir. A senha nunca passa pelo agente.

<details open>
<summary><strong>Claude</strong></summary>

No app (claude.ai ou Claude Desktop): **Customize**, **Connectors** e **Add custom connector** com o endereço.

No terminal (Claude Code):

```bash
claude mcp add --transport http --scope user crm-profills https://mcp.profills.com/mcp
```

Depois digite `/mcp` e faça o login.

</details>

<details>
<summary><strong>ChatGPT</strong></summary>

No terminal (Codex):

```bash
codex mcp add crm-profills --url https://mcp.profills.com/mcp
```

O comando já abre o login.

No app: **Developer mode** e **Create app** com o endereço.

</details>

Teste pedindo "Mostre a minha agenda desta semana". A conexão funciona quando a resposta traz compromissos ou diz que a agenda está vazia. O [passo a passo](docs/instalacao.md) cobre cada tela e o login sem navegador.

## Documentação

- [Instalação](docs/instalacao.md): o passo a passo no Claude e no ChatGPT e como o login funciona.
- [Guia para agentes](docs/guia-para-agentes.md): o que o agente faz em ordem, regras de segurança, paginação, datas e erros.
- [Tools](docs/tools.md): os 18 tools, os parâmetros, o ciclo da proposta e os prompts.
- [Exemplos](docs/exemplos.md): pedidos do vendedor com a sequência de tools e as perguntas frequentes.
