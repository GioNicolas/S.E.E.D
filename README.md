# S.E.E.D.

Symbolic Empirical Evolution Design — Design de Evolução Empírica e Simbólica.

**Autor:** Giorgio Nicolas de A. Bertino

**Status:** proposta de pesquisa em fase conceitual, sem implementação ou resultados experimentais.

**English summary:** An early research proposal exploring a new machine-learning model that learns explicit rules and relations, with the goal of reducing computational cost on tasks comparable to those addressed by LLMs. The algorithm and architecture are still open research questions; no efficiency gains or novel mechanism have been demonstrated. The documents below are in Portuguese.

Projeto de pesquisa em fase conceitual. O objetivo do autor é investigar um novo modelo de aprendizado de máquina, com algoritmo e arquitetura próprios, que aprenda regras e relações e as utilize para resolver problemas, buscando capacidades comparáveis às dos LLMs com menor custo computacional. Formular hipóteses, escolher experimentos e revisar conhecimento são direções propostas para esse modelo. O mecanismo concreto, sua originalidade e a economia ainda precisam ser estabelecidos.

## Documentos

- [Direção principal e perguntas sobre o novo modelo](docs/direcao-do-modelo.md): esclarecimento do autor em 15/10/2026, objetivo de menor custo e distinção entre a arquitetura principal e o experimento auxiliar da tabela.
- [Brainstorming original](docs/brainstorming-original.txt): texto inicial preservado, com as hipóteses e promessas da primeira versão.
- [Ideias e decisões de pesquisa](docs/pesquisa.md): evolução da proposta, incluindo o livro como gabarito.
- [Diagnóstico de erros e revisão de hipóteses](docs/diagnostico-e-revisao.md): direção de algoritmo e proposta de um modelo especializado em interpretar falhas e planejar testes.
- [O que já existe e o que investigar](docs/escopo-da-pesquisa.md): distinção entre trabalhos existentes, decisões de implementação e hipóteses de contribuição.
- [Falhas e plano de avaliação](docs/falhas-e-validacao.md): riscos, controles propostos e experimentos para verificar se esses controles funcionam.
- [Como a experiência modifica o aprendiz](docs/mecanismo-de-aprendizado.md): proposta auxiliar v0.1 de política tabular de revisão; não define o novo modelo pretendido.
- [Experimento auxiliar da política de revisão](docs/primeiro-experimento.md): protocolo ainda não executado para avaliar a tabela, sem avaliar substituição ou eficiência geral frente a LLMs.

## Estado atual

Ainda não há algoritmo implementado, treinamento realizado ou resultados experimentais. As afirmações de imunidade a alucinações e estabilidade absoluta do brainstorming original não são garantias demonstradas.

Em 15/10/2026 foi registrada uma proposta tabular v0.1 para priorizar operações de revisão. Depois, o autor esclareceu que a ambição principal é criar um novo tipo de modelo de ML. A tabela fica como hipótese auxiliar e referência possível, sem determinar a arquitetura ou o próximo passo obrigatório. As perguntas sobre representação, aprendizado, generalização e custos de treinamento e inferência estão no documento de direção principal.

## Como continuar

Registrar ideias e revisões nos documentos e criar commits com descrições claras. Manter o brainstorming original como referência histórica. Anotar resultados de experimentos quando forem executados, incluindo falhas e limitações.

Este repositório reúne a proposta, suas revisões e perguntas em aberto. O brainstorming original é um registro histórico; as afirmações iniciais de garantias universais foram revistas nos documentos atuais.
