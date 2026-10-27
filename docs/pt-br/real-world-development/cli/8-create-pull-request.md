---
title: "Lição 8 - Criar e integrar o PR do recurso"
description: "Revise a filtragem, as instruções, a skill, o perfil de QA e os testes; depois crie um PR e use o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

A implementação da filtragem, as atualizações das instruções e da skill, o perfil de garantia de qualidade (QA) e os testes estão salvos em uma única branch. Agora, revise todos esses itens e abra um pull request (PR). Você integrou manualmente o PR das avaliações por estrelas; desta vez, deixe que o **Agent Merge** gerencie o processo.

> [!NOTE]
> Normalmente, dividiríamos o recurso, as atualizações das instruções, a skill e o agente de QA em alguns PRs. Para simplificar o workshop, mantivemos todo o fluxo de filtragem e qualidade em uma conversa e branch e incluiremos o trabalho neste PR.

Nesta lição, você vai:

- entender o que é o Agent Merge e como ele automatiza o ciclo de merge.
- inspecionar a alteração completa do recurso e as evidências de verificação.
- criar o PR de filtragem.
- habilitar o Agent Merge somente depois da revisão e confirmar o merge.

## Cenário

Durante o fluxo de filtragem, você usou o Copilot para planejar, implementar e verificar um recurso. Agora, a Tailspin Toys quer automatizar o trabalho restante do PR, mantendo a autorização do merge sob o controle de quem desenvolve.

## Apresentação do Agent Merge

O **Agent Merge** automatiza o trabalho restante necessário para concluir um pull request. Quando você o habilita, o Copilot resolve os impedimentos do PR — corrige falhas de integração contínua (CI), responde a comentários de revisão e faz rebase quando necessário — e, em seguida, habilita o merge automático do GitHub quando as configurações do repositório permitem.

Até agora, você selecionou **Merge pull request** manualmente. O Agent Merge pode assumir essa responsabilidade. Revise as alterações e confirme que estão prontas antes de habilitá-lo.

## Usar o Agent Merge para gerenciar o PR

Com todo o código criado, revise as alterações, crie o PR e deixe que o Agent Merge gerencie o restante do processo.

1. Retorne ao Codespace.
2. Abra a caixa de diálogo do agente digitando `/agent`.
3. Selecione **Default** na lista e pressione <kbd>Enter</kbd>.
4. Crie um PR com o comando `/pr create`.
5. Ative o Agent Merge com `/pr agentmerge`.
6. O Copilot acompanhará a integração contínua do PR. Quando todas as verificações forem aprovadas, ele fará o merge.
7. Confirme que aparece uma mensagem do Copilot semelhante a `PR #14 was squash-merged successfully.`

> [!IMPORTANT]
> O Agent Merge não ignora aprovações obrigatórias, proteção de branch, filas de merge, configurações do repositório nem permissões ausentes. Se houver um bloqueio, leia o motivo informado e conclua manualmente o merge revisado quando o repositório permitir.

## Resumo e próximos passos

Você automatizou várias partes do desenvolvimento, incluindo a geração e validação de código e o processo de pull request. Você:

- entendeu o que é o Agent Merge e como ele automatiza o ciclo de merge.
- inspecionou a alteração completa do recurso e as evidências de verificação.
- criou o PR de filtragem.
- habilitou o Agent Merge somente depois da revisão e confirmou que o PR foi integrado.

Em seguida, você [explorará outros comandos de barra úteis do Copilot CLI][next-lesson] para contexto, modelos, compartilhamento e delegação opcional para a nuvem.

## Recursos

- [Gerenciar pull requests com o Copilot CLI][manage-prs]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
