# O que já existe e o que investigar

Registro: 14 de outubro de 2026.

## Direção principal esclarecida em 15/10/2026

A ambição confirmada pelo autor é investigar um novo modelo de ML, com algoritmo e arquitetura próprios, aprendizado de regras e relações e menor custo computacional frente a capacidades selecionadas dos LLMs. A comparação deve considerar qualidade, generalização e custos totais; a prioridade entre treinamento e inferência continua aberta. Ver [direção principal e perguntas](direcao-do-modelo.md).

O diagnóstico de erros é uma direção de mecanismo dentro dessa investigação. A proposta da tabela de operações é auxiliar e não define o modelo pretendido. Sua execução e eventual melhora não demonstrariam substituição de LLMs ou eficiência geral comparável.

## A distinção principal

Há métodos existentes para aprender expressões, programas e abstrações simbólicas a partir de exemplos. Há também ambientes para avaliar agentes que escolhem experimentos e descobrem equações.

O que falta definir no S.E.E.D. não é necessariamente algo que falta em toda a área. Precisamos escolher métodos, implementar uma combinação concreta e comparar seu comportamento com trabalhos próximos. A existência de componentes anteriores não demonstra que o sistema proposto seja equivalente a eles; a ausência de uma implementação nossa não demonstra uma lacuna científica inédita.

## Atualização de 16/10/2026 — O "Degrau Inédito" (Arquitetura A + B)

A arquitetura principal do S.E.E.D. foi formalizada em [Arquitetura e Algoritmo S.E.E.D. (A + B)](arquitetura-seed-a-b.md). A conferência das referências, também em 16/10/2026, mostrou que cada peça já tem precedente próximo. A comparação detalhada e a hipótese de contribuição revisada estão na seção 1 daquele documento.

## Componentes com precedentes (até 2026)

