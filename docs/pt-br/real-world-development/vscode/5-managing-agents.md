---
title: "Exercício 5 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Agentes personalizados][previous-lesson] |
|:--|

No modo de agente, o GitHub Copilot trabalha de forma autônoma: explora a base de código, propõe alterações, edita arquivos e executa comandos. Como esse trabalho acontece em tempo real no seu computador, o Copilot Chat no VS Code mostra as chamadas de ferramentas e as edições conforme acontecem. Você pode revisar cada diff no editor, aceitar ou rejeitar alterações e orientar a conversa com prompts de acompanhamento sem sair do ambiente de desenvolvimento integrado (IDE).

Neste exercício, você vai:

- monitorar em tempo real o trabalho do agente de acessibilidade.
- revisar e aceitar as edições propostas nos arquivos.
- orientar o agente durante a sessão para adicionar um controle do modo claro ao lado do alto contraste.

## Cenário

O agente personalizado de acessibilidade usado no [Exercício 4][previous-lesson] está implementando o modo de alto contraste. Enquanto o trabalho acontece, a equipe de design pede também um controle para o modo claro, para quem considera o tema padrão escuro demais. Em vez de começar uma conversa do zero, você orientará a sessão existente para ampliar o trabalho.

## Monitorar o progresso do Copilot

A exibição Copilot Chat mostra cada etapa do agente: os arquivos explorados, os comandos executados e as edições propostas. Cada edição aparece na conversa com um diff e os controles **Keep** e **Undo**.

1. Volte à exibição Copilot Chat usada no Exercício 4.
2. Enquanto o agente trabalha, percorra a conversa e observe as chamadas de ferramentas, leituras de arquivos, edições e comandos de terminal solicitados.
3. Quando o agente propuser uma edição, o diff aparecerá na conversa:
   - Selecione **Keep** para aceitar a alteração.
   - Selecione **Undo** para desfazê-la e permitir que o agente tente novamente.
4. Você também pode abrir o indicador **Files changed** na parte superior do chat para ver todos os arquivos alterados durante a sessão.

> [!NOTE]
> Se uma chamada de ferramenta pedir permissão, por exemplo, para executar um comando no terminal, o agente pausará e mostrará a opção **Allow**. Aprove somente os comandos que você considera seguros.

5. Aguarde o agente concluir o modo de alto contraste. Ele deverá apresentar uma mensagem final que resume as alterações.

## Orientar o agente para adicionar o modo claro

Agora que o modo de alto contraste está implementado, amplie a mesma conversa para adicionar um controle do modo claro. Como você continuará na mesma sessão, o agente manterá o contexto dos componentes alterados, da abordagem de estilos e do padrão de armazenamento local.

1. No campo de entrada do Copilot Chat, envie este prompt de acompanhamento:

   ```plaintext
   Bom trabalho! Agora adicione um controle para o modo claro ao lado do controle de alto contraste. O modo claro deve seguir o mesmo padrão: salvar a preferência no local storage e ficar acessível na mesma área da interface. As pessoas devem poder ativar o alto contraste e o modo claro de forma independente.
   ```

2. O agente retomará o trabalho, explorará as alterações já feitas e proporá mudanças para o modo claro.
3. Revise cada nova edição à medida que aparecer, usando **Keep** ou **Undo**.
4. Se o agente seguir uma direção indesejada — por exemplo, tentar refatorar os controles de uma forma que você não quer — interrompa-o selecionando o botão **Stop** ao lado do campo de entrada e envie uma instrução de acompanhamento para esclarecer o que precisa mudar.

> [!TIP]
> Orientar uma sessão em andamento costuma ser mais rápido do que recomeçar. Use prompts de acompanhamento para pedir ajustes, como "simplifique o CSS e use variáveis CSS em vez de duplicar seletores", corrigir a direção, como "não adicione um terceiro controle; integre o modo claro ao painel de configurações atual", ou ampliar o escopo, como "adicione atalhos de teclado para os dois controles".

## Revisar o conjunto de alterações

Antes de fazer commit do trabalho, revise rapidamente tudo o que o agente alterou.

1. Abra a exibição **Source Control** no VS Code.
2. Revise a lista completa de arquivos alterados. Você deverá ver atualizações nos componentes Astro, nos estilos e nos testes relacionados.
3. Abra alguns arquivos alterados e examine os diffs. Confirme se as práticas de acessibilidade do agente personalizado aparecem no código: atributos de Accessible Rich Internet Applications (ARIA), navegação por teclado, HTML semântico e persistência no armazenamento local.
4. Quando estiver satisfeito, prepare e faça commit das alterações no painel Source Control. Você as publicará na [próxima lição][next-lesson].

## Resumo e próximos passos

Você usou o modo de agente do Copilot Chat para monitorar e orientar uma sessão em andamento: revisou cada edição, aceitou ou rejeitou alterações e enviou prompts de acompanhamento sem perder o contexto.

Nesta lição, você:

- monitorou o trabalho de um agente em tempo real.
- aceitou e rejeitou edições individuais em arquivos.
- orientou uma sessão em andamento com prompts de acompanhamento.

Na [próxima lição][next-lesson], você publicará o trabalho de acessibilidade em um pull request e revisará as alterações feitas pelo agente personalizado local durante o laboratório.

## Recursos

- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo de agente][agent-mode]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
