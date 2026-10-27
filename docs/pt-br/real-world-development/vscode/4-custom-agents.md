---
title: "Exercício 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] do GitHub Copilot permitem criar assistentes de inteligência artificial (IA) especializados em tarefas ou domínios específicos do fluxo de desenvolvimento. Você define agentes com arquivos Markdown na pasta `.github/agents` do repositório e fornece instruções focadas, boas práticas, padrões de código e conhecimento do domínio. Assim, as equipes podem transformar sua experiência em agentes reutilizáveis: um agente de acessibilidade que aplica as [Diretrizes de Acessibilidade para Conteúdo Web (WCAG, na sigla em inglês)][wcag], um agente de segurança que segue práticas de programação segura ou um agente de testes que mantém padrões consistentes.

Os agentes personalizados são definidos em arquivos Markdown na pasta `.github/agents` do projeto ou globalmente em `~/.copilot/agents`. Cada arquivo tem um frontmatter YAML com pelo menos `name` e `description`, seguido de um prompt Markdown que define o comportamento, a especialidade e as instruções do agente.

### Agentes personalizados e skills de agente

Agentes personalizados e [skills de agente][agent-skills-concept] têm algumas semelhanças: ambos são definidos principalmente em arquivos Markdown e orientam uma IA sobre como executar tarefas. A distinção mais simples é: **o agente personalizado é quem executa o trabalho; as skills são as ferramentas**.

Os agentes personalizados têm sua própria janela de contexto e podem orquestrar skills e até outros agentes. Neste laboratório, o agente personalizado de acessibilidade revisará e atualizará o site conforme as diretrizes de acessibilidade. Durante esse trabalho, ele poderá usar skills, como uma para executar o fluxo de pull request ou outra para executar e gerenciar testes.

> [!NOTE]
> Não há uma única maneira correta de criar um agente personalizado. Como em outros usos de IA, teste e itere para encontrar o que funciona em cada ambiente e cenário.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Cenário

A Tailspin Toys quer que sua plataforma de financiamento coletivo seja acessível a todas as pessoas, independentemente de suas capacidades visuais ou preferências. Comentários recentes indicam que algumas pessoas têm dificuldade para ler o tema escuro atual por causa do contraste insuficiente entre o texto e o fundo. Para resolver esse problema de acessibilidade, a equipe de design solicitou um modo de alto contraste que possa ser ativado e desativado.

Como a acessibilidade é importante, você usará um agente personalizado para implementar esse recurso.

Neste exercício, você vai:

- revisar um agente personalizado de acessibilidade existente.
- usar o agente de acessibilidade no Copilot Chat para implementar o modo de alto contraste.

## Revisar o agente personalizado de acessibilidade

Já existe um agente personalizado de acessibilidade no projeto. Revise seu conteúdo para entender como ele orientará o Copilot.

Crie uma branch a partir de `main` para manter o PR de filtragem do Exercício 3 separado do trabalho de acessibilidade.

1. Retorne ao codespace, abra um terminal e execute:

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

Depois de preparar a branch, revise o agente personalizado para entender como ele orientará o Copilot.

2. Abra `.github/agents/accessibility.md`.
3. Observe o frontmatter YAML com os campos `name` e `description`.

> [!CAUTION]
> O frontmatter com `name` e `description` é obrigatório para agentes personalizados.

4. Examine as seções seguintes, que descrevem:
   - as principais responsabilidades ao gerar código para um site acessível.
   - as boas práticas de acessibilidade.
   - exemplos de código HTML, CSS e JavaScript.
   - problemas e erros comuns.

## Usar o agente personalizado no Copilot Chat

O VS Code exibe todos os agentes personalizados definidos em `.github/agents` no menu de agentes, na parte inferior da exibição Copilot Chat. Você pode selecionar um agente para limitar a sessão às instruções e ferramentas dele.

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de começar, retorne ao codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam conforme o exercício; as etapas correspondentes indicarão o que selecionar.

1. No menu de agentes da exibição Chat, selecione **Agent**, caso ainda não esteja selecionado.

   ![Seletor de agentes na exibição Chat](../../../_images/shared-chat-mode-selector.png)

2. Abra o menu de agentes na parte inferior da exibição Chat. Ele mostra o agente ativo; por padrão, **default**.
3. Selecione **Accessibility agent** na lista.
4. Envie este prompt ao agente de acessibilidade:

   ```plaintext
   Adicione ao site um modo de alto contraste. Deve haver um controle para ativá-lo, e a preferência deve continuar salva após recarregar a página por meio do local storage do navegador.
   ```

5. O Copilot Chat explorará a base de código, proporá alterações e editará os arquivos do projeto. Cada edição aparecerá na conversa com o caminho do arquivo e um diff para revisão.
6. Observe o indicador **Files changed**, que é atualizado à medida que o agente altera arquivos.

> [!NOTE]
> Esse processo pode levar alguns minutos. O Copilot fará alterações reais no repositório, como editar componentes Astro, CSS e testes relacionados.

Você revisará e orientará o trabalho em andamento no próximo exercício.

## Resumo e próximos passos

Neste exercício, você explorou os [agentes personalizados][custom-agents] do GitHub Copilot, assistentes especializados em tarefas e domínios específicos. Eles permitem transformar o conhecimento e os padrões da equipe em orientações reutilizáveis para o Copilot.

Você explorou como:

- definir agentes personalizados.
- usar um agente personalizado no modo de agente do Copilot Chat.

Em seguida, você [monitorará e orientará o trabalho do agente][next-lesson], revisando as alterações à medida que forem feitas e adicionando um controle para o modo claro à mesma sessão.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Criar agentes personalizados no IDE][creating-custom-agents-ide]
- [Agentes personalizados no VS Code][custom-agents-vscode]
- [Configuração de agentes personalizados][custom-agents-config]
- [Agentes personalizados no awesome-copilot][awesome-copilot-agents]

---

| [← Lição anterior: Testar seu recurso com o servidor MCP do Playwright][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
