---
title: "Exercício 3 - Testar seu recurso com o servidor MCP do Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Modo de agente][previous-lesson] · [Próxima lição: Agentes personalizados →][next-lesson]

Você acabou de criar o recurso de filtragem com o modo de agente. Antes de abrir um pull request, confirme que ele funciona no navegador. Em vez de navegar pelo aplicativo manualmente, conecte o **servidor MCP do Playwright** e deixe o Copilot controlar um navegador real para testar o recurso. Depois, publique sua branch e abra o PR.

Neste exercício, você vai:

- aprender o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam as ferramentas do Copilot.
- adicionar o servidor MCP do Playwright ao workspace.
- pedir ao Copilot que use o servidor para testar manualmente a filtragem no navegador.
- publicar a branch e abrir um pull request para o trabalho de filtragem.

## O que é o Model Context Protocol (MCP)?

O modo de agente se torna mais poderoso quando pode acessar recursos além do editor. O Model Context Protocol (MCP) é o padrão que permite ao Copilot se comunicar com ferramentas e serviços externos.

![Diagrama da arquitetura do modo de agente e da interação com contexto, modelo de linguagem de grande porte (LLM) e ferramentas, incluindo ferramentas fornecidas por servidores MCP e extensões do VS Code](../../../_images/ex1-mcp-diagram.png)

