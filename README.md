# S.E.E.D.

Symbolic Empirical Evolution Design — Design de Evolução Empírica e Simbólica.

**Autor:** Giorgio Nicolas de A. Bertino

**Status:** pausado desde 16/10/2026. Proposta de pesquisa em fase conceitual, sem implementação ou resultados experimentais.

**English summary:** Paused since October 2026. An early research proposal exploring a new machine-learning model that learns explicit rules and relations, with the goal of reducing computational cost on tasks comparable to those addressed by LLMs. The algorithm and architecture are still open research questions; no efficiency gains or novel mechanism have been demonstrated. The documents below are in Portuguese.

Projeto de pesquisa em fase conceitual. O objetivo do autor é investigar um novo modelo de aprendizado de máquina, com algoritmo e arquitetura próprios, que aprenda regras e relações e as utilize para resolver problemas, buscando capacidades comparáveis às dos LLMs com menor custo computacional. Formular hipóteses, escolher experimentos e revisar conhecimento são direções propostas para esse modelo. O mecanismo concreto, sua originalidade e a economia ainda precisam ser estabelecidos.

## Documentos

- **[Arquitetura e Algoritmo S.E.E.D. (A + B)](docs/arquitetura-seed-a-b.md): especificação formal (16/10/2026) do Grafo Simbólico Condicionado (Parte A) e da Evolução Guiada pela Topologia do Resíduo sem LLM (Parte B), com referências conferidas em 16/10/2026 e hipótese de contribuição revisada.**
- [Trabalhos relacionados clássicos](docs/trabalhos-relacionados-classicos.md): comparação de cada peça do S.E.E.D. com métodos clássicos (CART, M5, FIMT-DD, boosting, *query by committee*, programação genética, BACON, FORTE e outros), com 20 referências.
- [Direção principal e perguntas sobre o novo modelo](docs/direcao-do-modelo.md): esclarecimento do autor em 15/10/2026, objetivo de menor custo e distinção entre a arquitetura principal e o experimento auxiliar da tabela.
- [Brainstorming original](docs/brainstorming-original.txt): texto inicial preservado, com as hipóteses e promessas da primeira versão.
- [Ideias e decisões de pesquisa](docs/pesquisa.md): evolução da proposta, incluindo o livro como gabarito.
- [Diagnóstico de erros e revisão de hipóteses](docs/diagnostico-e-revisao.md): direção de algoritmo e proposta de um modelo especializado em interpretar falhas e planejar testes.
- [O que já existe e o que investigar](docs/escopo-da-pesquisa.md): distinção entre trabalhos existentes, decisões de implementação e hipóteses de contribuição.
- [Falhas e plano de avaliação](docs/falhas-e-validacao.md): riscos, controles propostos e experimentos para verificar se esses controles funcionam.
- [Como a experiência modifica o aprendiz](docs/mecanismo-de-aprendizado.md): proposta auxiliar v0.1 de política tabular de revisão; não define o novo modelo pretendido.
- [Experimento auxiliar da política de revisão](docs/primeiro-experimento.md): protocolo ainda não executado para avaliar a tabela, sem avaliar substituição ou eficiência geral frente a LLMs.

## Estado atual

Em 16/10/2026 foi definida a [arquitetura A + B do S.E.E.D.](docs/arquitetura-seed-a-b.md): um **Livro de regras simbólicas, cada uma com o domínio onde vale (Parte A)**, revisado por um **ciclo sem LLM que classifica o erro (ruído, termo faltante ou quebra de regime), corrige só a parte necessária e escolhe o próximo experimento (Parte B)**.

A conferência da literatura mostrou que cada peça já tem precedente: divisão em regimes (árvores de decisão simbólicas, 2026), correção de sub-expressões pelo resíduo (RED, 2025), visões aditiva e multiplicativa do resíduo (RISR, 2026, que usa LLM) e escolha ativa de experimentos em regressão simbólica (Medina e White). A hipótese de contribuição é a combinação dessas peças na revisão contínua de um Livro existente, sem LLM e sem regressão do que já foi aprendido. Ela ainda precisa ser demonstrada.

A comparação com métodos clássicos está em [trabalhos relacionados clássicos](docs/trabalhos-relacionados-classicos.md). Ela mostrou que até a revisão contínua com modelos locais tem precedente (FIMT-DD, 2011).

## Pausa

Em 16/10/2026 o autor pausou o projeto para priorizar outras frentes. As seções 4 e 5 da arquitetura ainda trazem promessas não revistas, como "zero esquecimento" e o benchmark contra redes neurais. Elas devem ser lidas como hipóteses, não como resultados esperados.

Se o projeto for retomado, a direção avaliada como mais promissora é a **revisão contínua e ativa de modelos simbólicos já em uso**: decidir entre ajustar, corrigir, dividir em regimes, coletar mais dados ou recomeçar quando o ambiente muda, sem quebrar o que depende do modelo. Junto com ela, propõe-se um **benchmark aberto de revisão contínua de fórmulas**, que não foi encontrado na literatura até 16/10/2026. O primeiro passo seria um piloto curto, com critérios de abandono definidos antes da execução.

## Como continuar

Registrar ideias e revisões nos documentos e criar commits com descrições claras. Manter o brainstorming original como referência histórica. Anotar resultados de experimentos quando forem executados, incluindo falhas e limitações.

Este repositório reúne a proposta, suas revisões e perguntas em aberto. O brainstorming original é um registro histórico; as afirmações iniciais de garantias universais foram revistas nos documentos atuais.
