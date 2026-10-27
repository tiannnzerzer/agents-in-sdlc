---
title: "Lição 12 - Implantar o site com o GitHub Pages"
description: "Use o aplicativo GitHub Copilot para configurar um workflow do Pages, revisar o pull request e verificar o Space Quiz publicado."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Publique o Space Quiz com o GitHub Pages para que outras pessoas possam acessá-lo. O Copilot pode preparar o workflow e orientar a implantação, mas cabe a você revisar as alterações e confirmar que o site está disponível.

Nesta lição, você vai:

- pedir ao Copilot para configurar a implantação com o GitHub Pages.
- revisar o workflow e o pull request antes de fazer o merge.
- acompanhar a implantação e verificar o site publicado.

## Cenário

O Space Quiz já está em um repositório do GitHub, mas as pessoas ainda precisam de uma forma simples de experimentá-lo. Publicar o site de arquivo único com o GitHub Pages o disponibiliza em uma URL pública, sem adicionar um servidor ou uma dependência de tempo de execução.

## Peça ao Copilot para implantar o site

Na sessão do aplicativo Copilot para `space-quiz`, envie este prompt:

```plaintext
Habilite o GitHub Pages para este repositório. Crie um workflow do GitHub Actions que compile o site, se necessário, e o implante no GitHub Pages. Adicione um README que descreva o aplicativo e como executá-lo localmente, caso ainda não exista. Crie um pull request com essas alterações e faça o merge depois que as verificações obrigatórias forem aprovadas. Acompanhe a implantação até que ela seja concluída com êxito e, em seguida, informe a URL final do site publicado. Se você não tiver permissão para habilitar o Pages ou fazer o merge, diga exatamente o que preciso aprovar ou fazer; não informe que a tarefa foi concluída até que o site esteja publicado.
```

![Diagrama em três etapas: um pull request de space-quiz integrado; uma execução bem-sucedida de Deploy website no GitHub Actions, com as etapas Build website, Upload artifact e Deploy to Pages; e o Space Quiz publicado em yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> Este prompt pede ao agente que crie e integre um pull request e altere as configurações do Pages do repositório. Revise o diff, as permissões de implantação e quaisquer solicitações de aprovação do GitHub antes de continuar.

## Revise o workflow e verifique a implantação

1. Confirme se o README só foi adicionado caso ainda não existisse no repositório.
2. Revise as alterações do pull request, incluindo as etapas de compilação e implantação do workflow.
3. Faça o merge somente depois que as verificações obrigatórias do pull request forem aprovadas.
4. Acompanhe a execução de implantação do GitHub Actions até que ela seja concluída com êxito.
5. Abra a URL informada e confirme se o Space Quiz carrega.

Se o agente não puder habilitar o Pages ou integrar o pull request, siga as instruções específicas que ele fornecer. Não considere a tarefa concluída até que a implantação seja bem-sucedida e o site publicado carregue.

## Resumo e próximos passos

Agora o Space Quiz está disponível como um site publicado, e você verificou a implantação em vez de confiar apenas no relato do agente. Continue para [Lição 13: Revisão e próximos passos][next-lesson].

[next-lesson]: ../13-review/
