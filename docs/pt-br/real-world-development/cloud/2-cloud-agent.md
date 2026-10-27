---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

Poucas organizações deixam de enfrentar dívida técnica. Ela pode incluir problemas de segurança não resolvidos, código legado que precisa de atualização ou solicitações de recursos que ficaram no backlog por falta de tempo. O agente de nuvem do GitHub Copilot pode atualizar código e adicionar funcionalidades de forma autônoma. Ao concluir o trabalho, ele cria um pull request (PR) de rascunho para revisão por uma pessoa desenvolvedora. Assim, é possível delegar tarefas repetitivas e concentrar-se em trabalhos de maior impacto.

Neste exercício, você explorará como:

- personalizar o ambiente de geração de código.
- garantir que as operações sejam executadas com segurança.
- definir issues com escopo claro.
- atribuir issues ao Copilot.

## Cenários

A Tailspin Toys quer resolver parte da dívida técnica. A documentação deixada pelas pessoas contratadas para criar a primeira versão do site está em estado inadequado — na verdade, ela está completamente ausente. Como primeiro passo, a equipe quer adicionar comentários de documentação TypeScript (TSDoc) às funções exportadas do aplicativo.

Além disso, a equipe de design quer facilitar a descoberta de jogos. A página de detalhes de cada jogo deve mostrar jogos relacionados — outros títulos da mesma categoria — para que as pessoas continuem explorando o catálogo. A equipe ainda não precisa de uma implementação refinada; quer algo que possa usar para testes de aceitação da experiência. Essa tarefa é um bloqueio, mas outras issues têm prioridade maior no momento.

Esses dois exemplos podem ser facilmente adiados e são bons candidatos para o agente de nuvem do Copilot. Enquanto ele trabalha de forma assíncrona, você pode se dedicar a outras tarefas e depois revisar o resultado.

## Apresentação do agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) executa tarefas em segundo plano de forma semelhante a uma pessoa desenvolvedora. Uma das formas de usá-lo é [atribuir uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois da atribuição, o Copilot cria um pull request de rascunho para acompanhar o progresso, configura um ambiente e começa a trabalhar. Você pode examinar a sessão durante a execução ou depois de concluída. Quando a solução estiver pronta para revisão, o Copilot mencionará você no pull request.

## A importância de instruções com escopo claro

O GitHub Copilot não faz mágica: ainda é preciso pensar. Mesmo tarefas aparentemente simples podem ter complexidades escondidas. Por isso, [defina com cuidado o escopo das tarefas para o agente de nuvem][cloud-agent-best-practices]. Trate-o como uma pessoa parceira de programação com IA: trabalhe por etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software não mudam com a inclusão da IA generativa.

## Instruções personalizadas neste repositório

Os exercícios anteriores apresentaram instruções personalizadas e como elas orientam o Copilot. Se você já concluiu o exercício prático sobre instruções, viu `.github/copilot-instructions.md` em ação; caso contrário, este é um bom momento para examiná-lo. Antes de atribuir trabalho ao agente de nuvem, leia os arquivos de instrução existentes, sem alterá-los, para depois identificar seus efeitos.

Abra estes arquivos na interface Web do GitHub ou em um codespace que já esteja em execução:

