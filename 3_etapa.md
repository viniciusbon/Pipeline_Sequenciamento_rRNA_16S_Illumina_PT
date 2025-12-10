# 3 - Importar para QUIIME2 e Denoising
#### Após controle de qualidade e trimagem, vamos importar os dados para o QUIIME2 para fazer o denoising

Obs 1: Para instalação do QUIIME2 faça o ([download](https://github.com/qiime2/distributions)).

Obs 2: O java, necessáriamente deve estar instalado e atualizado em sua máquina.



## Importar dados para o QUIIME2
#### O QIIME 2 usa um formato interno chamado QZA (QIIME Artifact), logo, os dados devem ser importados principalmente por:
#### -Padronizar arquivos vindos de diferentes equipamentos (Illumina, IonTorrent etc.)
#### - Garantir que os metadados (nome do sample, filepath, PHRED) fiquem organizados
#### - Permitir rastreabilidade total: cada arquivo QZA guarda histórico do que foi feito
#### - Proteger o pipeline de erros (nomes incorretos, PHRED errado, caminho relativo)

Os dados são importados usando os seguinte parâmetros:

qiime tools import \
 --type 'SampleData[SequencesWithQuality]' \
 --input-path fq-manifest.tsv \
 --output-path demux.qza \
 --input-format SingleEndFastqManifestPhred33V2

## Denoising (DADA2)
Atualmente, DADA2 e deblur são os dois métodos de redução de ruído disponíveis no QIIME 2, e ambos agrupam sequências em ASVs.

Amplicon Sequence Variants (ASVs) são sequências de DNA únicas, de alta resolução e corrigidas por erros, usadas em estudos de microbioma (metagenômica) para diferenciar cepas microbianas com precisão de um único nucleotídeo, sem depender de um limite arbitrário de similaridade como as OTUs.

