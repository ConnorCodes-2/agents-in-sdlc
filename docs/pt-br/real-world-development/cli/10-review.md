---
title: "Lição 10 - Recapitulação e próximos passos"
description: "Recapitule o fluxo de trabalho do Copilot CLI, as duas pull requests, as personalizações reutilizáveis e outros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Você usou o GitHub Copilot CLI em todo o fluxo de trabalho contínuo da Tailspin Toys. Você:

- preparou um Codespace, instalou o Copilot CLI, explorou o projeto e encontrou a issue de filtragem já criada.
- adicionou avaliações com estrelas, revisou o resultado em um navegador encaminhado e mesclou manualmente sua primeira pull request (PR).
- partiu da issue de filtragem, definiu a abordagem no modo Plan, implementou no modo Autopilot e revisou no modo Interactive.
- orientou o agente com instruções personalizadas, personalizou a skill existente `quality-checks` e a usou para executar testes unitários, lint e verificações de tipos.
- adicionou o servidor Model Context Protocol (MCP) do Playwright e o usou para explorar a filtragem em um navegador real.
- criou e selecionou um agente personalizado de garantia de qualidade (QA) para avaliar os requisitos, a cobertura, os resultados da skill e as evidências do navegador.
- revisou toda a alteração de filtragem e autorizou o Agent Merge para a PR da funcionalidade.
- explorou comandos de barra para contexto, modelos, compartilhamento e delegação opcional na nuvem.

## O que você entregou

O workshop inclui dois marcos de PR, cada um em uma branch própria baseada em `main` atualizada:

1. **Avaliações com estrelas:** exibe `starRating` e um estado explícito para jogos sem avaliação nos cards.
2. **Filtragem e fluxo de qualidade:** implementa a filtragem, atualiza as instruções e as aplica à funcionalidade, personaliza o relatório `quality-checks`, cria um perfil de QA e inclui os testes relacionados.

Do planejamento da filtragem à abertura da PR, você usou a mesma conversa e branch. Combinamos todo esse trabalho em uma única PR para agilizar o workshop.

## Diferentes tipos de verificação

Você verificou o código de várias maneiras: com testes automatizados, sua própria inspeção no navegador e a exploração do navegador pelo Copilot usando MCP. A skill `quality-checks` executou testes unitários, lint e verificações de tipos e apresentou os resultados no novo formato. O QA reuniu esses resultados com uma revisão dos requisitos e da cobertura dos testes antes da PR.

Os testes adicionados devem preencher lacunas reais; pode ser correto que uma execução de QA não precise de testes novos. Revise o código e as evidências antes de autorizar a mesclagem e atualize as evidências afetadas após as alterações.

## Práticas recomendadas

O contexto e as ferramentas fornecidos ao Copilot moldam o trabalho dele. Neste workshop, você atualizou instruções, personalizou uma skill, criou um perfil de QA e configurou um servidor MCP. Reutilize essas personalizações em outras conversas e ajuste-as conforme as necessidades da equipe. As instruções definem padrões, as skills descrevem tarefas repetíveis, os agentes personalizados definem funções especializadas e os servidores MCP conectam ferramentas externas. Revise as alterações reais e os resultados das ferramentas, não apenas o resumo do agente.

Escolha o **modo e o modelo** de acordo com a tarefa. Use **Plan** para pensar na abordagem antes de começar, **Interactive** para acompanhar alterações focadas e **Autopilot** para tarefas bem delimitadas. Escolha um modelo mais rápido para alterações rotineiras e outro mais avançado para tarefas complexas.

O contexto continua sendo tão importante quanto a infraestrutura. Descrever claramente *o que* você quer desenvolver, *por que* e *como* altera significativamente o resultado.

## Mais para explorar

Você aprendeu o fluxo principal. Confira outros recursos do CLI:

- `/review` para pedir ao agente de revisão de código que analise as alterações.
- `/rubber-duck` para conversar sobre um problema e obter outra perspectiva.
- `/fleet` para coordenar tarefas independentes em paralelo.
- `/worktree` para isolar uma tarefa separada.
- `/delegate` para enviar uma tarefa ao Copilot cloud agent.

## Próximos passos

A melhor maneira de melhorar no uso de qualquer ferramenta é continuar usando-a! Use-a em código de produção, em projetos pessoais ou naquele pequeno aplicativo que você pensa em criar há anos. Compartilhe seus aprendizados com a equipe e aprenda com os outros. E, como sempre, explore a documentação.

Se quiser ampliar o Tailspin Toys pelo terminal, continue com a [série opcional Foundry Backer Concierge][foundry]. Para comparar outros ambientes, explore o [workshop do VS Code][vscode], o [workshop do aplicativo GitHub Copilot][app] ou o [workshop do Copilot cloud agent][cloud].

## Recursos

- [Sobre o GitHub Copilot CLI][about-cli]
- [Referência de comandos do Copilot CLI][cli-reference]
- [Personalizar o Copilot CLI][customize-cli]
- [Gerenciar pull requests com o Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
