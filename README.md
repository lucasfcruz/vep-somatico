# vep-somatico
## Anotação de um VCF somático utilizando vep-ensembl 105.0

### Sobre o VEP
O Ensembl Variant Effect Predictorl (VEP) é um poderoso instrumento para análise, anotação e priorização de variantes genômicas em regiões de codificação e não codificação. Ele fornece acesso a uma extensa coleção de anotações genômicas, com uma variedade de interfaces para atender a diferentes requisitos, e opções simples para configurar e estender a análise. É de código aberto, gratuito e suporta reprodutibilidade total de resultados. 

#### Fazer um login na conta Google
#### Criar um colab (https://colab.research.google.com)
#### Importar arquivo do Google Drive

```python
from google.colab import drive
drive.mount('/content/drive')
```

### Instalar o VEP ensembl-vep-105.0 (https://github.com/Ensembl/ensembl-vep/tags)
###### 1. Instalar dependencias 
###### 2. Download da release enemble versão 105.0
###### 3. Descompactar o arquivo .tar.gz
###### 4. Entar dentro do diretório
###### 5. Rodar o script INSTALL.PL com a opção --NO_UPDATE (para não pegar a versão mais nova)
comando|descrição|
---|---
sudo | executa um determinado comando como se fosse outro usuário
wget | permite download de dados por um link
tar | gera arquivo com extensão
```python
%%bash
sudo apt install unzip curl git libmodule-build-perl libdbi-perl libdbd-mysql-perl build-essential zlib1g-dev
wget -c https://github.com/Ensembl/ensembl-vep/archive/refs/tags/105.0.tar.gz
tar -zxvf 105.0.tar.gz
cd ensembl-vep-105.0
./INSTALL.pl --NO_UPDATE 
```
### Testar o comando
```python
%%bash
cd ensembl-vep-105.0
./vep
```
### Listar diretório para verificar se tudo esta certo
```python
!ls /content/drive/Shareddrives/T4-2022/homo_sapiens_refseq/105_GRCh37/WP312.filtered.vcf.gz
```
##### É necessario download do arquivo fasta para o seu drive (tamanho do arquivo Homo_sapiens_assembly19.fasta ~ 2Gb). Esse arquivo pode ser encontrado em repositórios como o do UCSC, por exemplo (https://hgdownload.soe.ucsc.edu/downloads.html).
### Descompactar arquivos
```python
!zcat  /content/drive/Shareddrives/T4-2022/homo_sapiens_refseq/105_GRCh37/WP312.filtered.vcf.gz | head -n 180 > teste.vcf
! cat teste.vcf
```
### Usar o ensembl-vep-105.0 para fazer as anotações

comando|descrição|
---|---
-i | --input_file. 
-o | --output_file. 
--force_overwrite | vai forçar a substituição de um arquivo existente com mesmo nome.
--species [species]  |  Species to use [default: "human"]
--everything  | Shortcut switch to turn on commonly used options   
--fork [num_forks] | Use forking to improve script runtime
--filter_common | Filtra com base na frequencia
```python
%%bash
./ensembl-vep-105.0/vep  \
  --fork 3 \
	-i /content/drive/Shareddrives/T4-2022/homo_sapiens_refseq/105_GRCh37/WP312.filtered.vcf.gz \
	-o WP312.filtered.vcf.tsv \
  --dir_cache /content/drive/Shareddrives/T4-2022 \
  --fasta /content/drive/Shareddrives/T4-2022/homo_sapiens_refseq/Homo_sapiens_assembly19.fasta \
  --cache --offline --assembly GRCh37 --refseq  \
  --pick --pick_allele --force_overwrite --tab --symbol --check_existing --variant_class --everything --filter_common \
  --fields "Uploaded_variation,Location,Allele,Existing_variation,HGVSc,HGVSp,SYMBOL,Consequence,IND,ZYG,Amino_acids,CLIN_SIG,PolyPhen,SIFT,VARIANT_CLASS,FREQS" \
  --individual all
```  
#### Instalar o Pandas
```python 
!pip install pandas
``` 

```python  
import pandas as pd
import csv
tabela = pd.read_csv('/content/WP312.filtered.vcf.tsv', sep='\t', skiprows=38)
df = pd.DataFrame(tabela)
df
```  
## Exemplo  
| index | - | -.1 | G |      1:907758     | PLEKHN1 |       missense_variant       | NM_001367552.1 |  E/G | -.2 | -.3 | -.4 |
|:-----:|:-:|:---:|:-:|:-----------------:|:-------:|:----------------------------:|:--------------:|:----:|:---:|:---:|:---:|
|     0 | - | -   | - | 1:914209-914210   | PERM1   | intron_variant               | NM_001369897.1 | -    | -   | -   | -   |
|     1 | - | -   | T | 1:934394          | HES4    | 3_prime_UTR_variant          | NM_001142467.2 | -    | -   | -   | -   |
|     2 | - | -   | - | 1:934884-934886   | HES4    | intron_variant               | NM_001142467.2 | -    | -   | -   | -   |
|     3 | - | -   | T | 1:955693          | AGRN    | synonymous_variant           | NM_001305275.2 | L    | -   | -   | -   |
|     4 | - | -   | T | 1:955717          | AGRN    | synonymous_variant           | NM_001305275.2 | L    | -   | -   | -   |
|     5 | - | -   | T | 1:955735          | AGRN    | missense_variant             | NM_001305275.2 | Q/H  | -   | -   | -   |
|     6 | - | -   | C | 1:981307          | AGRN    | intron_variant               | NM_001305275.2 | -    | -   | -   | -   |
|     7 | - | -   | - | 1:989298          | AGRN    | frameshift_variant           | NM_001305275.2 | V/X  | -   | -   | -   |
|     8 | - | -   | T | 1:989299          | AGRN    | missense_variant             | NM_001305275.2 | V/L  | -   | -   | -   |
|     9 | - | -   | T | 1:1149064         | TNFRSF4 | missense_variant             | NM_003327.4    | P/T  | -   | -   | -   |
|    10 | - | -   | - | 1:1216617-1216619 | SCNN1D  | stop_gained,inframe_deletion | NM_001130413.4 | SQ/* | -   | -   | -   |
  
  
  
  
  
  
