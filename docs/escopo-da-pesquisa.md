# O que já existe e o que investigar

Registro: 14 de outubro de 2026.

## A distinção principal

Há métodos existentes para aprender expressões, programas e abstrações simbólicas a partir de exemplos. Há também ambientes para avaliar agentes que escolhem experimentos e descobrem equações.

O que falta definir no S.E.E.D. não é necessariamente algo que falta em toda a área. Precisamos escolher métodos, implementar uma combinação concreta e comparar seu comportamento com trabalhos próximos. A existência de componentes anteriores não demonstra que o sistema proposto seja equivalente a eles; a ausência de uma implementação nossa não demonstra uma lacuna científica inédita.

## Componentes com precedentes

| Componente | Referência próxima | O que ela demonstra ou oferece |
| --- | --- | --- |
| Aprender a recuperar uma regra usando respostas conhecidas no treino | [Neural Symbolic Regression that Scales, 2021](https://proceedings.mlr.press/v139/biggio21a.html) | Equações geradas e exemplos numéricos são usados no treinamento para prever expressões simbólicas. |
| Aprender abstrações simbólicas com componentes neurais | [DreamCoder](https://arxiv.org/abs/2006.08381) | Um sistema aprende programas e amplia uma biblioteca de abstrações reutilizáveis. |
| Escolher experimentos e buscar equações a partir dos dados obtidos | [Science-Gym, 2026](https://link.springer.com/article/10.1007/s10994-025-06914-x) | Um testbed disponibiliza simulações e protocolos para avaliar agentes nesses processos, incluindo métodos de referência. |

Esses trabalhos têm escopos específicos. Recuperar equações em tarefas avaliadas não equivale a aprender toda a lógica do mundo ou garantir correção universal.

## O que precisamos decidir para construir o projeto

- Como representar regras: começar, por exemplo, com expressões matemáticas e condições de aplicação.
- Qual método existente usar para propor hipóteses e como incorporar o retorno do gabarito.
- Como selecionar o próximo experimento e limitar o custo da investigação.
- Como registrar evidências, alternativas, confiança e dependências entre regras.
- Quais critérios permitem promover, revisar ou reverter uma regra.
- Como avaliar o comportamento quando o livro estiver incompleto, errado ou sem uma resposta para a hipótese nova.

São decisões de implementação. Só uma comparação com a literatura e resultados experimentais pode identificar quais delas constituem contribuições novas.

## Uma hipótese de contribuição a investigar

Uma direção possível é combinar aprendizado com gabarito, escolha de experimentos, tratamento explícito de incerteza e revisão de conhecimento para reduzir a aceitação de regras falsas em ambientes com dados imperfeitos.

Pergunta proposta: sob o mesmo orçamento de experimentos, o sistema consegue manter boas previsões e descobrir regras úteis, aceitando menos conclusões falsas quando há ruído, livro incompleto ou defeitos do ambiente?

Não basta rejeitar todas as hipóteses para reduzir erros. Precisamos medir também a aceitação de regras válidas, o custo e a capacidade de fazer previsões.

## Exemplo de avaliação

Um objeto desacelera. O livro sugere um modelo simples. Os dados podem indicar atrito, uma condição ausente, erro de medição ou um bug. O sistema deve registrar hipóteses concorrentes, escolher experimentos informativos e atualizar o conhecimento somente com suporte adequado.

Se as observações não distinguirem as hipóteses, o resultado esperado é manter a incerteza e indicar qual evidência está faltando. Identificar um bug pode exigir observações de uma implementação independente ou outra fonte externa; nem sempre é possível fazê-lo com dados de um único ambiente.

## Critério de contribuição

Comparar métodos existentes, a combinação completa e versões com componentes removidos. Uma melhora reproduzível em confiabilidade, custo ou transferência pode apoiar uma contribuição no domínio avaliado. Um protótipo que apenas reúne componentes existentes pode ainda ser útil, sem demonstrar um novo algoritmo.

Estado: hipótese de pesquisa, sem implementação ou resultados. Originalidade da combinação ainda não estabelecida.
