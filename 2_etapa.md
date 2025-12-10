# 2 - Processamento dos arquivos .fastq

### Controle de Qualidade (QC)

#### Nesta etapa faremos uma inspeção da qualidade da reads R1 e R2 usando a ferramenta FastQC (Version 0.12.0), essa ferramenta fornece um relatório detalhado sobre a qualidade das reads e será usado para definir os parâmetros na etapa seguinte.

A ferramenta FastQC pode ser baixada diretamente de seu reppositório ([Download](https://github.com/s-andrews/FastQC))

#### O relatório é gerado um para cada amostra, em arquivo .html, e pode ser visualizado diretamente no navegador, além disso alguns arquivos secundários também são gerados e ficam com a seguinte estrutura:

![Imagem](/FastQC_files.png)

## Interpretar FastQC report

Esta etapa é baseada no manual oficial da ferramenta, sobre os parâmetros de qualidade verificados pelo FastQC.

# Resumo Detalhado dos Módulos de Análise do FastQC

Este documento fornece um resumo detalhado, baseado no manual oficial, sobre os parâmetros de qualidade verificados pelo FastQC.

## 1. Estatísticas Básicas (Basic Statistics)
Este módulo gera estatísticas simples de composição para o arquivo analisado.

* **Informações Apresentadas:**
    * **Filename:** Nome original do arquivo.
    * **File type:** Se contém chamadas de base reais ou colorspace.
    * **Encoding:** A codificação ASCII dos valores de qualidade.
    * **Total Sequences:** Contagem total de sequências processadas (reais e estimadas). Se o modo Casava estiver ativo, sequências filtradas são removidas desta contagem.
    * **Sequence Length:** Comprimento da sequência mais curta e mais longa.
    * **%GC:** Percentual geral de GC de todas as bases.
* **Alertas:**
    * Este módulo nunca gera um aviso (Warning) ou erro (Failure).

![Imagem](/estatistica_basica.png)

---

## 2. Qualidade da Sequência por Base (Per Base Sequence Quality)
Mostra a visão geral da gama de valores de qualidade em cada posição no arquivo FastQ.

* **O Gráfico:**
    * Usa um gráfico do tipo BoxWhisker. A linha vermelha é a mediana, a caixa amarela é o intervalo interquartil (25-75%), os "bigodes" são os pontos de 10% e 90%, e a linha azul é a média de qualidade.
    * O fundo divide o eixo Y em qualidade muito boa (verde), razoável (laranja) e ruim (vermelha).
* **O que Representa:**
    * A qualidade geralmente degrada à medida que a corrida avança, então é comum ver as chamadas caindo na área laranja no final de uma leitura (read).
* **Alertas:**
    * **Aviso (Warning):** Quartil inferior < 10 ou mediana < 25 em qualquer base.
    * **Falha (Failure):** Quartil inferior < 5 ou mediana < 20 em qualquer base.



O arquivo SRR13985627_1_fastqc (R1), obteve os sequintes, note que a R2 tem normalmente desempenho inferior:

![Imagem](/sequence_quality.png)

O aumento de ciclos de amplificação antes da R2 introduz mais erros nas moléculas, o que contribui para uma taxa de erro percentual já elevada.

![Imagem](/R2.png)

# Entendendo Scores de Qualidade e Codificação ASCII

## 1. As Colunas da Tabela

* **Q (Phred Quality Score):** É a pontuação numérica da qualidade da base. Quanto maior o número, maior a confiança de que a base (A, T, C, G) foi lida corretamente.
* **P_error (Probabilidade de Erro):** A chance matemática de a base estar errada.
    * **Q20:** Significa 1 erro em 100 (**99%** de precisão).
    * **Q30:** Significa 1 erro em 1000 (**99,9%** de precisão).
    * **Q40:** Significa 1 erro em 10.000 (**99,99%** de precisão).
* **ASCII (Character):** O caractere que você realmente vê dentro do arquivo de texto FASTQ. Para economizar espaço, em vez de escrever "40", o arquivo usa um único símbolo (como `I` ou `J`).

---

## 2. A Diferença entre as Duas Tabelas (Base 33 vs. Base 64)

A imagem mostra dois padrões diferentes de "tradução" (offsets). O FastQC tenta adivinhar automaticamente qual desses foi usado.

### Tabela Superior: ASCII_BASE=33 (Padrão Moderno)
Este é o padrão atual e mais comum, usado por **Sanger, Illumina (1.8+), Ion Torrent e PacBio**.

* **A lógica:** O valor ASCII é igual ao Score de Qualidade + 33.
* **Exemplo:** Se a qualidade é **0** (péssima), o código ASCII é 33, que corresponde ao símbolo `!`.
* **Exemplo de Alta Qualidade:** Se a qualidade é **40** (excelente), o código é 40 + 33 = 73, que é a letra `I`.

### Tabela Inferior: ASCII_BASE=64 (Illumina Antiga)
Este padrão era usado em versões antigas dos sequenciadores Illumina (versões 1.3 a 1.7).

* **A lógica:** O valor ASCII é igual ao Score de Qualidade + 64.
* **Exemplo:** Se a qualidade é **0**, o código ASCII é 64, que corresponde ao símbolo `@`.
* **Exemplo de Alta Qualidade:** Se a qualidade é **40**, o código é 40 + 64 = 104, que é a letra `h`.

---


## 3. Scores de Qualidade por Sequência (Per Sequence Quality Scores)
Permite ver se um subconjunto de sequências possui valores de qualidade universalmente baixos.

* **O que Representa:**
    * Um subconjunto com má qualidade (ex: bordas do campo de visão) deve representar apenas uma pequena porcentagem do total.
    * Se uma proporção significativa tiver baixa qualidade, pode indicar um problema sistemático (ex: falha em uma parte da flowcell).
* **Alertas:**
    * **Aviso (Warning):** Qualidade média mais observada < 27 (taxa de erro de 0,2%).
    * **Falha (Failure):** Qualidade média mais observada < 20 (taxa de erro de 1%).

![Imagem](/persequence_R1.png)
---

## 4. Conteúdo de Sequência por Base (Per Base Sequence Content)
Plota a proporção de cada base (A, T, G, C) em cada posição.

* **O que Representa:**
    * Em uma biblioteca aleatória, as linhas devem correr paralelas com pouca diferença entre as bases.
    * Vieses fortes que mudam em bases diferentes indicam contaminação por sequência super-representada.
    * Viés consistente em todas as bases indica viés na biblioteca original ou problema sistemático no sequenciamento.
* **Alertas:**
    * **Aviso (Warning):** Diferença entre A e T, ou G e C > 10% em qualquer posição.
    * **Falha (Failure):** Diferença entre A e T, ou G e C > 20% em qualquer posição.

![imagem](/perbase_content.png)

---

## 5. Conteúdo GC por Sequência (Per Sequence GC Content)
Mede o conteúdo GC de cada sequência e compara com uma distribuição normal modelada.

* **O que Representa:**
    * Espera-se uma distribuição normal onde o pico central corresponde ao GC geral do genoma.
    * Distribuição com formato incomum pode indicar biblioteca contaminada; distribuição normal deslocada indica viés sistemático.
* **Alertas:**
    * **Aviso (Warning):** Soma dos desvios da distribuição normal representa mais de 15% das leituras.
    * **Falha (Failure):** Soma dos desvios representa mais de 30% das leituras.

![Imagem](/sequence_GCcontent.png)
---

## 6. Conteúdo N por Base (Per Base N Content)
Plota a porcentagem de chamadas de base "N" (sem confiança suficiente) em cada posição.

* **O que Representa:**
    * É comum ver uma proporção muito baixa de Ns, especialmente perto do fim da sequência.
    * Proporção acima de alguns por cento sugere que o pipeline de análise falhou em interpretar os dados.
* **Alertas:**
    * **Aviso (Warning):** Conteúdo de N > 5% em qualquer posição.
    * **Falha (Failure):** Conteúdo de N > 20% em qualquer posição.

![Imagem](/N_content.png)
---

## 7. Distribuição do Comprimento da Sequência (Sequence Length Distribution)
Gera um gráfico da distribuição dos tamanhos dos fragmentos.

* **O que Representa:**
    * Alguns sequenciadores geram comprimentos uniformes, outros variados. Pipelines podem cortar (trim) sequências de má qualidade, gerando comprimentos variados.
* **Alertas:**
    * **Aviso (Warning):** Se as sequências não tiverem todas o mesmo comprimento.
    * **Falha (Failure):** Se qualquer sequência tiver comprimento zero.

### Aqui podemos ver que uma grande quantidade de sequencias tem comprimento >245bp 
![Imagem](/comprimento.png)

---

## 8. Sequências Duplicadas (Duplicate Sequences)
Conta o grau de duplicação para cada sequência (analisa as primeiras 200.000 sequências para estimativa).

* **O que Representa:**
    * Baixa duplicação pode indicar alta cobertura da sequência alvo.
    * Alta duplicação geralmente indica viés de enriquecimento (ex: super amplificação por PCR).
    * Leituras longas (>75bp) são truncadas para 50bp para esta análise, pois erros de sequenciamento em leituras longas aumentam artificialmente a diversidade observada.
* **Alertas:**
    * **Aviso (Warning):** Sequências não únicas > 20% do total.
    * **Falha (Failure):** Sequências não únicas > 50% do total.

![Imagem](/Duplication.png)
---

## 9. Sequências Super-representadas (Overrepresented Sequences)
Lista sequências que compõem mais de 0,1% do total e busca correspondências em banco de dados de contaminantes.

* **O que Representa:**
    * Uma sequência muito super-representada pode ser biologicamente significativa ou indicar contaminação/falta de diversidade.
    * Para conservar memória, apenas sequências que aparecem nas primeiras 200.000 são rastreadas.
* **Alertas:**
    * **Aviso (Warning):** Qualquer sequência representando > 0,1% do total.
    * **Falha (Failure):** Qualquer sequência representando > 1% do total.

---

## 10. Kmers Super-representados (Overrepresented Kmers)
Conta o enriquecimento de cada 5-mer dentro da biblioteca.

* **O que Representa:**
    * Detecta problemas que a análise de sequência exata perde, como sequências longas com má qualidade ou sequências parciais.
    * Calcula a razão observado/esperado baseada no conteúdo de bases da biblioteca.
* **Alertas:**
    * **Aviso (Warning):** Enriquecimento > 3x no geral ou > 5x em posição individual.
    * **Falha (Failure):** Enriquecimento > 10x em qualquer posição individual.



### Feitas as observações, vamos definir os parametros para trimagem das amostras

# Trimmomatic

#### O Trimmomatic executa uma variedade de tarefas úteis de corte para dados Illumina pair-end e single-end. A seleção das etapas de corte e seus parâmetros associados são fornecidos na linha de comando.

#### As etapas de triming atuais são:
- ILLUMINACLIP: Remove o adaptador e outras sequências específicas da Illumina da sequência de leitura.
- SLIDINGWINDOW: Realize o corte na janela de correr, cortando-a quando a qualidade média dentro da janela ficar abaixo de um limite predefinido.
- LEADING: Cortar bases do início de uma leitura, se abaixo de um limite de qualidade.
- TRAILING: Cortar bases da extremidade de uma leitura, se a qualidade estiver abaixo de um limite predefinido.
- CROP: Corte a leitura para um comprimento especificado
- HEADCROP: Corte o número especificado de bases a partir do início da sequência de leitura.
- MINLEN: Descartar a leitura se ela for menor que um comprimento especificado.
- TOPHRED33: Converter pontuações de qualidade para Phred-33
- TOPHRED64: Converter pontuações de qualidade para Phred-64
