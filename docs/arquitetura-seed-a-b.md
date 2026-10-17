# Arquitetura e Algoritmo S.E.E.D. (A + B)
## Evolução de Grafos Simbólicos Condicionados Guiada pela Topologia do Resíduo (Sem LLM)

**Registro:** 16 de outubro de 2026  
**Autor:** Giorgio Nicolas de A. Bertino  
**Status:** Especificação formal da arquitetura principal e do algoritmo de aprendizado (fase pré-implementação do núcleo A + B). Projeto pausado em 16/10/2026. A seção 1 foi revista com referências conferidas; as seções 4 e 5 ainda trazem promessas não revistas e devem ser lidas como hipóteses.

---

## 1. Posicionamento frente à literatura

*Referências conferidas nas páginas oficiais em 16/10/2026. A comparação com métodos clássicos (árvores de regressão, aprendizado ativo, boosting, regressão simbólica tradicional) fica em [trabalhos relacionados clássicos](trabalhos-relacionados-classicos.md).*

Cada peça do S.E.E.D., isoladamente, já tem um precedente próximo. A tabela lista os trabalhos mais parecidos encontrados.

| Peça do S.E.E.D. | Trabalho mais próximo | O que ele já faz | Diferença proposta pelo S.E.E.D. |
| :--- | :--- | :--- | :--- |
| Livro de regras executáveis, revisado pela interação com o ambiente | *WorldCoder* — Tang, Key e Ellis, NeurIPS 2024 ([arXiv:2402.12275](https://arxiv.org/abs/2402.12275)); *Code World Models for General Game Playing* — Lehrach et al., 2025 ([arXiv:2510.04542](https://arxiv.org/abs/2510.04542)) | Um agente escreve e corrige um modelo do ambiente em código, com boa eficiência amostral e reaproveitamento de conhecimento. | Nesses trabalhos, uma LLM escreve e corrige o código. O S.E.E.D. propõe revisar com operadores numéricos, sem LLM. |
| Regras separadas por regime (guardas $P_k$) | *Learning regime-dependent governing equations: A symbolic decision tree approach* — Mitrai, Liu e Sanoja, 2026 ([arXiv:2605.24275](https://arxiv.org/abs/2605.24275)); *PS-Tree* (árvore de regressão simbólica por partes), citada na [análise da competição de regressão simbólica de 2022](https://arxiv.org/abs/2304.01117) | Aprendem juntas as condições que dividem o domínio e a equação de cada regime, sem LLM. | Esses métodos aprendem em lote, num conjunto de dados fixo. O S.E.E.D. propõe criar um regime novo só quando uma regra existente começa a falhar, sem refazer as demais. |
| Correção cirúrgica de uma sub-expressão pelo resíduo ($\Delta f$) | *RED: Prompting Neural-Guided Equation Discovery Based on Residuals* — Brugger et al., Discovery Science 2025 ([arXiv:2511.05586](https://arxiv.org/abs/2511.05586)) | Calcula o resíduo de cada sub-expressão, usa esse resíduo como novo alvo e troca apenas a sub-expressão que melhora a validação. Funciona inclusive com programação genética, sem LLM. | **Muito próximo do Estágio 2.** A diferença proposta é decidir antes qual tipo de correção aplicar (ruído, termo faltante ou quebra de regime). |
| Visões aditiva e multiplicativa do resíduo ($r$ e $\rho$) | *RISR* — Li et al., 2026 ([arXiv:2610.11387](https://arxiv.org/abs/2610.11387)) | Usa o resíduo aditivo e o multiplicativo regularizado para prever quais correções valem a pena. **Usa um modelo de linguagem para propor as fórmulas.** | O $\rho$ do Estágio 1 é praticamente o mesmo do RISR. A diferença proposta é usar essas visões sem LLM, como classificador explícito do tipo de falha. |
| Edição local de árvores de expressão | *FunctionEvolve* — Xia, Zhu e Yan, 2026 ([arXiv:2606.07704](https://arxiv.org/abs/2606.07704)) | Edições locais preservam sub-expressões úteis; uma LLM propõe as mutações. | Mesma ideia de preservar o que funciona, mas com mutações sem LLM. O resumo do artigo não menciona sondagem ativa. |
| Sondagem ativa (experimento onde as hipóteses discordam) | *Active Learning in Symbolic Regression with Physical Constraints* — Medina, White et al. ([arXiv:2305.10379](https://arxiv.org/abs/2305.10379)) | Usa as equações da fronteira de Pareto como "comitê" e escolhe o próximo experimento onde elas mais discordam (*query by committee*). | Já existe para regressão simbólica. O S.E.E.D. acrescentaria a busca binária na fronteira de regime. |
| Diversidade de resíduos | *DRSR* — Ikeda, Nomura e Hamano, GECCO 2026 ([arXiv:2605.15809](https://arxiv.org/abs/2605.15809)) | Devolve várias expressões que distribuem o erro de formas diferentes, com foco em pontos atípicos. | Relaciona-se ao tratamento de ruído (Tipo I), não à correção por resíduo. |

**Correções em relação à versão anterior desta seção:**
- O RISR usa modelo de linguagem; a versão anterior dizia que ele funcionava sem LLM.
- O DRSR trata de diversidade de expressões e pontos atípicos, não de correção guiada pelo resíduo.
- O resumo do FunctionEvolve não menciona sondagem ativa.
- O *CoT-SR* (2026) não foi encontrado e foi removido.
- Divisão em regimes, correção pelo resíduo e sondagem ativa já existem separadamente; a versão anterior apresentava essas lacunas como abertas.

### Hipótese de contribuição

Nenhuma peça é inédita isoladamente. A hipótese a testar é que a **combinação**, num cenário específico, traga ganhos que as peças separadas não trazem:

1. **Revisão contínua de um Livro existente.** Os trabalhos acima aprendem a partir de um conjunto de dados fixo. O S.E.E.D. propõe receber dados em sequência e corrigir apenas o que falhou.
2. **Diagnóstico antes da edição.** Classificar a falha (ruído, termo faltante ou quebra de regime) e aplicar a operação correspondente, em vez de experimentar todas as mutações possíveis.
3. **Sem LLM no ciclo de revisão.** Todas as etapas usam operações numéricas baratas.
4. **Proteção contra regressão.** Toda mudança precisa continuar acertando as âncoras $\mathcal{L}_k$.

Isso precisa ser demonstrado com experimentos. Vencer uma rede neural em dados gerados por fórmulas é o resultado esperado e, sozinho, não convence. As comparações principais devem incluir os métodos desta tabela (árvore de decisão simbólica, PS-Tree, regressão simbólica com RED e o aprendizado ativo de Medina e White), adaptados ao cenário sequencial. A seção 5 ainda precisa ser revista com essas comparações.

---

## 2. Parte A — Arquitetura de Representação: O Grafo Simbólico Condicionado ($\mathcal{G}$)

Em vez de armazenar o conhecimento do mundo em matrizes densas de ponto flutuante ($W \in \mathbb{R}^{d \times d}$), o estado de conhecimento do S.E.E.D. em qualquer instante $t$ é um **Grafo Acíclico Dirigido de Módulos Simbólicos Condicionados** (o "Livro"):

$$\mathcal{G}_t = \left\{ \mathcal{M}_1, \mathcal{M}_2, \dots, \mathcal{M}_K \right\}$$

Cada módulo (regra compilada) $\mathcal{M}_k$ é uma tupla explícita e executável:

$$\mathcal{M}_k = \left( P_k(\mathbf{x}), \; f_k(\mathbf{x}; \boldsymbol{\theta}_k, \mathcal{G}_t), \; \sigma_k, \; \mathcal{L}_k \right)$$

1. **Guarda de Domínio / Condição de Contorno ($P_k(\mathbf{x}) \in \{0, 1\}$):**
   Define explicitamente **onde e quando** a regra é válida (por exemplo, $x_1 \le \tau$, ou uma invariante relacional). Permite que leis diferentes coexistam para regimes diferentes (ex.: regime linear vs. regime de saturação/turbulência) sem que uma destrua os parâmetros da outra.
2. **Lei Funcional Componível ($f_k$):**
   Uma árvore de expressão simbólica enxuta composta por operadores primitivos ($\{+,-,\times,\div,\text{potências},\dots\}$) **e chamadas a outros módulos já verificados no Livro** ($\mathcal{M}_j \in \mathcal{G}_t$). Isso permite **abstração hierárquica**: uma vez descoberta a regra da aceleração, ela vira um bloco atômico reutilizável para descobrir a regra da energia ou do atrito.
3. **Envelope de Incerteza Empírica ($\sigma_k$):**
   Limiar de erro residual esperado dentro do domínio $P_k(\mathbf{x}) = 1$, calibrado nas observações de validação.
4. **Linhagem e Âncoras de Regressão ($\mathcal{L}_k$):**
   Conjunto compacto de pontos-chave verificados (*canonical anchors*) que garantem que nenhuma atualização futura quebre silenciosamente o módulo $\mathcal{M}_k$.

### Por que a Parte A reduz drasticamente o custo de inferência?
* **Em uma LLM / Rede Densa:** Responder a uma consulta exige multiplicar todas as camadas densas ($O(N_{\text{params}})$ operações de ponto flutuante por token).
* **No S.E.E.D. ($\mathcal{G}_t$):** A inferência consiste em avaliar a árvore de guardas de domínio $P_k(\mathbf{x})$ (custo $O(\log K)$ ou $O(K)$ com $K$ pequeno) e executar diretamente a expressão algébrica compilada $f_k(\mathbf{x})$ (dezenas de FLOPs em vez de bilhões).

---

## 3. Parte B — O Algoritmo de Aprendizado: Evolução Guiada pela Topologia do Resíduo

Quando o agente observa dados ou interage com o ambiente obtendo pares $(\mathbf{x}_i, y_i)$, ele avalia a previsão atual do Livro $\hat{y}_i = \mathcal{G}_t(\mathbf{x}_i)$. Se o erro excede o envelope $\sigma_k$, o algoritmo **não usa backpropagation** nem chama uma LLM. Ele executa um ciclo matemático de 4 estágios sobre a estrutura do erro:

```
┌─────────────────────────────────────────────────────────────────────────┐
│ 1. CAMPO DE RESÍDUOS E DECOMPOSIÇÃO TOPOLÓGICA                          │
│    Calcula r(x) = y - ŷ  e  ρ(x) = y / ŷ                                │
│    Classifica a geometria da falha:                                     │
│      • Tipo I:   Anomalia Isolada / Ruído não-reprodutível              │
│      • Tipo II:  Desvio Algébrico Global (aditivo, multiplicativo, z)   │
│      • Tipo III: Quebra de Regime / Fronteira Condicional (P_k divide)  │
└───────────────────────────────────┬─────────────────────────────────────┘
                                    │
                                    ▼
┌─────────────────────────────────────────────────────────────────────────┐
│ 2. SÍNTESE DE MUTAÇÕES CIRÚRGICAS NO GRAFO (Sem busca cega)             │
│    • Se Tipo II  → Ajusta sub-árvore corretiva Δf direto no resíduo r   │
│                    ou razão ρ: f_novo = f_antigo ⊕ Δf(x, z)             │
│    • Se Tipo III → Detecta ponto de dobra τ* (Change-Point / CART)      │
│                    Mantém f_antigo em P_1(x ≤ τ*) e induz f_2 em P_2    │
└───────────────────────────────────┬─────────────────────────────────────┘
                                    │
                                    ▼
┌─────────────────────────────────────────────────────────────────────────┐
│ 3. SONDAGEM ATIVA DISCRIMINANTE (Active Falsification Probing)          │
│    Escolhe matematicamente o próximo experimento x* no ambiente:        │
│    • Máxima Divergência entre candidatas: argmax_x Var(ŷ_m(x))          │
│    • Busca Binária na Fronteira de Regime: x* próximo a τ*              │
└───────────────────────────────────┬─────────────────────────────────────┘
                                    │
                                    ▼
┌─────────────────────────────────────────────────────────────────────────┐
│ 4. VERIFICAÇÃO EM STAGING E MERGE SEM ESQUECIMENTO                      │
│    Testa candidata nos dados novos + Âncoras de Regressão (L_k).        │
│    Promove para o Livro Principal G_{t+1} apenas se erro ≤ σ e 0 quebra.│
└─────────────────────────────────────────────────────────────────────────┘
```

### Detalhamento Matemático dos 4 Estágios

#### Estágio 1: Decomposição Topológica do Campo de Resíduos
Dado um conjunto de observações onde o módulo ativo $\mathcal{M}_k$ foi acionado, definimos:
* **Resíduo Aditivo:** $r(\mathbf{x}) = y(\mathbf{x}) - f_k(\mathbf{x})$
* **Resíduo Multiplicativo (Razão Regularizada):** $\rho(\mathbf{x}) = \frac{y(\mathbf{x})}{f_k(\mathbf{x}) + \operatorname{sign}(f_k)\epsilon}$

O **Operador de Diagnóstico Topológico** calcula três métricas estruturais rápidas (custo $O(N \log N)$):
1. **Índice de Bimodalidade / Particionamento Espacial ($I_{\text{split}}$):**
   Verifica se existe um hiperplano ou limiar $\tau$ em alguma variável $x_d$ tal que, de um lado ($x_d \le \tau$), o erro é nulo ($|r(\mathbf{x})| \le \sigma_k$) e, do outro ($x_d > \tau$), o erro cresce sistematicamente ($|r(\mathbf{x})| \gg \sigma_k$).
   * Se $I_{\text{split}}$ for alto $\rightarrow$ **Diagnóstico Tipo III (Quebra de Regime / Exceção de Domínio)**. A regra atual não está errada; seu domínio $P_k(\mathbf{x})$ é que era amplo demais!
2. **Coerência Aditiva vs. Proporcional ($I_{\text{add}}$ vs. $I_{\text{mult}}$):**
   Se o erro ocorre em todo o domínio, compara a variância explicada (via correlação de postos / informação mútua e ajuste esparso) de $r(\mathbf{x})$ versus $\rho(\mathbf{x})$ contra as variáveis observáveis $\mathbf{x}$ (incluindo variáveis omitidas $z$ da hipótese original):
   * Se $r(\mathbf{x})$ apresenta dependência funcional limpa com um subconjunto de variáveis $\mathbf{x}_S$ enquanto $\rho(\mathbf{x})$ é ruidoso $\rightarrow$ **Diagnóstico Tipo II-A (Correção Aditiva):** a lei verdadeira tem a forma $f_k(\mathbf{x}) + \Delta f(\mathbf{x}_S)$.
   * Se $\rho(\mathbf{x})$ apresenta dependência funcional limpa com $\mathbf{x}_S$ (ou é constante $\neq 1$) $\rightarrow$ **Diagnóstico Tipo II-B (Correção Multiplicativa/Escalar):** a lei verdadeira tem a forma $f_k(\mathbf{x}) \cdot \Delta g(\mathbf{x}_S)$.
3. **Teste de Repetibilidade Local ($I_{\text{noise}}$):**
   Se a discrepância ocorre em pontos isolados sem coerência espacial e não se repete ao reconsultar a vizinhança $\rightarrow$ **Diagnóstico Tipo I (Ruído / Bug Transiente)**. É isolado no log de anomalias sem alterar $\mathcal{G}_t$.

#### Estágio 2: Síntese Cirúrgica de Mutações no Grafo
Em vez de buscar uma equação inteira do zero (como na programação genética tradicional, onde o espaço de busca explode exponencialmente), o S.E.E.D. **congela a estrutura que já funciona** ($f_k$) e sintetiza apenas o **operador corretivo** $\Delta f$ diretamente sobre o alvo residual ($r$ ou $\rho$):
* **Redução de Complexidade:** Se a lei completa tem profundidade 5, mas o Livro já conhece uma aproximação de profundidade 3 ($f_k$), o S.E.E.D. só precisa buscar uma sub-árvore de profundidade 1 ou 2 ($\Delta f$) que zere o resíduo!
* **Bifurcação de Domínio (Quando $I_{\text{split}}$ é acionado):** O módulo $\mathcal{M}_k$ tem sua guarda restringida para $P_k^{\text{novo}}(\mathbf{x}) = P_k(\mathbf{x}) \wedge (x_d \le \hat{\tau})$, preservando 100% de precisão no regime antigo, e um novo módulo irmão $\mathcal{M}_{K+1}$ com guarda $P_{K+1}(\mathbf{x}) = P_k(\mathbf{x}) \wedge (x_d > \hat{\tau})$ é criado para modelar o novo regime.

#### Estágio 3: Sondagem Ativa Discriminante (Active Probing sem LLM)
Quando o Estágio 2 gera $M$ hipóteses estruturais candidatas $\{\mathcal{G}^{(1)}, \dots, \mathcal{G}^{(M)}\}$ que empatam nos poucos dados atuais, o S.E.E.D. calcula ativamente o próximo experimento $\mathbf{x}^*$ a executar no ambiente:

$$\mathbf{x}^* = \arg\max_{\mathbf{x} \in \Omega_{\text{factível}}} \underbrace{\operatorname{Var}_{m \in \{1..M\}} \left( \hat{y}_{\mathcal{G}^{(m)}}(\mathbf{x}) \right)}_{\text{Divergência entre Hipóteses}} + \lambda \underbrace{\mathbb{I}\left(\mathbf{x} \in \partial P_k\right)}_{\text{Refinamento de Fronteira}}$$

* Se a dúvida é entre duas fórmulas (ex.: $k x z$ vs. $k x / z$), ele escolhe o ponto $\mathbf{x}^*$ onde as previsões das duas mais se distanciam.
* Se a dúvida é onde exatamente uma lei quebra (fronteira de regime $\hat{\tau}$), ele faz **busca binária ativa** ao longo da variável de transição, encontrando o limiar exato com $O(\log(1/\delta))$ experimentos.

---

## 4. Consequência C — Revisão de Teoria e Imunidade ao Esquecimento Catastrófico

Porque a arquitetura une **A** (módulos simbólicos isolados por guardas de domínio $P_k$) e **B** (edição cirúrgica via resíduo + validação nas âncoras $\mathcal{L}_k$), o S.E.E.D. resolve naturalmente três problemas onde redes neurais e LLMs falham:

1. **Correção de um "Livro Inicial" Imperfeito:**
   Podemos inicializar $\mathcal{G}_0$ com leis aproximadas ou incompletas (por exemplo, física sem atrito ou regras de negócio simplificadas). Ao operar num ambiente com atrito ou tetos de saturação, o S.E.E.D. não joga o livro fora: ele detecta o resíduo proporcional/aditivo ou a quebra de domínio e **acopla o termo faltante ou a condição de contorno**.
2. **Zero Esquecimento Catastrófico em Mudanças de Regime:**
   Se o ambiente passa a operar num novo regime ($x > 10$), uma rede neural treinada continuamente nos dados novos sobrescreve seus pesos e esquece como prever em $x \le 10$. O S.E.E.D. bifurca a guarda $P_k$ e mantém a regra de $x \le 10$ congelada e protegida pelas âncoras $\mathcal{L}_k$.
3. **Extrapolação Fora da Distribuição (OOD):**
   Como os nós do grafo são relações algébricas e lógicas exatas (e não interpoladores locais por funções de ativação ReLU/Attention), uma regra validada em $x \in [1, 4]$ extrapola com erro zero para $x = 1000$, desde que nenhuma nova fronteira de regime seja cruzada.

---

## 5. Plano Experimental "Davi vs. Golias" (Como provar o valor para Tech Fellow / Premiações)

Para demonstrar de forma irrefutável que o S.E.E.D. (A + B) funciona e entrega o que promete, o protótipo em código deve rodar uma bateria de avaliação comparando **4 famílias de modelos** no mesmo ambiente sintético multi-regime:

1. **S.E.E.D. (A + B Completo):** Grafo Simbólico Condicionado + Diagnóstico Topológico do Resíduo + Sondagem Ativa.
2. **S.E.E.D. Cego (Ablação da Parte B):** Mesma estrutura de Grafo Simbólico, mas trocando o Diagnóstico Topológico do Resíduo por busca/mutação aleatória sem sondagem ativa (prova a contribuição do algoritmo B).
3. **Regressão Simbólica Estática (Ablação da Parte A):** Tenta ajustar uma única equação global sem guardas de domínio/bifurcação de regime (prova a contribuição da arquitetura modular A).
4. **Rede Neural Densa (MLP / Micro-Transformer via Backpropagation):** Treinada com gradiente descendente (Adam) nos mesmos dados e também com 100x mais dados (prova a vantagem do paradigma S.E.E.D. frente ao aprendizado neural padrão).

### As 4 Métricas Decisivas do Benchmark

| Dimensão | O que será medido | Resultado Esperado da Hipótese S.E.E.D. |
| :--- | :--- | :--- |
| **1. Eficiência Amostral (Dados)** | Nº de experimentos no ambiente até atingir erro $< 10^{-4}$ | S.E.E.D. converge com dezenas de pontos via sondagem ativa; Rede Neural exige milhares; Busca Cega estoura orçamento. |
| **2. Custo Computacional (FLOPs)** | Operações de ponto flutuante no treino/descoberta e por consulta na inferência | Inferência do S.E.E.D. custa $< 1\%$ dos FLOPs da rede neural e $0$ chamadas de LLM. |
| **3. Extrapolação OOD** | Erro de previsão quando variáveis saem $10\times$ da faixa vista no treino | Rede Neural degrada drasticamente fora da caixa de treino; S.E.E.D. mantém precisão exata dentro do regime. |
| **4. Retenção Contínua (Anti-Esquecimento)** | Erro no Regime 1 após o modelo aprender sequencialmente os Regimes 2 e 3 | Rede Neural sofre esquecimento catastrófico no Regime 1; S.E.E.D. mantém **0,0% de degradação** graças às guardas $P_k$ e âncoras $\mathcal{L}_k$. |
