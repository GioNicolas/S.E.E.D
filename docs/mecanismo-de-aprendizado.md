# Como a experiência modifica o aprendiz — proposta auxiliar v0.1

Registro: 15 de outubro de 2026.

**Esclarecimento posterior do autor, em 15/10/2026:** o objetivo principal é investigar um novo modelo de ML, com algoritmo e arquitetura próprios e menor custo computacional frente a capacidades dos LLMs. Esta tabela é um experimento auxiliar possível; não define a arquitetura principal nem seu próximo passo obrigatório. Ver [direção principal e perguntas](direcao-do-modelo.md). Os limites descritos abaixo valem somente para o experimento tabular.

Estado: proposta de mecanismo inicial elaborada nesta conversa, para orientar a implementação. Não há código, treinamento ou resultados. As escolhas e os valores abaixo são pontos de partida revisáveis, não descobertas demonstradas. O objetivo de investigar um novo algoritmo permanece; esta versão estabelece uma referência simples e testável.

## Resposta direta

Neste experimento auxiliar, o aprendiz operaria em dois níveis. No problema atual, atualizaria uma hipótese simbólica e as evidências que a sustentam. Entre problemas, atualizaria uma política que decide quais operações de revisão tentar primeiro diante de determinados padrões de erro.

Na primeira versão, essa política será uma tabela de resultados por contexto e operação. A experiência muda seus números e, portanto, a ordem das próximas tentativas. A biblioteca inicial de operações será definida por nós. Descobrir novas operações e treinar uma rede neural são etapas posteriores possíveis.

Assim, o sistema guarda a regra específica descoberta e também aprende uma preferência reutilizável de investigação. Guardar episódios sem usá-los para alterar decisões futuras não satisfaz esse segundo objetivo.

## 1. O que fica na memória

| Estado | Conteúdo | Quando muda |
| --- | --- | --- |
| Hipóteses da tarefa | Expressões computáveis, parâmetros ajustados, domínio, alternativas e status | Após novas observações e testes |
| Episódios | Hipótese anterior, dados, contexto, operação tentada, previsões registradas antes do teste, resultado e custo | A cada tentativa, incluindo falhas |
| Política entre tarefas | Contagem de tentativas e média dos resultados para cada par contexto/operação | Durante o treinamento, após verificar uma tentativa |

O livro continua sendo um registro versionado de conhecimento aceito no escopo avaliado. Aceitar uma hipótese numa tarefa não a transforma em lei universal nem modifica o ambiente que forneceu as observações.

## 2. Representação inicial e limites

Começar com expressões matemáticas representadas por árvores: variáveis, constantes ajustáveis, soma, multiplicação e divisão com domínio restrito. Interpretar essa árvore por funções permitidas; não executar texto gerado com `eval`.

O primeiro estudo será numérico, com duas variáveis observáveis sem significado físico fornecido ao aprendiz. A hipótese inicial terá a forma `h(x, z) = k*x`; `z` estará disponível para observação e intervenção, embora ausente da hipótese. A versão v0.1 não descobre sensores nem variáveis ocultas.

Biblioteca fixa de quatro operações de revisão, aplicadas à hipótese inicial:

| Operação | Expressão candidata |
| --- | --- |
| Acrescentar constante | `k*x + b` |
| Acrescentar termo da variável omitida | `k*x + b*z` |
| Multiplicar pela variável omitida | `k*x*z` |
| Dividir pela variável omitida | `k*x/z`, com `z` afastado de zero |

Cada operação gera uma estrutura; os coeficientes `k` e `b`, quando presentes, são ajustados usando somente os dados de ajuste disponíveis na tarefa. Todas essas estruturas permitem ajuste por mínimos quadrados lineares nas respectivas colunas de entrada. Não é necessário treinar uma rede para ajustar esses coeficientes.

A busca v0.1 testa cada operação no máximo uma vez por tarefa e sempre a partir da hipótese inicial. Não encadeia revisões. Expansão da linguagem, regras condicionais, composições de operações e diagnóstico de bugs ficam para estudos posteriores.

Isso limita deliberadamente o espaço de respostas. Recuperar uma regra nesse espaço não demonstra invenção livre de leis ou aprendizado de lógica geral.

## 3. Como representar o erro sem entregar a resposta

Para cada observação, calcular `residuo = observado - previsto`. O contexto deve ser extraído apenas da hipótese e das observações acessíveis ao aprendiz.

Proposta inicial de assinatura discreta:

- Sinal da média dos resíduos: negativo, próximo de zero ou positivo.
- Associação entre resíduos e a variável omitida: negativa, fraca ou positiva.
- Associação entre resíduos e a previsão da hipótese inicial: negativa, fraca ou positiva.

