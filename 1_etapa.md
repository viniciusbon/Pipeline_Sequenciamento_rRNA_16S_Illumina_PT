# 1 - Conversão de .sra para .fastq 

#### Os arquivos .sra baixados do NCBI devem ser convertidos em .fastq, contudo, antes de realizar a conversão é importante compreender a composição de cada um desses.

## . sra

#### O .sra é um formato binário proprietário do NCBI, usado para armazenar dados de sequenciamento brutos, com metadados internos e deve aparecer no seu terminal da seguinte forma:

![Minha imagem](/imgs/sra_raw_file.png)
#### Vantagens: 

- Mais compacto que FASTQ
- Organizado internamente (metadados)
- Ideal para download rápido

#### No terminal, inspecionando apenas as primeiras linhas da amostra SRR13985619.sra será observado o seguinte:

![Minha imagem](/imgs/sra_raw_head.png)

#### os caracteres aparecem desta forma pois estão em formato binário proprietário e não possibilitam uma leitura inicial sem a conversão para .fastq.


## .fastq

#### Os arquivos .fastq contem respectivamente em uma estrutura de 4 linhas, o identificador, as bases sequenciadas, identificador e Scores de Qualidade.

![Minha imagem](/imgs/fastq_formato.png)

#### No terminal, inspecionando apenas as primeiras linhas da amostra SRR13985619_1.fastq será observado o seguinte:
![Minha imagem](/imgs/fastq_head.png)


## Conversão usando SRA toolkit

#### Após conversão teremos 2 arquivos de cada amostra, Read 1 (forward) e Read 2 (reverse), uma vez que estamos lidando com dados pair-end.

#### Usando o SRA toolkit podemos converter os arquivos tendo agora as R1 e R2 de cada amostra, teremos algo da seguinte forma:

![Minha imagem](/imgs/fastq_amostras.png)

#### O sufixo _1.fastq e _2.fastq, indicam as reads R1 e R2 respectivamente.

#### A partir dos arquivos fastq podemos realizar as proximas etapas
Obs: A ferramenta pode ser baiaxada diretamente do seu repositório. ([Download](https://github.com/ncbi/sra-tools/wiki/01.-Downloading-SRA-Toolkit))
