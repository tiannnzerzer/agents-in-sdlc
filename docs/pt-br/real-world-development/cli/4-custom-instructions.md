---
title: "Lição 4 - Orientar o Copilot com instruções personalizadas"
description: "Explore as instruções do repositório, adicione um padrão de documentação e aplique-o ao código de filtragem."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisar ser feita de determinada maneira, disponibilize essa orientação ao Copilot. Os [arquivos de instruções][instruction-files] descrevem não apenas *o que* você quer que o código faça, mas também *como* ele deve ser estruturado. Agora que você implementou a filtragem, explore as instruções usadas pelo Copilot, adicione um padrão de documentação e aplique-o ao código.

Nesta lição, você vai:

- explorar como as instruções do repositório e os arquivos de instrução por caminho chegam ao agente.
- atualizar o arquivo de instruções para que os padrões de código sejam seguidos.
- observar o impacto dos arquivos de instrução no código.

## Cenário

A Tailspin Toys segue diretrizes e requisitos para as práticas de desenvolvimento:

- Os comentários devem explicar a intenção e decisões não óbvias, em vez de repetir o código.
- As funções exportadas em `db/` e `src/lib/` devem documentar sua finalidade, parâmetros e valores de retorno com documentação do TypeScript (TSDoc) ou do JavaScript (JSDoc), incluindo um argumento `db` injetável quando houver.
- Os componentes Astro reutilizáveis devem documentar seus contratos `Props`, e os comentários devem continuar atualizados quando o código relacionado mudar.
- As orientações existentes de formatação e lint devem ser preservadas.

Use os arquivos de instrução para fornecer ao Copilot o contexto necessário para seguir essas práticas.

## Arquivos de instrução

As instruções personalizadas fornecem contexto e preferências para que o Copilot entenda melhor o estilo e os requisitos do seu código. Elas permitem orientar as sugestões com convenções, bibliotecas e comentários preferidos. Você pode criar instruções para todo o repositório ou para tipos específicos de arquivo e tarefas.

Há dois tipos de arquivos de instrução:

- `.github/copilot-instructions.md` é um único arquivo enviado ao Copilot em todas as solicitações do repositório. Ele deve conter informações gerais relevantes à maioria das solicitações, como a pilha tecnológica, uma visão geral do projeto, boas práticas e orientações gerais.
- `.github/instructions/*.instructions.md` contém orientações para linguagens, tipos de arquivo ou tarefas específicas.

> [!NOTE]
> O suporte a outros formatos de instrução varia conforme o ambiente. Consulte a [referência de suporte a instruções personalizadas][custom-instructions-support] antes de depender de um formato específico.

## Explorar os arquivos de instrução do projeto

O projeto inicial já inclui arquivos de instrução. Explore o conteúdo existente antes de fazer alterações para perceber o impacto delas.

1. Retorne ao Codespace.
2. No editor do Codespaces, e não no terminal, abra `.github/copilot-instructions.md`.
3. Examine a descrição do projeto e as orientações de código. Elas se aplicam a todas as interações com o Copilot neste repositório.
4. Abra a pasta `.github/instructions` e explore os arquivos. Há instruções para arquivos Astro, a camada de dados Drizzle, testes e outros tópicos.
5. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` no início, que define um glob para determinar a quais arquivos as instruções se aplicam.
6. Abra `.github/instructions/drizzle.instructions.md` e observe os links para outros arquivos de instrução e arquivos existentes do projeto. Assim, conjuntos maiores de instruções podem ser divididos em arquivos menores e reutilizáveis, com exemplos para o Copilot seguir.

## Atualizar as instruções conforme as diretrizes da equipe

Os arquivos existentes são um bom começo, mas falta uma orientação. Altere o arquivo principal `copilot-instructions.md` para garantir que o TypeScript recém-gerado inclua comentários TSDoc.

1. Em `.github/copilot-instructions.md`, localize a seção **Code formatting guidance**, que deve estar perto da linha 35.
2. Adicione este item ao final da lista:

   ```markdown
   - Todo código TypeScript novo deve incluir comentários de documentação TSDoc.
   ```

O arquivo será salvo automaticamente.

## Usar as instruções atualizadas

O Copilot CLI carrega as instruções do repositório quando uma conversa começa. Depois de salvar as instruções atualizadas, saia e retome a conversa de filtragem para que o Copilot carregue as orientações novas sem perder o contexto do recurso.

1. Se a conversa de filtragem ainda estiver aberta, digite `/exit` para fechá-la. O Copilot CLI salva as conversas automaticamente.
2. Na raiz do repositório e na branch `game-filters-cli`, retome a conversa salva:

   ```bash
   copilot --resume
   ```

   Se solicitado, selecione a conversa de filtragem e confirme que a branch selecionada é `game-filters-cli`.
3. Peça ao Copilot que aplique as orientações atualizadas:

   ```plaintext
   Acabamos de atualizar as instruções e as diretrizes de código. Você pode atualizar o código gerado para seguir essas orientações?
   ```

4. Digite `/diff` e leia os arquivos TypeScript alterados. Observe os comentários TSDoc gerados e confirme que descrevem o código corretamente.

## Resumo e próximos passos

Você explorou como o Copilot CLI obtém contexto dos arquivos de instrução e aplicou um novo padrão ao recurso. Você:

- explorou como as instruções do repositório e os arquivos de instrução por caminho chegam ao agente.
- atualizou o arquivo de instruções para garantir que os padrões de código sejam seguidos.
- observou o impacto dos arquivos de instrução no código.

Em seguida, você [personalizará e executará a skill reutilizável `quality-checks`][next-lesson] para executar lint e testes de forma consistente.

## Recursos

- [Adicionar instruções personalizadas para o Copilot CLI][instruction-files]
- [Suporte a instruções personalizadas][custom-instructions-support]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [Awesome Copilot — coleção de arquivos de instrução e outros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
