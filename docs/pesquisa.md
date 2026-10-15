# Ideias e decisões de pesquisa

Registro inicial: 14 de outubro de 2026.

## Esclarecimento da direção principal — 15/10/2026

O autor confirmou que pretende investigar um novo modelo de ML, com algoritmo e arquitetura próprios, que aprenda regras e relações e resolva problemas, buscando capacidades comparáveis às dos LLMs com menor custo computacional. O S.E.E.D. é o modelo pretendido; livro, experimentos e diagnóstico são componentes propostos de seu aprendizado. Economia no treinamento, na inferência ou em ambos permanece como pergunta, sem prioridade definida.

As perguntas sobre representação, atualização do aprendiz, descoberta, resolução de problemas, generalização e avaliação de custo foram registradas na [direção principal](direcao-do-modelo.md). Esse esclarecimento prevalece sobre interpretações anteriores que restringiam o projeto à política tabular. A tabela v0.1 é uma hipótese auxiliar, sem implementação obrigatória ou evidência de substituir LLMs.

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
5. Usar o retorno para ajustar o aprendiz; a [proposta v0.1](mecanismo-de-aprendizado.md), de 15/10/2026, especifica uma tabela de resultados para priorizar revisões. O treinamento de um modelo gerador permanece em aberto.

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

Para a arquitetura ampla, precisamos especificar a representação das regras, a geração de hipóteses, a escolha de experimentos e o método de aprendizado. A proposta v0.1 registrada abaixo fornece escolhas iniciais limitadas para começar. Aprender ações por recompensa não garante produzir explicações simbólicas corretas.

## Papel proposto para o modelo de linguagem

Auxiliar na formulação de hipóteses, na organização de relatórios e na explicação dos resultados. A avaliação deve se apoiar em evidências e verificadores adequados, com registro das saídas e decisões de triagem. O modelo de linguagem não será tratado como prova de verdade.

## Direção incorporada: aprender com o diagnóstico de erros

Objetivo confirmado pelo autor: investigar um novo algoritmo, indo além de integrar componentes existentes.

A hipótese de mecanismo é substituir um retorno apenas binário por informação estruturada sobre as falhas: onde uma hipótese não explica os resultados, quais mudanças poderiam corrigi-la e que experimentos distinguem essas possibilidades. Investigar se o aprendiz consegue reutilizar estratégias de diagnóstico e revisão em problemas novos.

Proposta do autor: integrar outro modelo, possivelmente especializado, para interpretar os erros, sugerir explicações e testes. Esse papel complementa a triagem e revisão de regras por LLM sugerida originalmente. Diagnóstico é uma hipótese testável; não equivale a uma causa comprovada.

Fluxo proposto: hipótese → teste → detecção de discrepância → diagnósticos candidatos → experimento discriminante → revisão da hipótese → registro para aprendizado posterior.

Detalhes, limites e avaliação estão em [diagnóstico e revisão](diagnostico-e-revisao.md).

## Proposta operacional registrada em 15/10/2026

O [mecanismo de aprendizado v0.1](mecanismo-de-aprendizado.md) distingue a descoberta da regra de uma tarefa do aprendizado entre tarefas. Propõe atualizar contagens e médias de sucesso para cada contexto de erro e operação de revisão; a política resultante altera a ordem das tentativas futuras. Começa com quatro operações fixas sobre expressões simples e intervenções predefinidas. A geração de novas operações, a escolha aprendida de experimentos e o treinamento de uma rede ficam para etapas posteriores.

O [protocolo da política de revisão](primeiro-experimento.md) avalia essa política sob orçamento controlado, caso esse experimento auxiliar seja realizado. O estudo anterior sobre gabarito fica registrado como direção histórica, sem resultados. Essa proposta não estabelece originalidade nem presume melhora; suas restrições não definem o modelo principal.

## Questões em aberto

Há escolhas iniciais para domínio, representação e atualização na v0.1. As perguntas abaixo continuam relevantes para ampliar o sistema e avaliar se essas escolhas são suficientes.

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
