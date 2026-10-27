---
title: "Lição 6 - Validar a funcionalidade com o MCP do Playwright"
description: "Inspecione ou configure o MCP do Playwright no Copilot CLI e use-o para explorar o recurso de filtragem no navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como já vimos, desenvolver não é apenas escrever código. Também é preciso trabalhar com dados, serviços externos e automações adicionais para o Copilot. É aí que entram os servidores MCP: eles ampliam as ferramentas e os serviços disponíveis no Copilot CLI.

Nesta lição, você vai:

- entender o que é o Model Context Protocol (MCP) e como o Copilot CLI o utiliza.
- adicionar o servidor MCP do Playwright, se ele ainda não estiver disponível.
- pedir ao agente que controle um navegador e explore o recurso de filtragem.

## Cenário

Testes unitários e de ponta a ponta são importantes, mas validar alterações na interface exige interagir com ela. Você quer que o Copilot use o site como uma pessoa usuária, automatizando a verificação das alterações e aumentando a confiança de que elas funcionam como esperado.

## O que é o Model Context Protocol (MCP)?

O [Model Context Protocol (MCP)][mcp-blog-post] permite que agentes de IA se comuniquem com ferramentas e serviços externos. Com MCP, os agentes podem acessar informações atualizadas e executar ações em seu nome.

As ferramentas e os recursos são acessados por um servidor MCP, que atua como ponte entre o agente de IA e os serviços externos. Cada servidor MCP oferece um conjunto diferente de ferramentas e recursos.

Alguns servidores MCP conhecidos:

- **[Servidor MCP do GitHub][github-mcp]**: oferece acesso a APIs para gerenciar repositórios, issues e pull requests do GitHub.
- **[Servidor MCP do Playwright][playwright-mcp-server]**: oferece recursos de automação de navegador com Playwright.

Há muitos outros servidores MCP. O GitHub mantém um [registro MCP][mcp-registry] para facilitar a descoberta de servidores e as contribuições ao ecossistema.

> [!CAUTION]
> Trate os servidores MCP como qualquer outra dependência do projeto. Antes de usar um servidor, revise o código-fonte, verifique o publicador e considere as implicações de segurança.

## Adicionar o servidor MCP do Playwright

Se o servidor MCP do Playwright ainda não estiver disponível, adicione-o para que o Copilot possa interagir com o site como uma pessoa usuária.

> [!NOTE]
> Se o MCP do Playwright já estiver configurado no Copilot CLI, ignore estas etapas e continue na próxima seção.

1. Retorne ao Codespace.
2. No Copilot CLI, abra a caixa de diálogo para adicionar um servidor MCP com este comando:

   ```plaintext
   /mcp add
   ```

3. Digite `playwright` como nome e pressione <kbd>Tab</kbd>.
4. Confirme o tipo de servidor **STDIO** pressionando <kbd>Enter</kbd> e, em seguida, pressione <kbd>Tab</kbd>.
5. Cole o seguinte na caixa de diálogo **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pressione <kbd>Control</kbd>+<kbd>S</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para salvar o novo servidor MCP.
7. Pressione <kbd>Esc</kbd> para sair da caixa de diálogo do MCP.

## Pedir ao Copilot para explorar o recurso com Playwright

Você já confirmou manualmente que o recurso funciona. Agora peça ao Copilot para fazer o mesmo com o servidor MCP do Playwright.

1. Envie este prompt para pedir ao Copilot que valide a funcionalidade com o servidor MCP do Playwright:

   ```plaintext
   Inicie o aplicativo e use o MCP do Playwright para verificar a filtragem conforme a issue e nosso plano. Informe o que funciona e o que não funciona, sem fazer alterações. Quando terminar, pare o servidor que iniciou.
   ```

> [!NOTE]
> Não é necessário mencionar o servidor MCP: normalmente, o Copilot identifica qual deve usar. Ainda assim, indicar a ferramenta pode ajudar a obter resultados mais consistentes.

2. Acompanhe as etapas que o Copilot executa no navegador para verificar a funcionalidade.
3. Leia o relatório e confirme se tudo funciona como esperado.

O Copilot iniciará o servidor, usará o Playwright para interagir com o site, encerrará o servidor e apresentará um relatório.

## Resumo e próximos passos

Você usou o servidor MCP do Playwright para explorar o recurso em um navegador real a partir do Copilot CLI. Você:

- entendeu o que é o Model Context Protocol (MCP) e como o Copilot CLI o utiliza.
- adicionou o servidor MCP do Playwright, caso ainda não estivesse disponível.
- pediu ao agente que controlasse um navegador e explorasse o recurso de filtragem.

Em seguida, você [criará um agente personalizado de QA][next-lesson] que combinará a skill e as ferramentas de navegador em uma função especializada.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre isso?][mcp-blog-post]
- [Servidor MCP do Microsoft Playwright][playwright-mcp-server]
- [Adicionar servidores MCP ao Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