O [Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que agentes de IA se comuniquem com ferramentas e serviços externos em tempo real. Assim, eles podem acessar informações atualizadas por meio de recursos e executar ações em seu nome usando ferramentas.

O acesso a ferramentas e recursos é feito por meio de um servidor MCP, que atua como ponte entre o agente de IA e os serviços externos. O servidor gerencia a comunicação com esses serviços, como APIs existentes ou ferramentas locais, incluindo pacotes npm. Cada servidor MCP oferece um conjunto diferente de ferramentas e recursos.

Alguns servidores MCP conhecidos:

- **[Servidor MCP do GitHub](https://github.com/github/github-mcp-server)**: oferece acesso a interfaces de programação de aplicações (APIs) para gerenciar repositórios do GitHub, criar repositórios e administrar issues e pull requests.
- **[Servidor MCP do Playwright](https://github.com/microsoft/playwright-mcp)**: oferece automação de navegador com Playwright, incluindo navegação por páginas e interação com formulários e botões.

Há muitos outros servidores MCP. O GitHub mantém um [registro MCP](https://github.com/mcp) para facilitar a descoberta de servidores e as contribuições ao ecossistema.

> [!CAUTION]
> Trate os servidores MCP como qualquer outra dependência. Antes de usar um servidor, revise o código-fonte, verifique o publicador e considere as implicações de segurança. Use apenas servidores confiáveis e tenha cuidado ao conceder acesso a recursos ou operações confidenciais.

## Revisar a configuração do MCP

O arquivo `.vscode/mcp.json` configura os servidores MCP disponíveis neste workspace do VS Code.

1. Abra `.vscode/mcp.json` no codespace.
2. Observe que o servidor `github` já está configurado:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

A entrada `github` vem do modelo do projeto. Por isso, o Copilot conseguiu ler o backlog de issues no exercício anterior. Ela usa o [servidor MCP remoto do GitHub][remote-github-mcp-server], então não é necessário instalar nada localmente: o VS Code se conecta pelo protocolo de transferência de hipertexto (HTTP), e você autentica na primeira vez que o Copilot usa uma das ferramentas.

## Adicionar o servidor MCP do Playwright

Agora, adicione um segundo servidor. O [servidor MCP do Playwright][playwright-mcp-server] fornece ao Copilot um navegador que ele pode controlar para testar o recurso.

1. No arquivo `.vscode/mcp.json`, adicione uma entrada `playwright` ao lado de `github`, para que o arquivo fique assim:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. Salve o arquivo.

Diferentemente do servidor remoto `github`, `playwright` é um servidor **local**: o VS Code o inicia no computador com `npx @playwright/mcp@latest`. O sinalizador `--headless` executa o navegador sem uma janela visível, o que é necessário em um codespace sem área de trabalho gráfica.

> [!NOTE]
> O projeto Tailspin Toys já usa o Playwright para testes de ponta a ponta, então o navegador necessário geralmente já está instalado. Se o Copilot informar que não há um navegador, peça que execute `npx playwright install chromium` e tente novamente.

## Iniciar e confiar no servidor

O VS Code inicia servidores MCP **sob demanda**: na primeira vez que o Copilot precisa das ferramentas de um servidor, o VS Code o inicia e pede para confirmar que você confia nele.

1. Em `.vscode/mcp.json`, aparecerá a ação **Start** acima da entrada `playwright`. Selecione-a para iniciar o servidor e confirmar que a configuração está correta.
2. Se o VS Code perguntar se você confia no servidor, revise a configuração e confirme para permitir a inicialização.

Iniciar o servidor manualmente é opcional. Se você não fizer isso, o VS Code o iniciará e pedirá confirmação quando o Copilot precisar das ferramentas na próxima etapa.

## Testar o recurso de filtragem

O [servidor MCP do Playwright][playwright-mcp-server] oferece ao Copilot um navegador real. Em vez de navegar pelo aplicativo para verificar o trabalho, o agente pode abrir uma página, aplicar filtros, ler os resultados e resumir o que observou. Assim, você verifica rapidamente o comportamento do recurso sem sair da conversa.

O servidor MCP do Playwright usa a [árvore de acessibilidade][playwright-mcp-server] da página, não capturas de tela. O agente trabalha com elementos estruturados e identificados, como botões, links e itens de lista, da mesma forma que a tecnologia assistiva. Portanto, uma verificação funcional rápida também oferece uma avaliação básica de acessibilidade.

Com o servidor conectado e o aplicativo em execução, peça ao Copilot para testar a filtragem:

```text
Use o servidor MCP do Playwright para abrir um navegador no aplicativo em execução em http://localhost:4321 e verificar o novo recurso de filtragem de jogos:

1. Acesse a página de jogos e informe quantos jogos estão listados.
2. Aplique um filtro de categoria e confirme que a lista mostra apenas os jogos dessa categoria.
3. Limpe o filtro e aplique um filtro de editora. Confirme que a lista mostra apenas os jogos dessa editora.
4. Combine um filtro de categoria e um de editora e confirme que os resultados correspondem aos dois filtros.

Relate o que observou em cada etapa e indique qualquer comportamento inesperado.
```

O Copilot abrirá um navegador pelo servidor MCP do Playwright, seguirá as etapas e apresentará os resultados. Compare o resumo com os critérios de aceitação da issue. Se algo não estiver correto, faça perguntas adicionais ou peça que o Copilot corrija o código antes de abrir o pull request.

> [!NOTE]
> O aplicativo precisa estar em execução em `http://localhost:4321` para esse teste. Se o servidor de desenvolvimento tiver sido parado, inicie-o antes de enviar o prompt. Na primeira vez que o Copilot usar o servidor MCP do Playwright, talvez seja necessário baixar um navegador. Se ele informar que falta um navegador, peça que execute `npx playwright install chromium` e tente novamente.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publicar a branch e criar um pull request

Depois de confirmar que o recurso funciona, abra um pull request (PR) para que a equipe possa revisá-lo. Primeiro, publique a branch `filtering-vscode`.

1. No codespace, abra o painel **Source Control** e revise as alterações feitas pelo Copilot.
2. Prepare as alterações selecionando o ícone **+**.
3. Gere uma mensagem de commit usando o botão **Sparkle**.

   ![Painel Source Control com as alterações](../../../_images/ex3-source-control-changes.png)

4. Selecione **Publish** para enviar a branch ao repositório.

## Criar o pull request

Há várias maneiras de criar um pull request, inclusive no github.com e pela interface de linha de comando do GitHub (CLI). Como você já está usando o GitHub Copilot, peça que ele crie o PR e associe-o à issue correspondente.

1. Abra o painel Copilot Chat e selecione **New Chat** para iniciar uma sessão.
2. No menu de agentes, confirme que **Agent** está selecionado para permitir o uso das ferramentas do GitHub.
3. Peça ao Copilot para criar um PR:

   ```plaintext
   Encontre no repositório a issue relacionada à filtragem por categoria e editora. Crie um pull request para a branch atual e associe-o à issue correta.
   ```

4. Se necessário, selecione **Continue** para permitir que o Copilot reúna informações e execute as operações. Na primeira vez que uma ferramenta do GitHub for usada, talvez seja necessário autenticar sua conta; siga as instruções.
5. Observe o Copilot pesquisar as issues, localizar a correta e criar o PR.
6. Selecione o link gerado pelo Copilot para revisar o pull request, mas **não faça o merge ainda**.

## Resumo e próximos passos

Neste exercício, você:

- aprendeu o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam as ferramentas do Copilot.
- adicionou o servidor MCP do Playwright ao workspace.
- usou o servidor para testar manualmente o recurso de filtragem no navegador antes de publicá-lo.
- publicou a branch e abriu um pull request para o trabalho de filtragem.

Você usou um servidor MCP para *testar* um recurso, mas o MCP é apenas uma das maneiras de ampliar o Copilot. Em seguida, vamos [criar um agente personalizado][next-lesson] para otimizar tarefas específicas, como revisões de acessibilidade.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre isso?][mcp-blog-post]
- [Servidor MCP do Microsoft Playwright][playwright-mcp-server]
- [Servidor MCP do GitHub][github-mcp-server]
- [Registro MCP do GitHub][mcp-registry]
- [Servidores MCP no VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
