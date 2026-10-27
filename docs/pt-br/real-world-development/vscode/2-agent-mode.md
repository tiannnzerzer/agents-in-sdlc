---
title: "Exercício 2 - Adicionar funcionalidades com o modo de agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até mesmo uma pequena atualização de aplicativo pode exigir alterações em vários arquivos e operações como executar testes. Em geral, quem desenvolve precisa localizar os arquivos, fazer as alterações, executar os testes, depurar, descobrir o que faltou e atualizar outros arquivos. O processo pode se prolongar.

É aí que entra o modo de agente do Copilot.

O modo de agente foi criado para trabalhar com mais autonomia no IDE. Ele explora a estrutura existente do projeto, faz as atualizações necessárias, executa tarefas como testes e corrige automaticamente os problemas encontrados. Vamos ver como usá-lo para adicionar funcionalidades ao site.

> [!NOTE]
> Embora os nomes sejam semelhantes, o modo de agente e o agente de nuvem atendem a experiências diferentes. O modo de agente trabalha no IDE, com ciclos rápidos de feedback e interação. O agente de nuvem atua de forma assíncrona, como uma pessoa da equipe, e interage por meio de issues e pull requests.

Neste exercício, você verá como:

- o modo de agente do Copilot explora o projeto, identifica os arquivos relevantes e coordena as alterações.
- o modo de agente do GitHub Copilot implementa recursos na interface e na camada de dados.
- revisar alterações e testes gerados pelo modo de agente do Copilot antes de integrá-los à base de código.

## Cenário

À medida que a lista de jogos cresce, você quer permitir a filtragem por categoria e editora. No exercício anterior, você adicionou um auxiliar de editoras. Agora, use o modo de agente do Copilot para concluir o trabalho restante na camada de dados, na interface e nos testes.

## Executar o site Tailspin Toys

Antes de fazer alterações, explore o site para entender as funcionalidades existentes.

O site é uma plataforma de financiamento coletivo para jogos de tabuleiro com temática de desenvolvimento. Ele permite listar jogos e exibir os detalhes deles. Trata-se de um único aplicativo Astro, que renderiza as páginas como linguagem de marcação de hipertexto (HTML) estática durante a compilação. As páginas consultam diretamente um banco de dados SQLite local no frontmatter, usando o mapeador objeto-relacional (ORM) Drizzle. Não há uma interface de programação de aplicações (API) de backend separada nem um framework de interface no cliente. Os auxiliares reutilizáveis de acesso a dados ficam em `src/lib/`, e a interatividade é adicionada com pequenos elementos `<script>` do Astro que usam as APIs padrão do modelo de objetos do documento (DOM).

### Iniciar o site

Uma tarefa `npm run dev` inicia o servidor de desenvolvimento Astro. Execute-a no GitHub Codespaces para abrir o site.

1. Retorne ao codespace. Depois do exercício anterior, você deverá estar na branch `main`.
2. Abra um terminal no Codespace pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie e selecione uma branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicie o site:

   ```bash
   npm run dev
   ```

   Quando o servidor Astro estiver pronto, você verá um aviso com uma URL semelhante a esta:

   ```text
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se uma caixa de diálogo pedir para abrir `http://localhost:4321` em um navegador, feche-a selecionando **x**.

5. Abra o site mantendo pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecionando `http://localhost:4321` no terminal.

> [!NOTE]
> Em um codespace, selecionar o link localhost no terminal redireciona automaticamente para `https://<your-codespace-name>-4321.app.github.dev/`. Esse é um túnel privado para o codespace, que hospeda o servidor Web.

### Explorar o site

Com o site em execução, conheça suas funcionalidades principais:

- **Página inicial**: exibe a lista de jogos de tabuleiro com títulos, imagens e descrições.
- **Página de detalhes do jogo**: ao selecionar um jogo, você verá uma página com mais informações, como título, descrição, editora e categoria.

## Explorar o backlog com o Copilot

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de começar, retorne ao codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam conforme o exercício; as etapas correspondentes indicarão o que selecionar.

A implementação inicial do site funciona, mas queremos adicionar recursos. Consulte o backlog perguntando ao GitHub Copilot sobre as issues criadas automaticamente quando o projeto foi configurado a partir do modelo.

1. No menu de agentes da exibição Chat, selecione **Agent**. O agente explora e implementa alterações em vários arquivos, executa comandos no terminal e invoca ferramentas.

   ![Seletor de agentes na exibição Chat](../../../_images/shared-chat-mode-selector.png)

2. Na lista de modelos disponíveis, selecione **Claude Sonnet 4.5**.

> [!CAUTION]
> A inclusão de um modelo nestas instruções não representa preferência dos autores por um modelo. Usamos Claude Sonnet 4.5 ao criar o workshop para tornar as sugestões relativamente consistentes. Como os modelos de linguagem de grande porte (LLMs) são probabilísticos, os resultados podem ser diferentes dos descritos. Isso é normal e esperado.

> [!NOTE]
> Devido à natureza probabilística dos LLMs, o Copilot pode usar outro comando MCP e ainda concluir a tarefa.

