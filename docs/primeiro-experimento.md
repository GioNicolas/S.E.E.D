# Experimento auxiliar — política de revisão, ainda não executado

Atualização: 15 de outubro de 2026.

**Escopo após esclarecimento do autor:** este protocolo é uma opção auxiliar, não o próximo passo obrigatório do projeto. Ele mede a priorização de operações fixas e não avalia a criação de um novo modelo ou menor custo frente a LLMs. A [direção principal](direcao-do-modelo.md) registra a ambição do modelo e as perguntas ainda em aberto.

Este protocolo avalia a [proposta de mecanismo v0.1](mecanismo-de-aprendizado.md). Os números abaixo são escolhas iniciais para implementação e validação. Congelar uma configuração versionada antes de gerar ou inspecionar os resultados finais; qualquer ajuste posterior exige outro conjunto final.

## Pergunta e hipótese

Uma política que aprende com tentativas anteriores consegue priorizar revisões úteis em tarefas novas, reduzindo o número de candidatas avaliadas sem aumentar conclusões falsas?

Comparar a tabela treinada com a mesma arquitetura de tabela zerada. A hipótese é que o contexto do erro permita reutilizar preferências entre operações. A possibilidade de a tabela não melhorar, ou de uma heurística fixa ser suficiente, faz parte da avaliação.

A versão anterior deste documento perguntava se treinar com regras conhecidas como gabarito melhorava a descoberta. Nenhum experimento foi executado. Essa pergunta fica como direção futura; o estudo atual isola o aprendizado da ordem de revisão e não treina um gerador de expressões.

## 1. Ambiente e espaço de hipóteses

Usar funções sintéticas de duas variáveis, com `x` e `z` entre `0,5` e `4`, inicialmente sem ruído. O aprendiz recebe observações, uma hipótese inicial `k*x` e acesso limitado ao ambiente. As quatro revisões permitidas são:

- `k*x + b`;
- `k*x + b*z`;
- `k*x*z`;
- `k*x/z`.

O gerador de tarefas guarda a fórmula-alvo separadamente. Usar coeficientes com módulo entre `0,5` e `2` e sinais sorteados; excluir degenerações que tornem as intervenções constantes. As famílias são balanceadas no treino e na validação. Ajustar coeficientes por mínimos quadrados somente nos dados de ajuste.

Incluir controles com `y = k*x`, que já cabe na hipótese inicial, e regras fora da biblioteca, como `y = k*x*z*z`. Os controles verificam revisão desnecessária e aceitação indevida. Essa seleção é um benchmark limitado; os operadores já contêm as estruturas das respostas suportadas.

## 2. Dados acessíveis e respostas reservadas

Separar três usos de dados:

| Conjunto | Acesso do aprendiz | Uso |
| --- | --- | --- |
| Ajuste e diagnóstico | Sim | Ajustar coeficientes e calcular assinatura de erro |
| Confirmação local | Sim, mediante consultas debitadas do orçamento | Testar previsões e produzir o retorno das operações |
| Avaliação final da tarefa | Não | Medir erro e falsas confirmações após a saída do aprendiz |

Começar com 8 observações sorteadas para ajuste e 8 intervenções: fixar `x=1` e variar `z` em `0,5`, `1`, `2`, `4`; depois fixar `z=1` e variar `x` nos mesmos valores. A repetição do ponto `(1,1)` continua contando como consulta. Reajustar a hipótese inicial e ajustar cada candidata nesses 16 pontos, sem adicionar dados de confirmação ao ajuste na v0.1.

Por tarefa, preparar um fluxo de 20 pontos de confirmação, em blocos de 4 combinações novas sorteadas no mesmo domínio. O fluxo independe da operação escolhida e usa uma semente comum às políticas comparadas. Revelar cada bloco somente após registrar as previsões correspondentes.

Testar primeiro a hipótese inicial em um bloco. Se falhar, cada candidata recebe o próximo bloco; verificar também todos os blocos de confirmação já revelados. Ao reavaliar um bloco já consultado, usar os resultados registrados, sem cobrança duplicada. Esse compartilhamento evita repetir consultas, mas a contagem de avaliações numéricas deve permanecer separada.

Limites máximos: 4 candidatas e 36 consultas ao ambiente, incluindo as 16 iniciais, as 4 da hipótese inicial e até 16 das revisões. Relatar também orçamentos de 1 e 2 candidatas. As políticas recebem os mesmos limites e o mesmo fluxo de pontos por tarefa; parar cedo pode reduzir o consumo efetivo.

O avaliador prepara mais 64 pontos reservados por tarefa, nunca expostos ao aprendiz, à assinatura ou à política. Essa avaliação não integra o orçamento acessível e seu custo será relatado à parte. A equação-alvo e esses alvos não entram na memória de treinamento da política v0.1.

## 3. Verificação e aprendizado

Definir erro relativo normalizado por `erro = raiz(media((observado-previsto)^2)) / max(1e-12, raiz(media(observado^2)))`. Divisão fora do domínio, saída não finita ou ajuste inválido reprovam a candidata.

Uma candidata é localmente confirmada se o erro for no máximo `1e-6` em todos os blocos de confirmação revelados; usar o máximo dos erros por bloco. Cada candidata recebe pelo menos um bloco novo. A hipótese inicial usa o mesmo critério. Dados finitos não asseguram correção fora dessas observações.

