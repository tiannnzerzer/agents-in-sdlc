---
title: "Exercício 1 - Instruções personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Modo de agente →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisar ser feita de determinada maneira ou houver informações de contexto que o Copilot deva conhecer, disponibilize-as. Os [arquivos de instrução][instruction-files] fornecem orientações sobre a geração de código.

Neste exercício, você vai:

- explorar como o contexto do projeto, as diretrizes de código e os padrões de documentação chegam ao Copilot por meio de instruções personalizadas do repositório e arquivos de instrução por caminho.
- gerar a primeira parte da camada de dados para a filtragem — um auxiliar de editoras — usando as instruções atuais.
- adicionar um novo padrão para todo o repositório em `.github/copilot-instructions.md`.
- executar novamente o mesmo prompt e observar o código gerado seguir o novo padrão.
- fazer commit das instruções e da primeira parte da filtragem em `main` para que o Copilot use as orientações atualizadas no próximo exercício.

> [!CAUTION]
> O código gerado pode não seguir todos os padrões definidos. O Copilot é probabilístico. O objetivo é observar a tendência de mudança após a atualização das instruções, não obter resultados idênticos caractere por caractere.

## Arquivos de instrução

### Cenário

A Tailspin Toys segue diretrizes e requisitos de desenvolvimento:

- A camada de dados sempre precisa de testes unitários.
- A interface deve usar o modo escuro e ter uma aparência moderna.
- O código deve incluir documentação do TypeScript (TSDoc) na forma de comentários.
- O início de cada arquivo deve conter um bloco de comentários explicando sua finalidade.

Use os arquivos de instrução para fornecer ao Copilot o contexto necessário para seguir essas práticas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências para que o Copilot entenda melhor o estilo e os requisitos do seu código. Elas ajudam a orientar sugestões, convenções, bibliotecas e comentários. Você pode criar instruções para todo o repositório ou para tipos específicos de arquivos e tarefas.

Há dois tipos de arquivos de instrução:

- `.github/copilot-instructions.md` é um único arquivo enviado ao Copilot em todas as solicitações do repositório. Ele deve conter informações gerais relevantes à maioria das interações, como a pilha tecnológica, uma visão geral do projeto, boas práticas e outras orientações globais.
- `.github/instructions/*.instructions.md` pode conter diretrizes para tarefas ou tipos de arquivo específicos, como TypeScript, Astro, componentes de interface ou testes unitários.

> [!NOTE]
> No ambiente de desenvolvimento integrado (IDE), os arquivos de instrução são usados para gerar código no Copilot Chat, não para sugestões de conclusão de código ou próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente de nuvem do Copilot usam as instruções do repositório e os arquivos `*.instructions.md` com frontmatter `applyTo` ao gerar código.
>
> O Copilot também oferece suporte a arquivos de instrução em outros padrões, como AGENTS.md e CLAUDE.md.

### Boas práticas para gerenciar arquivos de instrução

O projeto de exemplo demonstra uma abordagem representativa para criar arquivos de instrução:

- Mantenha `copilot-instructions.md` focado em orientações gerais do projeto, como o que está sendo desenvolvido, a estrutura do projeto e os padrões globais de código.
- Use `*.instructions.md` para tipos de arquivo, como testes unitários, componentes Astro e camada de dados, ou para tarefas específicas.
- Use linguagem natural e orientações claras. Inclua exemplos de como o código deve ou não deve ser escrito.

Não há uma única maneira correta de criar arquivos de instrução ou de usar IA. A experimentação ajuda a descobrir o que funciona melhor para cada projeto.

