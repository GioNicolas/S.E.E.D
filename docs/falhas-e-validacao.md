# Falhas possíveis e plano de avaliação

Registro: 14 de outubro de 2026.

Estado geral: todos os controles abaixo são propostas. Nenhum risco foi demonstrado como resolvido. A numeração serve para ligar experimentos futuros aos problemas avaliados.

| ID | Falha possível | Controle proposto | Como avaliar |
| --- | --- | --- | --- |
| F01 | Confirmação circular: a hipótese altera o ambiente usado para validá-la. | Separar o livro do aprendiz da implementação do ambiente de avaliação. | Alterar uma hipótese e verificar que as leis do ambiente não mudam. |
| F02 | Bug recorrente aceito como lei. | Buscar condições variadas, implementações independentes e evidências externas. | Inserir defeitos sistemáticos em uma implementação e medir conclusões falsas e investigações solicitadas. |
| F03 | Regra sem condições de aplicação; anomalia rara descartada. | Registrar escopo e exceções; preservar observações anômalas. | Introduzir mudanças de material ou condições que alterem o comportamento e verificar revisão de escopo. |
| F04 | Agente age bem, mas não produz explicações verificáveis. | Definir linguagem de hipóteses e verificar suas previsões separadamente da recompensa por ação. | Comparar sucesso da tarefa com recuperação de regras e erro de previsão. |
| F05 | Memorização, ajuste aos exemplos ou vazamento do gabarito. | Separar treino e avaliação, remover pistas acidentais e reservar famílias de regras. | Medir desempenho em situações e famílias novas; testar sensibilidade a nomes e formatos. |
| F06 | Coincidência confundida com causa. | Realizar intervenções que variem fatores separadamente. | Correlacionar cor e massa no início e depois romper essa correlação. |
| F07 | Livro errado ou incompleto ensina uma conclusão falsa. | Separar gabaritos verificados de livros deliberadamente imperfeitos e registrar a origem das regras. | Comparar exercícios com livros corretos, incompletos e incorretos. |
| F08 | Evidências insuficientes levam a uma resposta forçada. | Manter hipóteses alternativas e permitir solicitar mais dados ou abster-se. | Apresentar dados ambíguos e verificar se a certeza aumenta apenas com evidência discriminante. |
| F09 | Regra equivalente rejeitada, ou regra aproximadamente correta aceita como exata. | Combinar verificação simbólica quando viável com avaliação numérica e condições de domínio. | Usar expressões equivalentes, aproximações e casos de divisão por zero. |
| F10 | Aprendiz explora o avaliador para ganhar nota. | Manter o avaliador separado, com critérios verificáveis e testes reservados. | Introduzir pistas irrelevantes e respostas que exploram métricas sem melhorar previsões. |
| F11 | Regra nova prejudica conclusões anteriores. | Registrar dependências, testar regressões e manter versões reversíveis. | Promover uma regra deliberadamente conflitante e verificar detecção e recuperação. |
| F12 | Fora do treino, não existe gabarito para a descoberta. | Avaliar previsões novas, hipóteses concorrentes e evidências independentes. | Ocultar respostas do aprendiz; o avaliador pode mantê-las reservadas em benchmarks sintéticos. |
| F13 | Ruído, variáveis ocultas ou mudança do ambiente confundem a revisão. | Registrar qualidade das medições e distinguir mudança de contexto de mudança de regra. | Variar ruído e introduzir fatores ocultos; medir erro, confiança e pedidos de evidência adicional. |
| F14 | LLM ou humano aprova/rejeita hipóteses sem suporte; triagem perde descobertas. | Preservar relatórios brutos, decisões e evidências; auditar uma amostra das hipóteses rejeitadas. | Comparar decisões com verificações independentes e medir falsos descartes e falsas aprovações. |
| F15 | Busca e curadoria ficam caras ou não terminam. | Definir orçamento de consultas, tempo e critérios de parada. | Comparar qualidade das descobertas, consumo de recursos e fila de revisão sob orçamento fixo. |
| F16 | Modelo de diagnóstico inventa um motivo convincente e a revisão o aceita sem teste. | Manter explicações alternativas, ligar diagnósticos aos dados e exigir previsões discriminantes. | Inserir diagnósticos incorretos; verificar se os experimentos os refutam e se o sistema preserva incerteza. |

## Ordem sugerida

1. Definir hipóteses e avaliador; garantir separação entre gabarito, aprendiz e ambiente.
2. Medir descoberta e previsões em condições novas, com dados limpos.
3. Introduzir ambiguidade, ruído e livros imperfeitos.
4. Introduzir defeitos e uma implementação independente.
5. Avaliar atualização de conhecimento, regressões e custo de curadoria.

## Métricas e comparações

- Erro de previsão em dados reservados.
- Proporção de regras aceitas que são falsas e proporção de regras válidas recuperadas.
- Capacidade de manter incerteza quando os dados não permitem decidir.
- Número de experimentos e custo até uma decisão.
- Desempenho em famílias fora do treinamento.
- Regressões após atualizações e capacidade de rollback.

Comparar com um método existente apropriado ao domínio e com versões do S.E.E.D. sem cada controle. Usar o mesmo orçamento, registrar as sementes e definir critérios de sucesso antes da avaliação final. Redução de erros acompanhada de rejeição de todas as descobertas não constitui sucesso.

## Limites

Experimentos finitos não provam funcionamento universal. Uma simulação isolada pode não permitir distinguir bug e lei. Testes em mundos sintéticos avaliam o comportamento do método nesses mundos; extrapolação para o mundo real exige evidência adicional.

Referências e contexto: [escopo da pesquisa](escopo-da-pesquisa.md). Exemplos de exploração de objetivos e defeitos: [DeepMind — Specification gaming](https://deepmind.google/blog/specification-gaming-the-flip-side-of-ai-ingenuity/).

Detalhamento da proposta de módulo especializado: [diagnóstico e revisão](diagnostico-e-revisao.md).
