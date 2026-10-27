---
title: "Exercício 3 - Agentes personalizados"
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

Neste exercício, você explorará:

- como os agentes personalizados são definidos.
- como atribuir uma tarefa a um agente personalizado.

## Cenário

A Tailspin Toys quer que sua plataforma de financiamento coletivo seja acessível a todas as pessoas, independentemente de suas capacidades visuais ou preferências. Comentários recentes indicam que algumas pessoas têm dificuldade para ler o tema escuro atual por causa do contraste insuficiente entre o texto e o fundo. Para resolver esse problema de acessibilidade, a equipe de design solicitou um modo de alto contraste que possa ser ativado e desativado.

Como a acessibilidade é importante, você usará um agente personalizado para implementar esse recurso rapidamente.

## Revisar o agente personalizado de acessibilidade

Já existe um agente personalizado de acessibilidade no projeto. Revise seu conteúdo para entender como ele orientará o Copilot.

Retorne ao codespace e examine o arquivo do agente:

1. Abra `.github/agents/accessibility.md`.
2. Observe o frontmatter YAML com os campos `name` e `description`.

> [!CAUTION]
> O frontmatter com `name` e `description` é obrigatório para agentes personalizados.

3. Examine as seções seguintes, que descrevem:
   - as principais responsabilidades ao gerar código para um site acessível.
   - as boas práticas de acessibilidade.
   - exemplos de código HTML, CSS e JavaScript.
   - problemas e erros comuns.

## Criar e atribuir uma issue

O painel de agentes é o local central para trabalhar com agentes no ambiente. Você pode atribuir tarefas ao agente de nuvem do Copilot, monitorar o trabalho e redirecionar uma sessão em andamento. Vamos atribuir ao Copilot a tarefa de criar o modo de alto contraste.

1. Acesse seu repositório.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de criação de issue.
4. Selecione **Blank issue**.
5. No campo **Title**, digite `Add high contrast mode to website`.
6. No campo **Description**, digite:

   ```plaintext
   Precisamos de um modo de alto contraste para o site. Deve haver um controle que permita ativá-lo. A preferência deve ser armazenada no local storage do navegador.
   ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de atribuição.
9. Selecione **Accessibility agent** na lista de agentes personalizados.

    ![Atribuição do agente de nuvem com o agente de acessibilidade selecionado](../../../_images/ex5-select-custom-agent.png)

10. Selecione **Assign**.
11. O Copilot começará a trabalhar em segundo plano.

## Resumo e próximos passos

Neste exercício, você explorou os [agentes personalizados][custom-agents] do GitHub Copilot: assistentes de IA especializados em tarefas e domínios específicos. Com eles, é possível transformar o conhecimento e os padrões da equipe em agentes reutilizáveis.

Você explorou como:

- definir agentes personalizados.
- atribuir uma tarefa a um agente personalizado.

Enquanto o Copilot implementa o modo de alto contraste, você pode [monitorar e orientar a sessão do agente][next-lesson] no painel de agentes.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Preparar sua organização para usar agentes personalizados][org-custom-agents]
- [Preparar sua empresa para usar agentes personalizados][enterprise-custom-agents]

---

| [← Lição anterior: Agente de nuvem do GitHub Copilot][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
