---
title: "Exercício 1 - Instruções personalizadas (agente de nuvem)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Agente de nuvem do Copilot →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisar ser feita de uma maneira específica ou houver informações de contexto que o Copilot deva conhecer, disponibilize-as. Os [arquivos de instrução][instruction-files] fornecem essas orientações para que o Copilot entenda não apenas *o que* você quer, mas também *como* o trabalho deve ser feito.

Neste exercício, você vai:

- explorar como o contexto específico do projeto, as diretrizes de código e os padrões de documentação chegam ao Copilot por meio de instruções personalizadas do repositório e arquivos de instrução por caminho.
- adicionar um novo padrão para todo o repositório em `.github/copilot-instructions.md`.

> [!NOTE]
> Diferentemente dos percursos de VS Code e CLI, você não executará um prompt antes e depois da alteração. O agente de nuvem do Copilot trabalha de forma assíncrona em issues do GitHub, então é mais difícil demonstrar o impacto lado a lado. Você verá a influência das instruções ao revisar os pull requests produzidos pelo agente.

## Arquivos de instrução

### Cenário

A Tailspin Toys segue diretrizes e requisitos de desenvolvimento:

- A camada de dados sempre precisa de testes unitários.
- A interface deve usar o modo escuro e ter uma aparência moderna.
- O código deve incluir documentação do TypeScript (TSDoc) na forma de comentários.
- O início de cada arquivo deve conter um bloco de comentários explicando sua finalidade.

Com os arquivos de instrução, você fornecerá ao Copilot o contexto necessário para seguir essas práticas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências para que o Copilot entenda melhor o estilo e os requisitos do seu código. Elas ajudam a orientar sugestões, convenções, bibliotecas e comentários. Você pode criar instruções para todo o repositório ou para tipos específicos de arquivos e tarefas.

Há dois tipos de arquivos de instrução:

- `.github/copilot-instructions.md` é um único arquivo enviado ao Copilot em todas as solicitações do repositório. Ele deve conter informações gerais relevantes à maioria das interações, como a pilha tecnológica, uma visão geral do projeto, boas práticas e outras orientações globais.
- `.github/instructions/*.instructions.md` pode conter diretrizes para tarefas ou tipos de arquivo específicos, como TypeScript, Astro, componentes de interface ou testes unitários.

> [!NOTE]
> No IDE, os arquivos de instrução são usados para gerar código no Copilot Chat, não para sugestões de conclusão de código ou próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente de nuvem do Copilot usam as instruções do repositório e os arquivos `*.instructions.md` com frontmatter `applyTo` ao gerar código.
>
> O Copilot também oferece suporte a arquivos de instrução usando [outros padrões][custom-instructions-support], como AGENTS.md e CLAUDE.md.

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

## Adicionar um novo padrão ao repositório

Esta será a única edição que você fará neste exercício: adicione uma regra geral para documentar o código com comentários TSDoc e incluir um comentário no início de cada arquivo. O agente de nuvem usará essa regra nas issues que receber mais adiante.

Antes de editar, crie uma branch. O agente de nuvem lê as instruções da branch de destino da issue, mas o commit permite que você integre a alteração de forma organizada.

1. No terminal do codespace, crie e selecione uma branch:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

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

## Fazer commit, enviar e integrar a atualização das instruções

O agente de nuvem lê os arquivos de instrução da branch de destino da issue. Ao atribuir uma issue ao Copilot no próximo exercício, ele criará uma branch a partir de `main`. Portanto, integre as instruções a `main` para que o agente possa usá-las.

1. Prepare e faça commit da alteração:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envie a branch:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. No github.com, abra um pull request de `custom-instructions-cloud` para `main` e faça o merge. O agente de nuvem poderá então ler essas instruções nas issues atribuídas no próximo exercício.

> [!TIP]
> Se preferir trabalhar diretamente em `main` durante o workshop, ignore as etapas da branch e faça commit diretamente em `main`. A branch serve para reproduzir o fluxo de um projeto real.

## Resumo e próximos passos

Você explorou como o Copilot obtém contexto dos arquivos de instrução do projeto e adicionou um novo padrão para todo o repositório em `.github/copilot-instructions.md`. Você verá esse padrão nos pull requests gerados pelo agente de nuvem durante o restante do percurso.

Em seguida, você [atribuirá sua primeira issue ao agente de nuvem do Copilot][next-lesson].

## Recursos

- [Arquivos de instrução para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [Cinco dicas para escrever instruções melhores para o Copilot][copilot-instructions-five-tips]
- [Awesome Copilot — coleção de arquivos de instrução e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
