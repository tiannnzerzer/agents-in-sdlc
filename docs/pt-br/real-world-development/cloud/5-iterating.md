---
title: "Exercício 5: Iterar sobre o trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

## Revisar o trabalho

Ao longo deste laboratório, você trabalhou com o GitHub Copilot em tarefas para melhorar a experiência e adicionar funcionalidades. Você pediu que o Copilot documentasse o código, criasse um recurso de jogos relacionados para a equipe de design e implementasse recursos de acessibilidade, incluindo controles de alto contraste e modo claro. Agora, examine as alterações e, se necessário, dê feedback ao Copilot para aprimorar o trabalho.

### Cenário

Como já vimos, os fundamentos do design de software e do DevOps não mudam com a inclusão da IA generativa. É importante revisar o código gerado e seguir o processo normal de DevOps. Revise as sugestões do GitHub Copilot para documentação, jogos relacionados e acessibilidade antes de habilitar a revisão para o restante da equipe.

## Segurança e agente de nuvem do GitHub Copilot

Como o agente de nuvem do Copilot executa tarefas de forma assíncrona e sem supervisão, existem restrições de segurança para proteger o ambiente:

- O Copilot tem acesso de leitura ao repositório e acesso de gravação **somente** à branch que usará para o código.
- O agente de nuvem é executado no GitHub Actions, que cria um ambiente separado e temporário para o trabalho.
- Os workflows do GitHub Actions precisam da aprovação de uma pessoa antes de serem executados.
- [O acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluindo o acesso a servidores do Model Context Protocol (MCP).

## Revisar a documentação gerada

Comece pelo primeiro pull request (PR) criado pelo agente de nuvem do GitHub Copilot: o PR que adiciona documentação ao código. Você usará a interface padrão de PR do GitHub.com.

> [!NOTE]
> Ao abrir o PR, talvez apareça um aviso de que o Copilot está bloqueado por um firewall. Isso é **esperado**, pois o acesso do Copilot a recursos externos é limitado por padrão, inclusive a servidores MCP externos. Se desejar, você pode [personalizar ou desabilitar o firewall do agente de nuvem][agent-firewall].

1. Volte ao repositório no github.com.
2. Selecione **Pull Requests** para abrir a lista de pull requests.
3. Abra o PR com um título semelhante a **Add missing documentation**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, o PR terá a indicação **[WIP]** (work in progress, ou trabalho em andamento). Aguarde a conclusão, que pode levar alguns minutos.

4. Quando o PR estiver pronto, selecione a guia **Files changed** e revise as alterações.

    ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Examine o código atualizado, que inclui comentários de documentação TypeScript (TSDoc) e outra documentação. As alterações exatas variam. Procure comentários TSDoc, convenções de TypeScript e cabeçalhos de arquivo, seguindo os arquivos de instrução personalizados que você examinou no Exercício 2.
6. Quando a revisão estiver concluída, volte à guia **Conversation** e role a página para baixo.
7. Procure um indicador de que há workflows aguardando aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

    ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

9. Os workflows deverão aparecer na seção de verificações do pull request. Se tudo correr bem, as verificações do projeto para o aplicativo Astro único serão aprovadas. Isso pode levar alguns minutos.

## Pedir alterações ao GitHub Copilot

O trabalho com o Copilot em um pull request não é uma via de mão única. Você pode mencionar o Copilot em comentários do PR ou comentários em linhas específicas do código, como faria com outra pessoa da equipe. O Copilot lê esses comentários e inicia outra sessão para atendê-los. Como os resultados não são determinísticos, não há um texto único a recomendar. Algumas ideias de solicitações:

- Adicionar no início de cada arquivo de código um cabeçalho com uma breve descrição da finalidade.
- Adicionar comentários TSDoc a arquivos TypeScript e Astro.
- Criar um README com uma descrição da estrutura do aplicativo Astro.

1. Publique um comentário pedindo uma alteração na documentação gerada e mencione **@copilot**. Use uma das ideias acima ou outra sugestão que gostaria de ver no código.
2. Selecione **View Session** para acompanhar o trabalho do Copilot. Observe que ele inicia uma nova sessão para fazer as atualizações.
3. Selecione **Back to pull request** para voltar ao pull request.

    ![Voltar ao pull request](../../../_images/ex7-back-to-pr.png)

4. Quando o Copilot concluir as alterações, deverá aparecer um novo commit no PR.
5. Selecione **Files changed** para revisar as alterações.

Continue iterando até ficar satisfeito. Depois, converta o PR de rascunho em pronto para revisão e faça o merge na branch principal.

![Converter o PR em pronto para revisão](../../../_images/ex7-ready-for-review.png)

## Revisar o recurso de jogos relacionados

Agora, volte ao PR criado pelo Copilot para resolver a issue de jogos relacionados na página de detalhes.

1. Acesse o repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título semelhante a **Show related games on the game details page**.
4. Selecione a guia **Files changed** e revise o código gerado.
5. Quando a revisão estiver concluída, volte à guia **Conversation** e role para baixo.
6. Procure um indicador de que há workflows aguardando aprovação.
7. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

    ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

8. Os workflows deverão aparecer na seção de verificações do pull request. Se tudo correr bem, as verificações do aplicativo Astro único serão aprovadas. Isso pode levar alguns minutos.
9. **Opcional:** teste manualmente o recurso de jogos relacionados no codespace. Abra o terminal e execute os comandos a seguir, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot, por exemplo, `copilot/fix-8`:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

O Copilot criou o recurso de jogos relacionados. Como antes, você pode pedir atualizações ao agente de nuvem adicionando um comentário na guia **Conversation**. Por exemplo, solicite que ajuste a quantidade de jogos relacionados ou que inclua cabeçalhos e comentários TSDoc. Lembre-se de que essa tarefa foi atribuída antes de atualizar as instruções personalizadas.

## Revisar os recursos de acessibilidade

Por fim, revise os recursos de acessibilidade implementados pelo agente personalizado. O PR deve incluir o modo de alto contraste solicitado no Exercício 3 e o modo claro pedido no painel de agentes durante o Exercício 4.

1. Acesse o repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título semelhante a **Add high contrast mode to website**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, o PR terá a indicação **[WIP]**. Aguarde alguns minutos até a conclusão.

4. Selecione a guia **Files changed** e revise o código gerado.
5. Examine a implementação, prestando atenção especial a:
   - controles da interface para alternar entre os modos.
   - uso do armazenamento local para manter as preferências.
   - alterações de CSS e estilos para os modos de alto contraste e claro.
   - atributos de Accessible Rich Internet Applications (ARIA), como rótulos e navegação por teclado.
   - código JavaScript/TypeScript que gerencia a alternância de modos.
6. Quando a revisão estiver concluída, volte à guia **Conversation** e role para baixo.
7. Procure um indicador de que há workflows aguardando aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

    ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

9. Os workflows deverão aparecer na seção de verificações do pull request. Se tudo correr bem, as verificações do aplicativo Astro único serão aprovadas. Isso pode levar alguns minutos.
10. **Opcional:** teste manualmente os recursos de acessibilidade no codespace. No terminal, execute os comandos abaixo, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Em seguida, inicie o aplicativo e teste os controles dos modos de alto contraste e claro no navegador. Confirme se funcionam como esperado e se as preferências continuam salvas depois de recarregar a página.

O agente personalizado de acessibilidade ajudou a orientar a implementação segundo boas práticas. Se identificar problemas ou oportunidades de melhoria, mencione **@copilot** em um comentário e peça as alterações.

## Exercício opcional — continuar delegando

O agente de nuvem é útil quando você pode delegar itens reais do backlog e se dedicar a outras tarefas. Para praticar, crie mais algumas issues no repositório e atribua-as ao Copilot. Por exemplo:

- Criar um formulário para pessoas interessadas em apoiar um jogo na página de detalhes.
- Implementar paginação na lista de jogos.
- Adicionar validação de entrada e tratamento de erros aos auxiliares de acesso a dados.

## Resumo

Você concluiu o percurso do agente de nuvem. Ao longo das lições, você:

- **Inspecionou os arquivos de instrução personalizados do repositório** para ver como influenciam o trabalho do agente de nuvem.
- **Atribuiu issues ao agente de nuvem do Copilot** e acompanhou a configuração do ambiente, o planejamento e a execução assíncrona.
- **Criou e usou um agente personalizado** para acessibilidade, adicionando controles para os modos de alto contraste e claro.
- **Usou a página de agentes do Copilot como painel de controle** para monitorar e orientar a sessão de acessibilidade.
- **Revisou e iterou sobre os pull requests do agente de nuvem**, mencionando `@copilot` para solicitar alterações e aprovando workflows.

## Revisão e próximos passos

Você concluiu o percurso do agente de nuvem. Para continuar, os outros percursos complementam o que praticou:

- 🖥️ **[Percurso do VS Code](../../vscode/)** — explore o modo de agente do Copilot Chat e a integração com MCP diretamente no IDE.
- 💻 **[Percurso do CLI](../../cli/)** — entregue duas alterações revisadas pelo terminal com os modos Plan e Autopilot, skills de agente, agente personalizado de QA, MCP do Playwright, Agent Merge e a integração opcional `/delegate` com o agente de nuvem.

No seu repositório, experimente também:

- atribuir ao agente de nuvem uma issue de refatoração ou cobertura de testes.
- criar um agente personalizado para outro domínio.
- configurar `copilot-setup-steps.yml` para outra pilha tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente de nuvem][cloud-agent-best-practices]
- [Configurar o firewall do agente de nuvem do Copilot][agent-firewall]

---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
