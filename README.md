# CRM Profills MCP

![Fichas de cliente, proposta, agenda e gráfico ligadas a um núcleo central](docs/img/capa.png)

O conector liga o seu app de IA ao CRM da Profills. O agente busca clientes, lê propostas, agenda e o Desempenho, e registra atendimentos, compromissos e contatos em seu nome, com as mesmas permissões da sua tela. Ele também manda propostas para aprovação e marca propostas como enviadas. O conector só serve a quem tem conta ativa no CRM.

Este repositório também é o plugin `crm-profills` do Claude e do Codex. O plugin traz o servidor do CRM e a skill `crm-profills-conexao`, que confere a conexão, refaz o login e acha servidor duplicado.

**Agente: para instalar, siga [INSTALAR.md](INSTALAR.md). Para usar o CRM, leia [docs/agentes.md](docs/agentes.md).**

## Como o pedido chega ao CRM

<img src="docs/img/caminho.svg" alt="O app de IA manda o pedido com o login do vendedor ao conector, que pede ao CRM em nome dele. O CRM devolve só o que o vendedor vê na tela." width="100%">

## Instalar

Cole este pedido no seu agente, no Claude Code, no claude.ai, no Claude Desktop ou no Codex (inclusive a aba Codex do app do ChatGPT):

```text
Instale o CRM da Profills seguindo https://raw.githubusercontent.com/profillsbrasil/crm-profills-mcp-docs/main/INSTALAR.md
```

O agente descobre o app, instala o plugin `crm-profills` e pede o seu login no CRM. Você entra com a sua conta e clica em permitir. A senha nunca passa pelo agente. No claude.ai, uma instalação vale também no Claude Desktop e no Claude Code da mesma conta. No Codex, vale na máquina onde foi instalado.

O passo a passo que o agente segue está em [INSTALAR.md](INSTALAR.md). Para testar, peça "Mostre a minha agenda desta semana".

## O que a tela de consentimento mostra

A tela tem o título "Conectar" seguido do nome do app, e o texto "quer acessar o CRM em seu nome.". Ela lista o app, o endereço para onde o login volta e a sua conta. O bloco "O que o acesso permite" diz duas coisas:

- "Ler clientes, propostas, agenda e Desempenho que você já vê no CRM."
- "Registrar, editar e excluir os seus atendimentos e compromissos, com as regras da tela."

Com o mesmo acesso, o agente também cria e edita contatos e manda propostas para aprovação ou marca a proposta como enviada. Uma conta inativa ou em análise recebe uma página de erro no lugar da tela de consentimento.

## Quanto tempo o login dura

O servidor de login é o próprio CRM. O app abre a página de login do CRM, você entra e permite, e o CRM entrega ao app um acesso em seu nome. O conector confere esse acesso em cada pedido e nunca vê a sua senha.

| Item | Duração |
|---|---|
| Acesso | 1 hora, renovado sozinho pelo app |
| Renovação do acesso, contada do último uso | 30 dias |
| Conexão inteira, contada do primeiro login | 90 dias |
| Pedido de consentimento aberto | 10 minutos |

Depois de 90 dias, ou se a conexão cair antes disso, faça o login de novo.

## Documentação

- [Instalar](INSTALAR.md): o passo a passo que o agente segue em cada app.
- [Guia para agentes](docs/agentes.md): o que o agente faz em ordem, regras de segurança, o ciclo da proposta, erros, exemplos e perguntas frequentes.
- [Tools e prompts](docs/tools.md): os 18 tools e os 2 prompts com a descrição e os parâmetros, gerados pelo código do servidor.
- [Skill de conexão](skills/crm-profills-conexao/SKILL.md): como o agente confere a conexão, refaz o login e tira servidor duplicado.