> [!TIP]
> Projetos que usam o GitHub Copilot devem ter um conjunto sólido de arquivos de instrução. Ao explorar este projeto, você encontrará arquivos para vários tipos de tarefas, incluindo [atualizações de interface][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar arquivos de instrução. Cada interface oferece essa função de forma diferente, como **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI; a lição correspondente explica quando usá-la.
>
> Para encontrar modelos e exemplos, explore o [awesome-copilot][awesome-copilot], um repositório com arquivos de instrução, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar os arquivos de instrução personalizados deste projeto

Leia os arquivos de instrução incluídos no repositório: há um arquivo principal `copilot-instructions.md` e vários `*.instructions.md` para tarefas diferentes. Abra-os no editor ou na interface Web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Leia a breve descrição do projeto e observe seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe as orientações aninhadas sobre **GitHub Actions Workflows**. Elas se aplicam a todas as interações com o Copilot.
3. Abra a pasta `.github/instructions` e explore seu conteúdo. Observe as instruções para arquivos Astro, a camada de dados Drizzle, testes e outros tópicos.
4. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo`, que define um glob relativo à raiz do repositório para determinar a quais arquivos as instruções se aplicam. Neste exemplo, ele corresponde a arquivos de teste TypeScript, como `**/*.test.ts`.
5. Leia as instruções específicas para criar testes unitários neste projeto.
6. Abra `.github/instructions/drizzle.instructions.md` e examine os links no final para outros arquivos de instrução, como `unit-tests.instructions.md`, e para arquivos existentes no projeto. Os caminhos são relativos ao arquivo de instrução, não à raiz do repositório.

> [!NOTE]
> A seção **Code formatting requirements** em `copilot-instructions.md` descreve os padrões de código do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de arquivo.

## Criar uma branch para as alterações

Crie uma branch para trabalhar com boas práticas de controle de versão.

1. Retorne ao codespace usado no exercício anterior.
2. Abra um terminal pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie e selecione uma branch:

   ```bash
   git checkout -b custom-instructions
   ```

## Usar o Copilot Chat antes de atualizar as instruções

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de começar, retorne ao codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam conforme o exercício; as etapas correspondentes indicarão o que selecionar.

Para observar o impacto das instruções personalizadas, envie primeiro um prompt com o arquivo de instruções atual. Depois, atualize-o e envie o mesmo prompt novamente para comparar o resultado.

1. Feche as guias do editor abertas nos exercícios anteriores para limitar o contexto disponível ao Copilot.
2. Abra `src/lib/publishers.ts` para indicar onde o auxiliar deve ficar.
3. No menu de agentes da exibição Chat, selecione **Agent** para permitir que o Copilot altere arquivos.

   ![Seletor de agentes na exibição Chat](../../../_images/shared-chat-mode-selector.png)

4. Envie este prompt:

   ```plaintext
   Crie ou atualize src/lib/publishers.ts com um auxiliar de acesso a dados que retorne uma lista de todas as editoras, com o nome e o id de cada uma. Aplique as alterações nos arquivos.
   ```

5. O Copilot explora o projeto e aplica as alterações, que podem incluir o arquivo auxiliar e os testes.
6. Observe que o auxiliar proposto é uma função tipada que recebe um cliente `db` como primeiro argumento e retorna uma matriz tipada de editoras. Isso segue as convenções da camada de dados definidas em `.github/instructions/drizzle.instructions.md`, aplicáveis a `src/lib/*.ts`.
7. Observe que o código proposto **não inclui** comentários TSDoc nem um cabeçalho no início do arquivo.

> [!CAUTION]
> Como o Copilot é probabilístico, talvez ele adicione comentários mesmo sem essa orientação. Tudo bem: o objetivo é observar a melhora da consistência depois da atualização das instruções.

## Adicionar um novo padrão ao repositório

O arquivo `.github/copilot-instructions.md` fornece informações gerais do projeto ao Copilot. Atualize os padrões do repositório para orientar melhor as sugestões de código.

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements**, que deve estar perto da linha 27. Observe que ainda não há uma regra para a documentação no código.
3. Logo abaixo dos padrões existentes, adicione estas linhas para pedir cabeçalhos de arquivo e comentários TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Como visto na lição anterior, as instruções podem ser definidas no repositório (`.github/copilot-instructions.md`) para orientações gerais ou em arquivos `*.instructions.md` para linguagens, tipos de arquivo ou tarefas específicas. O arquivo do repositório é adequado para padrões gerais, como a regra de comentários de documentação que você acabou de adicionar.

## Executar novamente o prompt e observar a diferença

Envie o mesmo prompt em uma conversa nova para que o Copilot carregue as instruções atualizadas.

1. Volte ao Copilot Chat e selecione **New Chat** para limpar o contexto.
2. Selecione o arquivo `src/lib/publishers.ts` para que o Copilot se concentre nele.
3. Envie novamente o mesmo prompt:

   ```plaintext
   Crie ou atualize src/lib/publishers.ts com um auxiliar de acesso a dados que retorne uma lista de todas as editoras, com o nome e o id de cada uma. Aplique as alterações nos arquivos.
   ```

4. Observe que o arquivo proposto agora começa com um bloco de comentários semelhante a este:

   ```typescript
   /**
    * Auxiliares de acesso aos dados de editoras da plataforma de financiamento coletivo da Tailspin Toys.
    * Inclui funções para consultar informações de editoras no banco de dados.
    */
   ```

5. Observe que a função proposta agora inclui um comentário TSDoc semelhante a este:

   ```typescript
   /**
    * Retorna uma lista de todas as editoras com seus ids e nomes.
    *
    * @param db - Cliente do banco de dados Drizzle.
    * @returns Uma promessa que resolve para uma matriz de objetos de editoras.
    */
   ```

Você orientou o Copilot a seguir um novo padrão e aplicá-lo ao código que servirá de base para o próximo exercício.

## Fazer commit das instruções e enviar a branch

Os arquivos de instrução são controlados pelo Git como os outros arquivos do repositório. Faça commit das alterações e envie a branch.

1. Abra um terminal pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. No terminal, confirme que as alterações das instruções e do auxiliar estão presentes:

   ```bash
   git status
   ```

3. Prepare e faça commit das instruções e de todos os arquivos alterados pelo Copilot para o auxiliar:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Se git status mostrar outras alterações relacionadas, como testes, adicione esses arquivos também.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envie a branch ao repositório:

   ```bash
   git push -u origin custom-instructions
   ```

## Criar e integrar um pull request

Depois de enviar a branch, crie um pull request relacionado à issue sobre os padrões de documentação do backlog. O Copilot pode encontrar a issue, criar um PR que a encerre e fazer o merge.

> [!NOTE]
> Este é o primeiro exercício em que o Copilot executa ações no GitHub. O modelo do projeto já configura as ferramentas do GitHub no workspace, então não é necessário configurá-las. Na primeira vez que uma ferramenta for usada, o VS Code talvez peça para você entrar no GitHub; siga as instruções. Você aprenderá como essa conexão funciona — por meio do Model Context Protocol (MCP) — em um exercício posterior.

1. Abra o Copilot Chat no codespace.
2. Pressione <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir a exibição Copilot Chat e confirme que **Agent** está selecionado no menu de agentes.
3. Peça ao Copilot que localize a issue sobre as instruções e crie um PR a partir da branch atual, descrevendo as atualizações e a base do auxiliar de editoras:

   ```plaintext
   Encontre a issue relacionada à atualização do arquivo de instruções. Crie um PR a partir da branch atual, indique que o PR encerra essa issue e mencione também que adicionamos a base do auxiliar de editoras para o próximo trabalho de filtragem.
   ```

4. O Copilot localizará a issue e criará o PR.
5. Quando o Copilot solicitar permissão para executar ações no GitHub, revise o comando e selecione **Allow** se estiver de acordo.
6. Depois que o PR for criado, peça ao Copilot que o integre e volte à branch `main`:

   ```plaintext
   Faça o merge do PR em main. Em seguida, volte localmente para main e baixe o código mais recente.
   ```

7. Quando o Copilot solicitar permissão para executar ações no GitHub ou comandos do shell, revise o comando e selecione **Allow** se estiver de acordo.

Você criou e integrou um pull request com a ajuda do GitHub Copilot.

## Resumo e próximos passos

Você explorou como o Copilot obtém contexto dos arquivos de instrução e usou o Copilot Chat no VS Code para:

- enviar um prompt de geração de código e observar o resultado com as instruções existentes.
- adicionar um novo padrão para todo o repositório em `.github/copilot-instructions.md`.
- enviar novamente o mesmo prompt e observar o código proposto seguir o novo padrão.
- fazer commit das instruções e da base do auxiliar em `main` para que o exercício seguinte possa usá-las.

Em seguida, você usará essas instruções no [modo de agente][next-lesson] para adicionar um recurso à base de código.

## Recursos

- [Arquivos de instrução para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [Cinco dicas para escrever instruções melhores para o Copilot][copilot-instructions-five-tips]
- [Instruções pessoais personalizadas para o GitHub Copilot][personal-instructions]
- [Awesome Copilot — coleção de arquivos de instrução e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