- `.github/copilot-instructions.md` — revise a seção **Code standards**, principalmente as convenções de TypeScript e os comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md` — observe o frontmatter `applyTo`, que aplica as instruções a arquivos `**/*.test.ts`.

Ao atribuir a issue `Code lacks documentation` ao agente de nuvem na próxima seção, observe no pull request os comentários TSDoc, as convenções de TypeScript e os cabeçalhos de arquivo. Essas orientações vêm dos arquivos de instrução e serão mencionadas novamente ao revisar PRs em um exercício posterior.

## Configurar o ambiente de desenvolvimento do agente de nuvem

A criação de código geralmente exige um ambiente específico e scripts de configuração que garantam que tudo esteja pronto. Isso também se aplica às tarefas atribuídas ao Copilot, que trabalha de maneira semelhante a uma pessoa engenheira de software.

O agente de nuvem usa o [GitHub Actions][github-actions] como ambiente de trabalho. Você pode personalizá-lo com um [workflow de configuração especial][setup-workflow] em `.github/workflows/copilot-setup-steps.yml`, executado antes do início da tarefa. Esse workflow fornece as ferramentas e dependências necessárias. Ele já está configurado no laboratório: prepara o Node.js, as dependências do projeto, os navegadores para testes de ponta a ponta e o banco de dados SQLite migrado e preenchido com dados para o aplicativo Astro único:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões no nível mínimo necessário para as etapas. O Copilot recebe um token próprio para suas operações.
    permissions:
      # Para clonar o repositório durante as etapas de configuração, por exemplo, para instalar dependências, será necessária a permissão `contents: read`. Se o repositório não for clonado nessas etapas, o Copilot fará isso automaticamente depois que elas forem concluídas.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do frontend e aplicativo: Node.js (o aplicativo agora é Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra e preenche o banco de dados SQLite local para que compilações e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

O arquivo se parece com outros workflows do GitHub, mas tem alguns pontos importantes:

- Há um único trabalho chamado `copilot-setup-steps`. Ele é executado no GitHub Actions antes de o Copilot começar a trabalhar no pull request.
- O gatilho `workflow_dispatch` permite executar o workflow manualmente na guia **Actions** do repositório. Isso é útil para verificar se ele funciona antes de aguardar uma execução do Copilot.

## Adicionar documentação

Muitos projetos têm documentação desatualizada ou não têm documentação. Essa dívida técnica reduz a produtividade e dificulta a manutenção do código e a integração de novas pessoas à equipe. O Copilot pode ajudar a criar documentação, por isso esta é uma boa issue para atribuir ao agente de nuvem. Em um exercício futuro, você voltará para revisar o trabalho.

1. Em uma nova guia do navegador, acesse seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de criação de issue.
4. Selecione **Blank issue**.
5. No campo **Title**, digite `Code lacks documentation`.
6. No campo **Description**, digite:

    ```plaintext
    Nossa organização exige que funções e métodos incluam comentários de documentação TSDoc quando forem úteis. Infelizmente, as atualizações recentes não seguiram esse padrão. Precisamos atualizar o código existente para incluir comentários de documentação onde eles esclareçam o comportamento.
    ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de atribuição.

    ![Atribuir uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.
10. Selecione a guia **Pull Requests**.
11. Abra o pull request recém-criado, com um título semelhante a `[WIP]: Code lacks documentation`. Se ele ainda não aparecer na lista, aguarde um ou dois minutos e atualize a página.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas pode levar alguns minutos para aparecer no PR. O Copilot está configurando o ambiente com o workflow descrito acima, analisando o projeto e definindo uma abordagem.

13. Revise a lista e as tarefas que serão executadas.
14. Role a linha do tempo do pull request e procure a atualização informando que o Copilot começou a trabalhar na issue.
15. Selecione o botão **View session**.

    ![Exibição da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a página para ver o indicador atualizado.

16. Acompanhe a sessão em andamento e observe como o Copilot explora o código, entende o estado do projeto, planeja a abordagem e cria código.

Essa tarefa pode levar vários minutos. Um dos objetivos do agente de nuvem é executar tarefas de forma assíncrona para que você possa se dedicar a outros trabalhos. Aproveite esse recurso atribuindo outra tarefa ao agente e, em seguida, volte a atenção para outras atividades.

## Adicionar uma seção de jogos relacionados à página de detalhes

Uma das vantagens do agente de nuvem é dividir o trabalho: enquanto você se concentra em uma tarefa, o agente cuida de outra. Vamos atribuir a ele a criação da seção de jogos relacionados pedida pela equipe de design.

1. Volte ao repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue**.
4. Selecione **Blank issue**.
5. No campo **Title**, digite `Show related games on the game details page`.
6. No campo **Description**, digite:

    ```markdown
    Queremos ajudar as pessoas a descobrir mais jogos, exibindo jogos relacionados na página de detalhes de cada jogo. A equipe de design quer explorar a experiência e fazer testes de aceitação. Os requisitos são:

   - Adicionar um auxiliar de acesso a dados em `src/lib/` que retorne outros jogos da mesma categoria que um jogo específico, excluindo o próprio jogo
   - Exibir uma seção "Jogos relacionados" na página de detalhes do jogo usando o auxiliar
   - Tratar o caso em que um jogo não tem jogos relacionados
   - Criar testes unitários para o novo auxiliar
   - Antes de criar o PR, confirmar que todos os testes passam
    ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de atribuição.

    ![Atribuir uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Pouco depois, você verá o emoji 👀 no primeiro comentário da issue, indicando que o Copilot começou a trabalhar.

![O Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

O Copilot está trabalhando na segunda solicitação. Como o agente de nuvem trabalha de maneira semelhante a uma pessoa engenheira de software, não é necessário monitorá-lo continuamente; revise o resultado quando estiver pronto. Agora, vamos criar e usar agentes personalizados.

## Resumo e próximos passos

Neste exercício, você explorou o [agente de nuvem do GitHub Copilot][copilot-agents]. Com ele, é possível atribuir issues ao Copilot para execução assíncrona e delegar tarefas como resolver dívida técnica, criar recursos ou ajudar a migrar código entre frameworks.

Você explorou como:

- personalizar o ambiente de geração de código.
- garantir que as operações sejam executadas com segurança.
- definir issues com escopo claro.
- atribuir issues ao Copilot.

Enquanto o agente de nuvem trabalha, você pode se concentrar em criar e usar agentes personalizados. O [agente de nuvem do Copilot também pode usar servidores do Model Context Protocol (MCP)][cloud-agent-mcp] e instruções personalizadas.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente de nuvem][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