Usar correlação de postos de Spearman para as associações; valores menores que `-0,5`, entre `-0,5` e `0,5` inclusive, e maiores que `0,5` definem as três faixas. Quando uma série for constante, marcar associação indisponível em vez de inventar uma correlação. Para o sinal médio, usar tolerância `1e-8 * max(1, raiz_da_media_dos_quadrados_dos_alvos)`. Esses limiares são escolhas iniciais, a congelar antes do teste final.

A assinatura não recebe a equação verdadeira, o nome da família de regras, o diagnóstico do gerador nem o identificador da tarefa. Os papéis das variáveis vêm da presença na hipótese e não de nomes como `massa` ou `forca`. Embaralhar nomes e ordem das colunas é um controle obrigatório.

Essa assinatura descreve evidências; não identifica uma causa. Por exemplo, associação com `z` motiva testar sua participação, mas não prova que uma divisão por `z` é a revisão correta. Uma representação pouco informativa também pode impedir a política de aprender: esse será um resultado a registrar.

## 4. Regra exata de atualização da política

Para cada contexto `c` e operação `o`, manter `N[c,o]`, número de tentativas, e `Q[c,o]`, média dos resultados observados.

Inicializar `N = 0` e `Q = 0`. Registrar o retorno após verificar cada tentativa durante o treinamento e aplicar as atualizações ao fim da tarefa:

```text
r = 1 se a candidata atender ao critério de confirmação; caso contrário, 0
N[c,o] = N[c,o] + 1
Q[c,o] = Q[c,o] + (r - Q[c,o]) / N[c,o]
```

O critério de confirmação usa novas observações do ambiente, separadas dos dados usados para ajustar coeficientes. A definição numérica e o orçamento estão no [primeiro experimento](primeiro-experimento.md). O gabarito reservado da avaliação final nunca fornece `r` ao aprendiz.

Na seleção durante o treinamento, com probabilidade `0,2`, escolher uniformemente uma operação ainda não tentada na tarefa; nas demais vezes, escolher a de maior `Q` entre as disponíveis. Desempatar aleatoriamente, com semente registrada. Contextos novos começam sem preferência. Essa exploração evita depender apenas das primeiras escolhas, sem garantir que toda ação receba dados suficientes num treinamento finito.

No teste final, congelar `N` e `Q`, usar a ordem por `Q` e desempates com semente registrada. A hipótese local e seus coeficientes podem mudar com os dados daquela tarefa, mas a memória entre tarefas não recebe atualizações. Restaurar o mesmo checkpoint antes de cada tarefa final.

O número `Q` é uma preferência empírica de busca, condicionada à distribuição e à assinatura usadas. Não é confiança calibrada na verdade de uma regra. Falhar numa tentativa não prova que a operação é inútil em outros contextos.

## 5. Escolha e verificação de experimentos

Na v0.1, o plano de coleta é fixo e explícito: variar `x` e `z` separadamente para observar a hipótese inicial; depois, testar candidatas em combinações novas de ambas as variáveis. Essa intervenção ajuda a investigar a variável omitida.

Usar um fluxo de pontos de confirmação gerado independentemente da operação escolhida. Cada candidata recebe um bloco novo e é verificada também nos blocos já revelados, conforme o protocolo do primeiro experimento. Não escolher testes mais favoráveis para determinada operação. Debitar consultas e avaliações do orçamento.

Os experimentos são realizados ativamente, mas a política v0.1 aprende somente a ordem das revisões. Aprender qual intervenção fazer é uma hipótese adicional para uma próxima versão. Essa separação permite verificar qual componente produziu eventual melhora.

Uma candidata que falha permanece registrada e não altera o livro aceito. Se nenhuma passar, ou várias forem indistinguíveis ao fim do orçamento, devolver hipótese não confirmada ou alternativas compatíveis. Confirmação significa adequação aos testes e ao domínio disponíveis, não prova universal.

## 6. Exemplo do que muda

Suponha uma tarefa cujo comportamento reservado seja `y = k*x/z`. O aprendiz começa com `y = k*x` e observa falhas quando `z` varia. Ainda precisa comparar multiplicação, divisão, termo adicional e constante.

Após ajustar e testar as candidatas, a divisão pode passar nas observações de confirmação. Nesse episódio:

1. A hipótese local passa a incluir `z` no denominador, com domínio e evidências.
2. O episódio guarda também as operações que falharam.
3. Durante o treino, a entrada `Q[contexto, dividir]` recebe resultado `1`; tentativas malsucedidas recebem `0` nas respectivas entradas.

