---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos últimos exercícios, você atribuiu três tarefas diferentes ao agente de nuvem do Copilot para melhorar a experiência e adicionar funcionalidades. Embora o agente trabalhe de forma assíncrona e autônoma, ainda é importante monitorar essas tarefas.

Há várias ferramentas para gerenciar tarefas atribuídas ao agente de nuvem, incluindo a [página de agentes][agents-page] no GitHub.com. Nesse painel, você pode ver todas as tarefas do agente com pull requests (PRs) abertos, explorar as operações realizadas e orientar uma sessão em andamento.

Neste exercício, você vai:

- explorar a página de agentes para monitorar as tarefas do agente de nuvem.
- orientar uma sessão em andamento para pedir uma funcionalidade adicional.

## Cenário

Depois que a equipe atribuiu ao agente a criação do modo de alto contraste, percebeu que seria útil adicionar também um modo claro. Como o agente já estava atualizando o estilo do site e adicionando controles de alternância, a equipe decidiu incluir esse modo. Oriente o trabalho para que o agente implemente o modo claro além do alto contraste.

## Revisar as tarefas do agente de nuvem

Confira o estado atual das tarefas atribuídas ao agente de nuvem do Copilot.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe a lista de tarefas no painel principal e no painel à esquerda. Você deverá ver as tarefas atribuídas ao Copilot, incluindo:
   - atualizar a documentação do código.
   - gerar APIs para modificar jogos.
   - adicionar um modo de alto contraste ao site.
3. Selecione uma tarefa em andamento e revise as operações já executadas pelo Copilot. Elas podem incluir:
   - obter o código do repositório.
   - criar o ambiente de trabalho do Copilot.
   - configurar servidores do Model Context Protocol (MCP).
   - executar várias etapas para concluir a tarefa.

> [!NOTE]
> As etapas exibidas variam de acordo com o estado do trabalho e a abordagem adotada.

4. Observe também o painel de pull request à direita, que mostra o PR e os arquivos alterados para facilitar o monitoramento.

## Orientar o agente de nuvem

Agora que você viu as tarefas em andamento, peça ao Copilot para adicionar o modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão referente ao modo de alto contraste. O título exato varia conforme o nome usado pelo Copilot e o estado atual do trabalho.

    ![Sessão de acessibilidade no painel de agentes](../../../_images/ex6-accessibility-session.png)

2. Aguarde alguns minutos até que a sessão indique que a configuração foi concluída e o trabalho começou. Você verá mensagens semelhantes às descritas na interface.
3. Na caixa de diálogo **Steer active session while Copilot is working**, digite:

   ```plaintext
   Enquanto trabalhamos no modo de alto contraste, vamos adicionar também um modo claro. Deve haver um controle para esse modo, para que as pessoas possam selecionar a aparência desejada.
   ```

    ![Tarefa do agente de nuvem com a caixa de diálogo de orientação da sessão em destaque](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe como o Copilot confirma o recebimento do prompt e o inclui no fluxo de trabalho.

## Deixar o Copilot trabalhar

O Copilot incorporará a solicitação atualizada depois de concluir a etapa em andamento quando você enviou a mensagem.

Como antes, isso pode levar alguns minutos. Aproveite a espera para refletir sobre o que você aprendeu e explorou.

## Resumo e próximos passos

Neste exercício, você explorou a página de agentes do Copilot, o painel central para monitorar e orientar as tarefas do agente de nuvem. Você pode acompanhar tarefas ativas e concluídas, revisar o trabalho e redirecionar sessões em andamento para ajustar o escopo ou fornecer orientações adicionais.

Você:

- explorou o painel de agentes para monitorar as tarefas do agente de nuvem.
- redirecionou uma sessão em andamento para solicitar funcionalidade adicional.

Enquanto o Copilot conclui os recursos de acessibilidade, você pode [iterar sobre os pull requests criados pelo Copilot][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre agentes personalizados][custom-agents]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
