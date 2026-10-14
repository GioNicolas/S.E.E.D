# Diagnóstico de erros e revisão de hipóteses

Registro: 14 de outubro de 2026.

Estado: proposta conceitual autorizada para registro pelo autor. Ainda não existe implementação, treinamento ou demonstração de originalidade.

## Objetivo de pesquisa

Investigar um novo algoritmo que aprenda estratégias de diagnóstico e revisão de regras a partir de falhas observadas e consiga reutilizar essas estratégias em problemas novos.

O livro como gabarito pode fornecer supervisão nos exercícios de treinamento. O retorno proposto inclui informação sobre a estrutura do erro, além de acerto ou erro da resposta final.

## Ciclo proposto

1. Propor uma hipótese e registrar suas condições e previsões.
2. Executar um experimento e medir a diferença entre previsão e observação.
3. Localizar padrões da discrepância: em quais casos ocorre, com quais variáveis e com qual magnitude.
4. Gerar explicações candidatas e alterações possíveis na hipótese.
5. Escolher um experimento cujos resultados possam distinguir as explicações.
6. Confrontar o resultado com as previsões de cada alternativa.
7. Revisar a hipótese, preservar alternativas ainda viáveis e registrar o que foi aprendido.

Não é obrigatório obter uma causa única. Se as evidências forem insuficientes, registrar incerteza e as observações necessárias para avançar.

## Proposta de modelo especializado

O autor sugere integrar outro modelo para traduzir falhas em explicações candidatas e possibilidades de investigação, de forma semelhante ao uso de LLMs na triagem de novas regras.

Entradas propostas: hipótese, condições de aplicação, dados do experimento, previsões, discrepâncias e histórico de testes. O acesso ao livro depende do regime de treinamento ou avaliação; respostas reservadas não devem vazar para o aprendiz nem para seu diagnóstico durante a avaliação final.

Saídas propostas:

- Uma descrição objetiva de onde a previsão falhou, ligada aos dados.
- Explicações alternativas, como variável ausente, relação invertida, condição omitida, erro de medição ou defeito do ambiente.
- Revisões candidatas da regra.
- Experimentos propostos, com fatores controlados e resultados esperados para cada alternativa.
- Limitações da evidência e possibilidade de abstenção.

O módulo pode inicialmente ser um modelo existente orientado para essa tarefa. Treinar ou especializar um modelo dedicado é uma opção a avaliar, não uma decisão tomada. Os papéis também podem ser executados por um mesmo modelo; processos distintos usando o mesmo modelo não garantem independência de erros.

## Exemplo

Hipótese: aplicar a mesma força sempre produz a mesma aceleração. Dois objetos apresentam acelerações distintas.

Diagnósticos candidatos: faltou considerar a massa; o atrito difere; a medição é inadequada. Um teste possível varia a massa, mantendo a força e as condições de atrito controladas. As previsões específicas de cada alternativa precisam ser registradas antes de observar o resultado.

O diagnóstico só ganha suporte conforme os experimentos diferenciam as alternativas. Uma explicação convincente em linguagem natural não comprova a causa.

## Relação com o verificador e a revisão de regras

O modelo de diagnóstico propõe interpretações e testes. O verificador confronta previsões, evidências e consistência formal quando aplicável. A triagem por LLM organiza o relatório para revisão. O processo de promoção de regras utiliza esses registros, testes e os critérios de curadoria definidos pelo projeto.

Não basta um modelo explicar um erro e outro concordar. A concordância pode refletir o mesmo equívoco; aprovação deve estar ligada a evidências verificáveis.

## Treinamento e transferência

Uma opção é criar exercícios com regras conhecidas e hipóteses deliberadamente alteradas, por exemplo removendo uma variável ou condição. O avaliador conhece a alteração e pode oferecer supervisão. Entretanto, a alteração inserida não é necessariamente a única explicação compatível com observações limitadas.

Separar famílias de regras e mecanismos de falha entre treino e avaliação. No teste final, o aprendiz e seu módulo de diagnóstico recebem somente a informação disponível ao sistema; o avaliador pode guardar respostas conhecidas para medir o desempenho.

Guardar episódios numa memória e atualizar os parâmetros de um modelo são mecanismos distintos. Precisamos definir como a experiência altera efetivamente a estratégia de diagnóstico e demonstrar sua reutilização em problemas novos.

## Como avaliar a contribuição

Comparar retorno binário, discrepâncias numéricas, diagnóstico estruturado por modelo existente e o mecanismo especializado proposto, sob recursos comparáveis. Medir recuperação de regras válidas, conclusões falsas, erro de previsão, custo e transferência.

Incluir diagnósticos errados para avaliar se os testes conseguem refutá-los. Verificar se a melhora depende da escolha de experimentos, do diagnóstico ou apenas de mais consultas ao modelo.

## O que ainda falta inventar ou definir

- Representação computável dos tipos de erro e operações de revisão.
- Procedimento para gerar, comparar e atualizar diagnósticos candidatos.
- Critério de escolha do próximo experimento e de parada.
- Regra de aprendizado que permita reutilizar estratégias entre problemas.
- Revisão da literatura especificamente sobre esses mecanismos.

A presença de dois modelos não demonstra um novo algoritmo. O mecanismo definido e seus resultados devem sustentar a eventual contribuição.