Em uma tarefa nova com assinatura semelhante, a divisão pode ser tentada antes. Ela ainda será ajustada e testada; a memória não autoriza copiá-la como verdade. Se um contexto tiver `N=3` e `Q=2/3`, um novo sucesso produzirá `N=4` e `Q=3/4`.

O exemplo não demonstra que a assinatura proposta basta para aprender essa preferência. O experimento deve verificar isso.

## 7. Ciclo implementável

```text
carregar política ou inicializar tabela vazia
para cada tarefa:
    ajustar hipótese inicial nos dados iniciais
    coletar intervenções iniciais e calcular contexto
    manter política de entrada e contexto fixos durante esta tarefa
    confirmar hipótese inicial em observações novas; se passar, retornar
    enquanto houver operações e orçamento:
        escolher operação pela política de entrada
        gerar candidata e ajustar coeficientes nos dados de ajuste
        registrar previsões para pontos novos antes de obter suas respostas
        consultar ambiente e verificar candidata
        registrar episódio, custo e resultado
        parar se houver confirmação suficiente
    no treino: aplicar à tabela os resultados das operações tentadas
    no teste final: descartar atualizações entre tarefas
    retornar hipótese com escopo, alternativas ou abstenção
```

Atualizar a política entre tarefas evita que, dentro de um episódio, o retorno de uma candidata altere o significado da comparação entre políticas. As observações da tarefa podem ser usadas na verificação local conforme o protocolo; os alvos do teste final reservado continuam inacessíveis.

## 8. Como distinguir aprendizado de registro e de regras escritas por nós

O teste central é comparar a política treinada e congelada com a mesma arquitetura cuja tabela foi zerada, usando tarefas novas, dados pareados e os mesmos limites. Manter o gerador de candidatas, a assinatura, o ajuste numérico e os verificadores idênticos.

Incluir ainda busca aleatória sem reposição, ordem fixa, preferência global sem contexto e enumeração das quatro candidatas. Uma heurística fixa escolhida na validação serve de comparação adicional. Se ela empatar ou vencer, registrar isso; a complexidade da política precisa se justificar.

Para o estudo inicial, esperar que o possível ganho apareça no número de candidatas avaliadas até confirmação e no desempenho com orçamento de busca curto. As políticas com acesso às quatro candidatas podem empatar no resultado final. Coletar todos os dados de todas as tentativas de antemão pode eliminar qualquer economia de consultas; por isso custo de dados e custo de busca serão relatados separadamente.

A biblioteca, a assinatura, os limiares e o plano de intervenção são projetados por nós. A seleção empírica entre operações é aprendida. A v0.1 não aprende novas operações, sequências de investigação nem a representação dos contextos.

## 9. Relação com a ambição original e trabalhos existentes

A seleção entre ações baseada em resultados tem precedentes no estudo de exploração e aproveitamento, como [Auer, Cesa-Bianchi e Fischer, 2002](https://link.springer.com/article/10.1023/A:1013689704352). O protocolo proposto aqui não herda automaticamente as garantias desse trabalho e a tabela não é apresentada como invenção nova.

Aprender abstrações e orientar buscas por programas também possui precedentes, como [DreamCoder](https://arxiv.org/abs/2006.08381). A contribuição pretendida do S.E.E.D. exige revisão mais específica da literatura e evidência além de uma política tabular.

Se a v0.1 demonstrar utilidade, os passos seguintes serão investigar a escolha aprendida de experimentos e, depois, a construção de operações compostas de revisão. Cada ampliação terá uma comparação que mantenha os demais componentes iguais. Treinar uma rede é uma opção caso a representação tabular se mostre insuficiente, não um requisito para começar.

## 10. Entregas para iniciar a implementação

1. Gerador separado do aprendiz, expressões e ajuste de coeficientes; confirmar que mudar a hipótese não muda o ambiente.
2. Coleta, assinatura de erro e verificador; testar correlação indisponível e divisão inválida.
3. Política tabular, episódios e checkpoint; verificar a conta de atualização, persistência e congelamento no teste.
4. Comparações pareadas e relatório reproduzível; mostrar resultados positivos e negativos e a distribuição de contextos visitados.

Usar inicialmente Python, uma CLI local e arquivos JSON/JSONL para estados e episódios. LLM, interface gráfica e simulador físico completo ficam fora desta primeira execução. Os nomes e a organização dos módulos podem ser definidos na implementação.

Ao concluir essas entregas haverá um protótipo desse mecanismo. O registro atual é sua especificação; não é evidência de funcionamento, originalidade ou substituição geral de modelos de linguagem.
