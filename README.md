## Pipeline de Análise de rRNA 16S - Sequenciamento por Illumina MiSeq

A segui estão detalhadas e explicadas todas as etapas do pipeline de análise de rRNA 16S para processamento de dados desde SRA até os arquivos finais para análise no R, pertitindo a classificação taxonômica, alfa e beta diversidade e figuras relacionadas.

As estapas primárias e secundárias foram realizadas em ambiente Linux usando WLS2, com Ubuntu 24.04.3 LTS.

Os dados foram adquiridos do BioProject identificado como: PRJNA715083, que contem 384 runs (5.47Gb) e emglobam apmplicons do tipo pair-end, com material coletado do intestino.

# 1 - Conversão de .sra para .fastq 

### Arquivos .sra

Os arquivos .sra são o formato padrão usados pelo NCBI, logo ao fazer o download dos dados, cada uma das reads será baixada e terão esta extensão.

Caso precise de auxilio em suas análises de sequencimento, deixo meu contato abaixo.
([Linkedin](www.linkedin.com/in/vinicius-mantovam))

### Atutor: Vinicius Mantovam