## Pipeline de Análise de rRNA 16S - Sequenciamento por Illumina MiSeq

A segui estão detalhadas e explicadas todas as etapas do pipeline de análise de rRNA 16S para processamento de dados desde SRA até os arquivos finais para análise no R, pertitindo a classificação taxonômica, alfa e beta diversidade e figuras relacionadas.

As estapas primárias e secundárias foram realizadas em ambiente Linux usando WLS2, com Ubuntu 24.04.3 LTS.

Os dados foram adquiridos do BioProject identificado como: PRJNA715083, que contem 384 runs (5.47Gb) e emglobam apmplicons do tipo pair-end, com material coletado do intestino.

# 1 - [Conversão de .sra para .fastq](1_etapa.md)
- ### Formato padrão NCBI para .fastq

# 2 - [Processamento dos arquivos .fastq](2_etapa.md)
- ### Controle de Qualidade (FastQC e MultiQC)
- ### Trimagem (Trimmomatics)

# 3 - [Importar para QUIIME2 e Denoising](3_etapa.md)
- ### QUIIME2
- ### DADA2

# 4 - [Classificação Taxonômica](4_etapa.md)
- ### QUIIME2

Para auxilio em suas análises de sequencimento:

#### www.linkedin.com/in/vinicius-mantovam

### Atutor: Vinicius Mantovam