| Componente | Referência próxima | O que ela demonstra | Limitação que o S.E.E.D. (A + B) ataca |
| --- | --- | --- | --- |
| Aprender a recuperar uma regra usando respostas conhecidas no treino | [Neural Symbolic Regression that Scales, 2021](https://proceedings.mlr.press/v139/biggio21a.html) | Equações geradas e exemplos numéricos são usados no treinamento para prever expressões simbólicas. | Modelo neural estático; não constrói grafo modular multi-regime nem faz intervenção ativa. |
| Aprender abstrações simbólicas com componentes neurais | [DreamCoder, 2021](https://arxiv.org/abs/2006.08381) | Um sistema aprende programas e amplia uma biblioteca de abstrações reutilizáveis. | Busca enumerativa/wake-sleep cara; não utiliza a topologia do resíduo para editar regras nem particiona regimes. |
| Modelos de mundo simbólicos em código | *WorldCoder* (NeurIPS 2024), *Code World Models* (2025) | Representar leis do ambiente como código modular evita esquecimento catastrófico e dá alta eficiência amostral. | **Dependem de chamar uma LLM gigante** em loop para diagnosticar erros e reescrever o código (alto custo computacional). |
| Edição local de árvores de expressão com LLMs | [FunctionEvolve, 2026](https://arxiv.org/abs/2606.07704) | Edições locais preservam sub-expressões úteis; uma LLM propõe as mutações. | As mutações dependem de uma LLM; o S.E.E.D. propõe operadores numéricos. |
| Correção guiada pelo resíduo | [RED, 2025](https://arxiv.org/abs/2511.05586) (sem LLM); [RISR, 2026](https://arxiv.org/abs/2610.11387) (com LLM) | RED troca só a sub-expressão que o resíduo indica; RISR usa visões aditiva e multiplicativa do resíduo. | Trabalham num conjunto de dados fixo, sem guardas de domínio nem revisão contínua de um Livro. |
| Divisão em regimes | [Árvores de decisão simbólicas, 2026](https://arxiv.org/abs/2605.24275) | Aprendem juntas as condições de cada regime e a equação local. | Aprendizado em lote; o S.E.E.D. propõe criar regimes de forma incremental. |
| Escolha ativa de experimentos em regressão simbólica | [Medina e White et al.](https://arxiv.org/abs/2305.10379) | Escolhe o experimento onde as equações candidatas mais discordam. | Não trata regimes nem revisão de um Livro existente. |
| Ambientes de avaliação científica ativa | [Science-Gym, 2026](https://link.springer.com/article/10.1007/s10994-025-06914-x) | Testbed com simulações para avaliar agentes que escolhem experimentos e descobrem equações. | É um ambiente de benchmark, não uma nova arquitetura de aprendizado. |

## O que precisamos decidir para construir o projeto

- Como representar regras: começar, por exemplo, com expressões matemáticas e condições de aplicação.
- Qual método existente usar para propor hipóteses e como incorporar o retorno do gabarito.
- Como selecionar o próximo experimento e limitar o custo da investigação.
- Como registrar evidências, alternativas, confiança e dependências entre regras.
- Quais critérios permitem promover, revisar ou reverter uma regra.
- Como avaliar o comportamento quando o livro estiver incompleto, errado ou sem uma resposta para a hipótese nova.

São decisões de implementação. Só uma comparação com a literatura e resultados experimentais pode identificar quais delas constituem contribuições novas.

## Uma hipótese de contribuição a investigar

O objetivo do autor é descobrir um novo algoritmo. A direção atual é um mecanismo que use a estrutura dos erros para orientar revisão de hipóteses, escolha de experimentos e aprendizado reutilizável. A integração de um segundo modelo é uma opção de arquitetura; a possível contribuição algorítmica depende de definir precisamente como essas operações e atualizações funcionam. Ver [diagnóstico e revisão](diagnostico-e-revisao.md).

Uma direção possível é combinar aprendizado com gabarito, escolha de experimentos, tratamento explícito de incerteza e revisão de conhecimento para reduzir a aceitação de regras falsas em ambientes com dados imperfeitos.

Pergunta proposta: sob o mesmo orçamento de experimentos, o sistema consegue manter boas previsões e descobrir regras úteis, aceitando menos conclusões falsas quando há ruído, livro incompleto ou defeitos do ambiente?

Não basta rejeitar todas as hipóteses para reduzir erros. Precisamos medir também a aceitação de regras válidas, o custo e a capacidade de fazer previsões.

## Exemplo de avaliação

Um objeto desacelera. O livro sugere um modelo simples. Os dados podem indicar atrito, uma condição ausente, erro de medição ou um bug. O sistema deve registrar hipóteses concorrentes, escolher experimentos informativos e atualizar o conhecimento somente com suporte adequado.

Se as observações não distinguirem as hipóteses, o resultado esperado é manter a incerteza e indicar qual evidência está faltando. Identificar um bug pode exigir observações de uma implementação independente ou outra fonte externa; nem sempre é possível fazê-lo com dados de um único ambiente.

## Critério de contribuição

Comparar métodos existentes, a combinação completa e versões com componentes removidos. Uma melhora reproduzível em confiabilidade, custo ou transferência pode apoiar uma contribuição no domínio avaliado. Um protótipo que apenas reúne componentes existentes pode ainda ser útil, sem demonstrar um novo algoritmo.

Atualização de 15/10/2026: a [especificação operacional v0.1](mecanismo-de-aprendizado.md) propõe aprendizado tabular da ordem de operações fixas de revisão, condicionado a assinaturas de erro. É uma referência implementável para testar se a experiência melhora a busca. Não demonstra uma contribuição nova: a política, a representação de contexto e a biblioteca precisam ser comparadas com alternativas. A escolha aprendida de experimentos e a geração de novos operadores permanecem fora dessa versão.

Estado: hipótese de pesquisa com uma primeira especificação operacional, sem implementação ou resultados. Originalidade da combinação ainda não estabelecida.
