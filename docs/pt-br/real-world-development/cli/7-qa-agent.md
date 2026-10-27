---
title: "Lição 7 - Criar e usar um agente de QA"
description: "Crie um agente personalizado de QA que combine os requisitos da issue, a skill quality-checks e o MCP do Playwright."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Você usou a skill `quality-checks` para executar verificações automatizadas e o MCP do Playwright para observar a filtragem no navegador. Agora, combine esses recursos em um agente personalizado com um processo de garantia de qualidade (QA) bem definido.

Nesta lição, você vai:

- explorar como um agente personalizado trabalha com instruções, skills e ferramentas MCP.
- criar e inspecionar um perfil reutilizável de QA.
- selecionar o agente de QA e revisar as descobertas dele em relação à issue de filtragem.

## Cenário

A Tailspin Toys quer revisar de forma consistente os requisitos, a qualidade do código, as verificações automatizadas, a cobertura de testes e o comportamento no navegador antes de abrir um pull request (PR). Um agente personalizado pode coordenar esse processo de QA e gerar um relatório reutilizável.

## O que é um agente personalizado?

Um agente personalizado é uma versão especializada do Copilot, definida por um perfil Markdown que descreve sua finalidade, suas instruções e as ferramentas disponíveis. Neste workshop, você definirá a função de QA em `.github/agents/qa.agent.md` e a selecionará no Copilot CLI.

As personalizações usadas até aqui têm funções diferentes. As instruções do repositório descrevem os padrões da equipe. A skill `quality-checks` reúne verificações repetíveis. O MCP do Playwright fornece ferramentas de navegador. O perfil de QA orienta o Copilot sobre como usar esses recursos para avaliar os requisitos e relatar as descobertas. O perfil não substitui esses recursos nem exige outra conversa.

## Criar o perfil de QA

Antes de abrir o PR do recurso, peça ao Copilot que crie um perfil reutilizável de QA. O perfil definirá as verificações e os limites que o agente deve respeitar.

1. Retorne ao Codespace e confirme que a conversa de filtragem está no modo Interactive.
2. Peça ao agente padrão que crie o agente personalizado:

   ```plaintext
   Crie um agente personalizado chamado QA em .github/agents/qa.agent.md. Ele deve verificar os recursos em relação às issues e aos requisitos acordados, seguir as instruções do repositório, executar a skill quality-checks, usar o MCP do Playwright para verificar o comportamento e adicionar testes quando houver lacunas na cobertura.

   Peça que o relatório indique cada requisito como aprovado, reprovado ou bloqueado, com evidências de apoio. O agente deve pedir autorização antes de alterar o código de implementação e não deve fazer commits nem abrir pull requests.

   Por enquanto, apenas crie o perfil para que eu possa revisá-lo.
   ```

## Inspecionar o perfil

Antes de usar o novo agente, revise o perfil para confirmar que o Copilot incluiu o fluxo de QA e os limites desejados. Assim, você evita um agente incompleto ou amplo demais que altere o recurso quando você só queria verificá-lo.

1. Digite `/diff` e abra `.github/agents/qa.agent.md`.
2. Leia o frontmatter. `description` é obrigatório; `name` é opcional, mas fornece um nome claro para exibição.
3. Leia as instruções do perfil e confirme que o QA começa pelos requisitos, segue as instruções do repositório, executa a skill `quality-checks` e usa o MCP do Playwright.
4. Confirme que o QA apresenta evidências, pede autorização antes de alterar o código de implementação e não faz commits nem abre pull requests.
5. Se o perfil não incluir alguma responsabilidade ou limite, peça ao agente padrão que o revise antes de continuar.

## Executar o QA para a issue

O Copilot CLI carrega os agentes do projeto quando uma conversa começa. Retome a conversa de filtragem depois de criar o perfil e selecione o QA para que ele possa usar a issue e as decisões de planejamento já disponíveis no contexto.

1. Ative o agente com este prompt:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Como você acabou de criar o agente, talvez ele não apareça na lista de agentes disponíveis. Mesmo assim, o comando acima o ativará.

2. Envie este prompt para pedir ao agente de QA que revise o recurso:

   ```plaintext
   Revise o recurso de filtragem em relação à issue e às decisões do nosso plano. Ele está pronto para um PR?
   ```

3. Aguarde o agente de QA concluir o trabalho.
4. Leia o relatório apresentado.

## Resumo e próximos passos

Você adicionou uma função especializada reutilizável ao fluxo de trabalho e revisou o resultado. Você:

- explorou como um agente personalizado trabalha com instruções, skills e ferramentas MCP.
- criou e inspecionou um perfil reutilizável de garantia de qualidade.
- selecionou o agente de QA e revisou as descobertas em relação à issue de filtragem.

Agora você tem a implementação, a atualização da skill, o perfil de QA, os testes e o relatório de verificação prontos para revisão. Em seguida, você [reunirá esses itens em um PR do recurso e usará o Agent Merge][next-lesson].

## Recursos

- [Criar agentes personalizados para o Copilot CLI][create-agents]
- [Configuração de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
