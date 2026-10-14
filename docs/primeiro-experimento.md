# Primeiro experimento — proposta, ainda não executada

## Pergunta

O treinamento com regras conhecidas como gabarito melhora a capacidade de formular regras corretas e fazer previsões em problemas novos?

## Ambiente inicial sugerido

Um ambiente numérico simples de movimento, sem interface gráfica obrigatória. O aprendiz observa variáveis como força, massa, velocidade e tempo, dentro de condições explicitamente definidas. A implementação do ambiente e o livro do aprendiz ficam separados.

Começar com descoberta de relações simples. Depois incluir regras com condições de aplicação e defeitos sistemáticos, para avaliar os demais mecanismos do S.E.E.D.

## Protocolo a detalhar antes da execução

1. Definir a linguagem de hipóteses e o conjunto de regras do ambiente.
2. Separar problemas de treinamento, validação e avaliação final. Reservar famílias de regras para avaliar transferência; reservar também condições novas dentro de famílias conhecidas.
3. Apresentar observações e um orçamento limitado de consultas ao ambiente.
4. Solicitar hipótese, condições de aplicação e previsões para casos não observados.
5. Usar o livro como gabarito no treinamento, sem expor as respostas da avaliação final ao aprendiz.
6. Comparar o sistema treinado com uma versão equivalente sem esse treinamento, usando o mesmo orçamento de experimentos.

## O que medir

- Erro de previsão em casos não observados.
- Recuperação de regras equivalentes ao gabarito, quando a equivalência puder ser verificada.
- Número de experimentos até obter uma hipótese adequada.
- Desempenho em famílias de regras que ficaram fora do treino.
- Nas etapas posteriores, aceitação de bugs como regras e identificação de condições ausentes.

## Limites da conclusão

Um resultado positivo apoia a abordagem no domínio e no protocolo avaliados. Não demonstra descoberta universal, imunidade a alucinações nem validade física fora do simulador.

Definir critérios de sucesso, sementes aleatórias, orçamento e baselines antes de executar. Registrar resultados negativos e preservar a avaliação final contra ajustes feitos após observar suas respostas.
