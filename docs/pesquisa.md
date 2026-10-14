# Ideias e decisões de pesquisa

Registro inicial: 14 de outubro de 2026.

## Proposta inicial

Combinar conhecimento explícito, experimentação em um ambiente virtual e atualização controlada de regras. O fluxo proposto inclui hipóteses, triagem assistida por modelo de linguagem, revisão humana, testes em staging e promoção para uma versão de produção.

O livro representa o conhecimento atualmente aceito. Cada versão pode ser preservada; novas evidências podem motivar revisões e rollback. A aceitação de uma regra não equivale a uma verdade universal.

## Ideia incorporada ao projeto: o livro como gabarito

Proposta do autor: usar regras conhecidas para avaliar e treinar um modelo que tenta deduzir regras a partir de situações e observações.

Essa proposta foi registrada no brainstorming do S.E.E.D., mas possui precedentes próximos na literatura. "Neural Symbolic Regression that Scales" treina um modelo para recuperar expressões a partir de exemplos gerados por equações conhecidas. Não reivindicamos originalidade desse princípio. Ver [escopo da pesquisa](escopo-da-pesquisa.md).

Fluxo de treinamento proposto:

1. Selecionar uma regra conhecida e suas condições de aplicação.
2. Apresentar situações e resultados ao aprendiz, sem revelar a resposta-alvo.
3. Solicitar uma hipótese explícita e previsões para novos casos.
4. Comparar a hipótese e as previsões com o gabarito por verificações apropriadas ao domínio.
5. Usar o retorno para ajustar o aprendiz; o método de ajuste ainda precisa ser definido.

O objetivo é investigar se o sistema aprende procedimentos de descoberta que se transferem para problemas novos. Treinar e avaliar nas mesmas regras pode medir memorização, sem demonstrar essa transferência.

Uma situação isolada pode admitir várias explicações. O aprendiz deve receber situações variadas ou poder escolher experimentos que diferenciem hipóteses concorrentes.

O avaliador deve reconhecer regras equivalentes, como `F = m × a` e `a = F/m`, respeitando as condições matemáticas. A correspondência literal com o texto do livro não basta. Regras alternativas compatíveis com evidências limitadas devem ser registradas, sem forçar uma certeza prematura.

## Cuidados discutidos

### Separar crenças e ambiente

O livro do aprendiz não deve alterar automaticamente as leis do ambiente usado para avaliar sua hipótese. Caso contrário, uma hipótese pode produzir sua própria confirmação. Aprender uma regra do simulador não demonstra sua validade no mundo real.

### Repetição não distingue lei de bug

Um defeito sistemático pode aparecer em todas as tentativas. Repetição motiva investigação, mas validação deve incluir condições variadas, implementações independentes e, quando pertinente, evidências externas. Dois processos usando o mesmo motor não constituem necessariamente validações independentes.

### Regras precisam de escopo

Registrar condições, evidências, confiança, exceções e versão. Uma anomalia deve ser preservada para investigação, mesmo quando não altera o conhecimento aceito. Frequência baixa não é motivo suficiente para apagá-la.

### Definir o mecanismo de descoberta

Ainda precisamos especificar a representação das regras, a geração de hipóteses, a escolha de experimentos e o método de aprendizado. Aprender ações por recompensa não garante produzir explicações simbólicas corretas.

## Papel proposto para o modelo de linguagem

Auxiliar na formulação de hipóteses, na organização de relatórios e na explicação dos resultados. A avaliação deve se apoiar em evidências e verificadores adequados, com registro das saídas e decisões de triagem. O modelo de linguagem não será tratado como prova de verdade.

## Questões em aberto

- Qual domínio inicial permite uma avaliação pequena e verificável?
- Qual formato de regra admite hipóteses e verificações automáticas?
- Como o aprendiz escolhe o próximo experimento?
- Como o retorno do gabarito será usado para treinar o sistema?
- Como separar treino e avaliação para testar transferência entre famílias de regras?
- Como distinguir regra incompleta, erro de observação e defeito do ambiente?

## Trabalhos próximos consultados

- [Neural Symbolic Regression that Scales — ICML 2021](https://proceedings.mlr.press/v139/biggio21a.html): treinamento com equações conhecidas e seus exemplos para prever expressões simbólicas.
- [Science-Gym — artigo publicado em 7 de janeiro de 2026](https://link.springer.com/article/10.1007/s10994-025-06914-x): ambientes e avaliações para coleta autônoma de dados, escolha de experimentos e descoberta de equações; é um testbed, não uma solução universal.
- [DreamCoder](https://arxiv.org/abs/2006.08381): aprendizado de programas e abstrações simbólicas reutilizáveis.
- [AI Feynman](https://arxiv.org/abs/1905.11481): busca de expressões matemáticas que expliquem dados.
- [Specification gaming — DeepMind](https://deepmind.google/blog/specification-gaming-the-flip-side-of-ai-ingenuity/): exemplos de exploração de objetivos mal especificados e defeitos em ambientes.

Essas referências situam componentes da proposta. Ainda não foi feita uma revisão sistemática da literatura nem demonstrada a originalidade da combinação.

As falhas discutidas e os testes propostos estão registrados no [plano de avaliação](falhas-e-validacao.md). Nenhum controle foi implementado ou validado até o momento.
