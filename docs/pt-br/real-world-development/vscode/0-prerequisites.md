---
title: "Exercício 0: Pré-requisitos"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Antes de começar os exercícios do VS Code, prepare tudo o que será necessário. Você criará uma cópia própria do repositório Tailspin Toys, iniciará um [codespace][codespaces] e confirmará que o GitHub Copilot Chat está funcionando no editor.

## Configurar o repositório do laboratório

Para criar uma cópia do repositório onde trabalhará, crie uma instância a partir do [modelo][template-repository]. A nova instância conterá todos os arquivos necessários para o laboratório.

1. Em uma nova janela do navegador, acesse o [modelo da Tailspin Toys][tailspin-template].
2. Crie uma cópia do repositório selecionando **Use this template** na página do modelo e, em seguida, **Create a new repository**.

    ![Botão Use this template](../../../_images/ex0-use-template.png)

3. Se você estiver participando de um evento conduzido pelo GitHub ou pela Microsoft, siga as instruções das pessoas mentoras. Caso contrário, crie o repositório em uma organização onde você tenha acesso ao GitHub Copilot.

    ![Configurações do modelo do repositório](../../../_images/ex0-repository-settings.png)

4. Anote o caminho do repositório criado (**organization-or-user-name/repository-name**), pois você precisará dele mais adiante.

> [!NOTE]
> **O backlog está pronto**
>
> Ao criar o repositório a partir do modelo, um backlog de issues do GitHub será criado automaticamente. Você trabalhará com essas issues durante o workshop; não é necessário criar outras.

## Criar um codespace

Em seguida, você usará um codespace para concluir os exercícios.

[GitHub Codespaces][codespaces] é um ambiente de desenvolvimento na nuvem que permite escrever, executar e depurar código diretamente no navegador. Ele oferece um ambiente de desenvolvimento integrado (IDE) com suporte a várias linguagens, extensões e ferramentas.

1. Acesse o repositório que acabou de criar.
2. Selecione o botão verde **Code**.

    ![Botão Code](../../../_images/ex0-code-button.png)

3. Selecione a guia **Codespaces** e, em seguida, o botão **+** para criar um codespace.

    ![Criar um codespace](../../../_images/ex0-create-codespace.png)

A criação do codespace pode levar alguns minutos, mas ainda é mais rápida do que instalar todos os serviços manualmente. Enquanto aguarda, explore outros recursos do GitHub Copilot que serão apresentados a seguir.

> [!CAUTION]
> Você voltará ao codespace em um exercício futuro. Por enquanto, mantenha-o aberto em uma guia do navegador.

> [!NOTE]
> Este workshop foi criado para ser executado em um codespace ou em um [contêiner de desenvolvimento][dev-containers] local. Ambos oferecem os pré-requisitos para uma experiência tranquila. Para trabalhar localmente, abra o repositório clonado no VS Code e selecione **Reopen in Container** quando solicitado. O VS Code criará o mesmo contêiner de desenvolvimento usado pelo codespace.

## Usar o GitHub Copilot Chat e o modo de agente

Para acessar o modo de agente do GitHub Copilot Chat, é necessário instalar a extensão GitHub Copilot Chat no IDE. Ela já deve estar disponível se você estiver usando um GitHub Codespace.

> [!TIP]
> Se a extensão GitHub Copilot Chat não estiver instalada, [instale-a pelo Marketplace do Visual Studio Code][copilot-chat-extension]. Outra opção é abrir a exibição Extensions no Visual Studio Code, pesquisar **GitHub Copilot Chat** e selecionar **Install**.

Depois de instalar a extensão, talvez seja necessário autenticar sua conta do GitHub para ativá-la.

1. Retorne ao codespace.
2. Se o Copilot Chat ainda não aparecer à direita do editor, selecione o ícone **Copilot Chat** na parte superior do codespace.
3. Digite uma mensagem, como `Hello world`, na janela do Copilot Chat e pressione <kbd>Enter</kbd>. Isso deverá ativar o Copilot Chat.
4. Se você ainda não estiver autenticado, será solicitado que entre na sua conta do GitHub. Siga as instruções para autenticar.

    ![Prompt de autenticação do Copilot Chat](../../../_images/ex1-copilot-authentication.png)

5. Depois da autenticação, a janela do Copilot Chat deverá aparecer.

## Resumo

Você criou uma cópia do repositório do laboratório e iniciou a criação do codespace que usará para começar a escrever código.

## Próxima etapa

Agora, vamos usar o Copilot. Continue para [Exercício 1 - Instruções personalizadas][next-lesson], onde você ensinará as convenções do projeto.

## Recursos

- [Visão geral do GitHub Codespaces][codespaces]
- [Criar um repositório a partir de um modelo][template-repository]
- [Introdução ao Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