3. Envie este prompt para perguntar sobre o backlog:

   ```plaintext
   Mostre o backlog de itens do meu repositório do GitHub. Ajude a priorizá-los com base no que será mais útil para as pessoas usuárias.
   ```

4. Selecione **Continue** para permitir que o comando liste todas as issues.
5. Revise a lista de issues gerada.

Observe como o Copilot priorizou os itens de acordo com o que considera mais útil para as pessoas usuárias.

## Revisar os arquivos de instrução

Antes de pedir ao agente que gere código, examine o arquivo de instruções que fornecerá contexto. O arquivo [de interface do usuário (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md) explica como abordar a implementação de recursos no site.

1. No codespace, abra `.github/instructions/ui.instructions.md`.
2. Observe as orientações gerais para adicionar funcionalidades:
   - uma visão geral da arquitetura.
   - princípios de design de componentes, testabilidade e acessibilidade.
   - links para instruções específicas sobre Astro, Tailwind CSS e Drizzle.

> [!TIP]
> Os arquivos de instrução podem fazer referência a outros arquivos de instrução e a arquivos do projeto. Os caminhos são relativos ao arquivo de instrução, o que permite reutilizar orientações, dividir conjuntos complexos e fornecer exemplos e modelos.

## Implementar a filtragem

Para concluir a filtragem, serão necessárias pelo menos três alterações no aplicativo:

- adicionar ou aprimorar a lógica de filtragem na camada de dados (`src/lib/`).
- adicionar ou atualizar testes do comportamento de filtragem.
- atualizar a página de listagem de jogos com a interface de filtragem.

Além disso, os testes precisam passar antes do merge. O modo de agente pode executar esse trabalho.

1. Continue a conversa atual com o Copilot ou selecione **New Chat** para iniciar outra.
2. Selecione **Add Context**, **Instructions** e **ui** como arquivo de instruções.

   ![Exemplo de seleção do arquivo de instruções de interface](../../../_images/ex3-select-instructions-file.png)

3. Confirme que **Agent** está selecionado no menu de agentes da exibição Chat.

   ![Seletor de agentes na exibição Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirme que **Claude Sonnet 4.5** continua selecionado como modelo.
5. Envie este prompt para implementar os requisitos da issue relacionada no backlog:

   ```plaintext
   Atualize o site para incluir a filtragem por editora e categoria conforme os requisitos da issue relacionada no backlog do GitHub. O auxiliar de editoras já existe por causa do exercício anterior; preserve e refine esse trabalho conforme necessário ao concluir as alterações restantes na camada de dados, na interface e nos testes. Confirme que todos os testes passam antes de concluir. O servidor já está em execução, portanto não é necessário iniciá-lo.
   ```

6. Observe o Copilot explorar o projeto e localizar os arquivos relacionados à funcionalidade, incluindo auxiliares da camada de dados, interface e testes. Depois, ele modificará os arquivos e executará os testes.

   ![Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> O Copilot executará várias tarefas, como explorar o projeto, modificar arquivos e executar testes. Isso pode levar alguns minutos, dependendo da complexidade da tarefa e da base de código. Durante o processo, os controles **Keep** e **Undo** podem aparecer no editor. Quando o Copilot terminar, você poderá aceitar ou desfazer cada alteração; não é necessário selecionar esses controles enquanto o trabalho está em andamento.

7. Quando solicitado pelo Copilot, selecione **Continue** para executar os testes.

   ![Caixa de diálogo do Copilot Chat pedindo confirmação para executar testes](../../../_images/ex3-agent-mode-run-tests.png)

8. Pode haver pausas e falhas em alguns testes durante o processo. O Copilot alterna entre geração de código e testes até concluir a tarefa sem detectar erros.

   ![Sessão concluída do modo de agente do Copilot](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Examine o código gerado para identificar possíveis problemas.

> [!CAUTION]
> Sempre revise o código gerado pelo Copilot ou por qualquer outra ferramenta de IA.

10. Volte ao navegador em que o site está em execução e explore a nova funcionalidade.
11. Depois de confirmar que tudo funciona e revisar o código, selecione **Keep** na janela Copilot Chat.

## Resumo e próximos passos

Neste exercício, você usou o modo de agente do GitHub Copilot para adicionar recursos ao site da Tailspin Toys. Você explorou como:

- o modo de agente do GitHub Copilot implementa recursos na interface e na camada de dados.
- o modo de agente do Copilot explora o projeto, identifica os arquivos relevantes e coordena alterações.
- revisar alterações e testes gerados pelo modo de agente do Copilot antes de integrá-los à base de código.

Agora, vamos [testar o recurso com o servidor MCP do Playwright][next-lesson] e abrir um pull request.

### Exercício extra de exploração — Implementar paginação

À medida que a lista de jogos crescer, será necessário oferecer paginação. Use o que aprendeu neste exercício e peça ao Copilot que atualize o site. Considere as boas práticas e os arquivos de instrução existentes, decida se as pessoas poderão escolher o tamanho da página ou se ele será fixo e inclua as orientações necessárias no prompt. Talvez seja preciso iterar e fornecer contexto adicional; isso é normal ao trabalhar com o Copilot.

## Recursos

- [Modos ask, edit e agent do Copilot: o que fazem e quando usar cada um][choose-mode]
- [Modo de agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