Durante o treino, guardar `r=1` para uma candidata confirmada e `r=0` para uma tentativa reprovada. Ao fim da tarefa, atualizar as médias da tabela para as operações efetivamente tentadas conforme a regra do [mecanismo](mecanismo-de-aprendizado.md). Não atribuir resultado às não tentadas. Uma hipótese inicial confirmada não atualiza operações que não foram usadas.

Se uma candidata passar, retornar sua expressão, coeficientes, domínio e evidências e parar. Se esgotar a busca, retornar abstenção e o histórico. Reportar a expressão como confirmada localmente, sem declarar causa única ou lei universal. Este protocolo não garante que todas as alternativas compatíveis tenham sido enumeradas; testes de ambiguidade ficam para a fase seguinte.

## 4. Divisão entre treino, validação e teste

Proposta de escala inicial por semente de treinamento:

- Treino: 400 tarefas suportadas, 100 por família, em ordem embaralhada.
- Validação: 80 suportadas, mais 20 controles de hipótese inicial correta e 20 fora da biblioteca.
- Teste final: 200 suportadas, mais 50 controles de cada tipo.
- Repetir o treinamento com 10 sementes e comparar políticas nas mesmas tarefas finais pareadas. Manter sementes de treino, validação e teste distintas e registrar os identificadores.

As instâncias usam combinações novas de coeficientes e pontos, sem copiar tarefas entre divisões. Embaralhar nomes e posição das variáveis em todas as divisões, mantendo os papéis da hipótese acessíveis. Congelar a tabela e restaurar o mesmo checkpoint antes de cada tarefa final; não aprender com a sequência de testes.

Isso mede reutilização entre instâncias novas de famílias conhecidas. Não chamar esse resultado de transferência para famílias desconhecidas. Reservar famílias inteiras e permitir composições novas será um estudo separado, que exige ampliar a linguagem e os comparadores.

## 5. Comparações

Todas as políticas usam a mesma biblioteca, ajuste, dados iniciais, assinatura e verificador:

1. Tabela treinada por contexto de erro, congelada no teste.
2. Tabela zerada, com as mesmas regras de seleção e desempate usadas no teste da política treinada.
3. Ordem aleatória sem reposição.
4. Ordem fixa escolhida na validação, sem contexto.
5. Tabela treinada globalmente, sem contexto, para verificar se basta aprender a frequência das operações.
6. Enumeração completa como referência de cobertura e custo.

A tabela zerada com desempate aleatório equivale à ordem aleatória nesta versão; apresentar essa equivalência, sem contá-las como evidências independentes. Uma heurística fixa baseada nos resíduos, definida e congelada na validação, é uma comparação adicional recomendada antes de reivindicar utilidade da política contextual.

Nos orçamentos de 1, 2 e 4 candidatas, aplicar o mesmo limite a todos os métodos. Usar enumeração completa também como teto empírico de cobertura da biblioteca, sem tratá-la como método de custo igual ao de uma tentativa.

## 6. Métricas e critério de sucesso

Relatar:

- Número de candidatas e consultas até confirmação; falhas consomem o orçamento inteiro na métrica de custo limitado, sem omiti-las da média.
- Taxa de tarefas que produzem uma regra adequada nos 64 pontos reservados, sob cada orçamento.
- Erro nos pontos reservados, separado por família e tipo de controle.
- Falsas confirmações: passou localmente, mas excedeu `1e-6` no conjunto reservado. Relatar proporção entre saídas confirmadas e incidência sobre todas as tarefas; usar a incidência no critério abaixo. Sem saídas confirmadas, a primeira proporção fica indisponível, não zero.
- Abstenções, revisões desnecessárias, tempo e avaliações numéricas.
- Distribuição de contextos, contagens por operação e mudanças na tabela.

Critério exploratório a congelar: redução de pelo menos 10% no número médio de candidatas, com orçamento máximo de 4, frente à política zerada, sem perder mais de 2 pontos percentuais de recuperação de regras adequadas e sem aumentar em mais de 2 pontos percentuais a taxa de falsas confirmações. Relatar diferenças pareadas e intervalos de incerteza; esses limites são escolhas de projeto, não padrões universais.

O ganho precisa ser confrontado também com ordem fixa e preferência global. Se o intervalo não permitir distinguir melhora de empate ou dano, a conclusão será inconclusiva. Controles de integridade devem passar antes da avaliação; métricas positivas com vazamento, mudança do ambiente ou atualização entre tarefas finais invalidam o resultado.

## 7. Integridade e próximas fases

Antes da execução final, verificar atualização numérica e persistência da tabela, repetibilidade por semente, ausência de acesso às respostas reservadas, estabilidade do ambiente ao mudar a hipótese e tratamento de correlações indisponíveis. Salvar configuração, versão do código, checkpoint e episódios, incluindo tentativas fracassadas.

Este estudo isola uma política de ordenação de revisões. Depois, avaliar ruído, livros imperfeitos, defeitos do ambiente, escolha aprendida de intervenções e operações compostas, uma ampliação por vez, seguindo o [plano de falhas](falhas-e-validacao.md).

Nenhum resultado foi produzido. Um resultado positivo apoiará esse mecanismo neste benchmark; não estabelecerá originalidade, descoberta universal ou substituição geral de LLMs.
