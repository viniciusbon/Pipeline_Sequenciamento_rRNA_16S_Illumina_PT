# 4 - Classificação Taxonômica
### Para classificação é necessário o download de bibliotecas, neste caso, foi usada SILVA

SILVA é um banco de dados de RNA ribossômico estabelecido em colaboração entre o Microbial Genomics Group do Instituto Max Planck de Microbiologia Marinha em Bremen, Alemanha, o Departamento de Microbiologia da Universidade Técnica de Munique e o Ribocon.

A classficação resultará em um arquivo .qzv que pode ser visualizado em ([QUIIME2](https://view.qiime2.org/))

#### A partir da classificação, podemos observar os seguintes resultados:

![Imagem](/QIIME2.png)
Podemos ver até onde foi possível chegar na escala de classificação e qual a confiança associada, com isso, alguns plots podem ser realizado ainda no QIIME2, como por exemplo, este barplot, para ver a composição de cada amostra.

![Imagem](/barplot.png)
Obs: A ferramenta permite algumas personalizações.

Ou até exportar para outras plataformas como iTOL:
![Imagem](/iTOL.png)

## Os dados podem ser exportados para o R para serem usado diversas bibliotecas como phyloseq e DESeq2.
Obs: Esta etapa será abordada em outro repositório.
