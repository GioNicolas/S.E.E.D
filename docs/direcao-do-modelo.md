# Direção principal — um novo modelo de aprendizado de máquina

Registro: 15 de outubro de 2026, após esclarecimento do autor.

Estado: objetivo de pesquisa confirmado; algoritmo, arquitetura e ganhos ainda não demonstrados.

## O que o autor pretende construir

Investigar um novo algoritmo e uma nova arquitetura de aprendizado de máquina, capazes de aprender regras e relações e utilizá-las para resolver problemas, buscando capacidades comparáveis às dos LLMs com menor custo computacional.

A ambição é criar um novo tipo de modelo. A hipótese é que um núcleo de aprendizado e resolução de problemas baseado em regras, relações e experimentação possa oferecer uma alternativa ao mecanismo de previsão de próximos tokens. A representação, a implementação e o alcance dessa alternativa ainda precisam ser definidos.

O S.E.E.D. pretende ser o próprio modelo que aprende e resolve problemas. O livro, os experimentos e o diagnóstico de erros são propostas de componentes desse processo. O papel de modelos de linguagem auxiliares e o formato de entrada e saída continuam em aberto.

O caráter revolucionário expressa a ambição do projeto. Originalidade, capacidade e economia precisam ser sustentadas por definição do mecanismo, comparação com trabalhos existentes e experimentos.

## Pergunta central

Como representar, aprender e combinar conhecimento de uma maneira que produza respostas úteis com menos computação?

## Perguntas a responder sobre o modelo

1. **Representação:** em que estruturas computáveis o conhecimento ficará armazenado? Regras, relações, programas ou outra representação? Como expressar condições, incerteza e exceções?
2. **Aprendizado:** como o modelo transforma observações, exemplos ou textos em conhecimento? Qual algoritmo cria, seleciona e modifica essas estruturas?
3. **Mudança causada pela experiência:** depois que uma hipótese falha por omitir uma variável, o que muda no aprendiz? Parâmetros, operações de revisão, procedimentos armazenados ou uma combinação? Como essa mudança altera decisões futuras?
4. **Descoberta:** como gerar uma regra ou operação que ainda não esteja pronta na biblioteca inicial? O que é fornecido por nós e o que é efetivamente aprendido?
5. **Resolução de problemas:** como combinar conhecimento aprendido para responder a uma situação nova? Como limitar o custo dessa busca ou inferência?
6. **Experimentação e correção:** como escolher testes, distinguir explicações concorrentes e revisar conhecimento sem transformar uma hipótese em sua própria confirmação?
7. **Entrada e saída:** como o modelo recebe uma pergunta e produz uma resposta? Como essa interface se relaciona com o núcleo que aprende e resolve problemas?
8. **Generalização:** que tarefas e capacidades queremos alcançar primeiro? Como medir transferência para problemas e famílias que não fizeram parte do treinamento?

Essas perguntas não ficam resolvidas pela existência de um livro de regras ou por registrar episódios. Precisamos especificar como o estado do modelo é atualizado e como ele é usado na execução.

## Perguntas a responder sobre custo e avaliação

1. A economia pretendida está no **treinamento**, no **uso do modelo (inferência)** ou em ambos? O autor ainda não escolheu uma prioridade.
2. Qual modelo e qual conjunto de tarefas serão a referência de comparação?
3. Como comparar custo mantendo qualidade e dificuldade das tarefas comparáveis?
4. Que recursos serão medidos: tempo, memória, operações computacionais, energia ou custo financeiro? Quais conseguimos medir com o equipamento disponível?
5. O custo total inclui coleta de dados, experimentos, verificadores, busca, armazenamento e eventuais modelos auxiliares?
6. Como avaliar qualidade das respostas, generalização e alcance das tarefas junto da economia?
7. Em quais condições o método deixa de ser econômico ou perde capacidade? O que contaria como resultado negativo?

Menor custo é uma hipótese a testar. Escolher uma representação simbólica ou deixar de usar previsão de tokens no núcleo não demonstra, por si só, economia ou capacidade comparável à de um LLM.

## Papel da proposta anterior da tabela

A [proposta tabular v0.1](mecanismo-de-aprendizado.md) foi sugerida pelo assistente como mecanismo simples para aprender a ordem de operações fixas de revisão. Ela pode servir como experimento auxiliar ou referência de comparação.

Essa proposta estreitou demais a intenção do autor quando foi apresentada como definição inicial do S.E.E.D. Aprender a ordenar quatro operações não atende, por si só, à ambição de criar um novo tipo de modelo. O protocolo associado não avalia substituição de LLMs nem o objetivo geral de menor custo.

Os documentos anteriores serão preservados como histórico e hipótese auxiliar. A implementação da tabela não é um próximo passo obrigatório, e suas restrições não definem a arquitetura principal. A prioridade de pesquisa deve ser escolhida em função das perguntas acima e da hipótese central do novo modelo.

## O que está decidido e o avanço de 16/10/2026

**Decidido (15/10/2026):** a direção principal é investigar um novo modelo de ML, com algoritmo e arquitetura próprios, aprendizado de regras e relações e objetivo de eficiência computacional frente a capacidades selecionadas dos LLMs.

**Formalizado em 16/10/2026 ([Arquitetura e Algoritmo S.E.E.D. A + B](arquitetura-seed-a-b.md)):**
- **Representação (Parte A):** Grafo Simbólico Condicionado ($\mathcal{G}_t$), composto por módulos algébricos reutilizáveis protegidos por guardas explícitas de domínio ($P_k(\mathbf{x})$), envelopes de incerteza ($\sigma_k$) e âncoras de regressão ($\mathcal{L}_k$).
- **Algoritmo de Aprendizado (Parte B):** Evolução Estrutural Guiada pela Topologia do Resíduo (separando anomalias isoladas, desvios algébricos aditivos/multiplicativos e quebras de regime/fronteira) acoplada a Sondagem Ativa Discriminante — **operando sem LLM no loop**.
- **Consequência pretendida (Parte C):** revisar continuamente livros imperfeitos, preservando o que já funcionava e extrapolando fora da faixa observada. São hipóteses: as âncoras só protegem os casos armazenados, e uma fórmula explícita pode extrapolar mal.

**Revisão de 16/10/2026:** a conferência da literatura mostrou que cada peça já tem precedente (ver a seção 1 da [arquitetura](arquitetura-seed-a-b.md) e os [trabalhos relacionados clássicos](trabalhos-relacionados-classicos.md)). O projeto foi pausado; o [README](../README.md) registra a direção sugerida caso seja retomado.
