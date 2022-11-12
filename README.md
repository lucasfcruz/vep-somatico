# vep-somatico
#Anotação de um VCF somático utilizando vep-ensembl 105.0

Fazer um login na conta Google
Criar um colab (https://colab.research.google.com)
Importar arquivo do Google Drive

from google.colab import drive
drive.mount('/content/drive')

##Instalar o VEP ensembl-vep-105.0 (https://github.com/Ensembl/ensembl-vep/tags)
###1. Instalar dependencias 
###2. Download da release enemble versão 105.0
###3. Descompactar o arquivo .tar.gz
###4. Entar dentro do diretório

%%bash
sudo apt install unzip curl git libmodule-build-perl libdbi-perl libdbd-mysql-perl build-essential zlib1g-dev
wget -c https://github.com/Ensembl/ensembl-vep/archive/refs/tags/105.0.tar.gz
tar -zxvf 105.0.tar.gz
cd ensembl-vep-105.0
./INSTALL.pl --NO_UPDATE 

##Testar o comando

%%bash
cd ensembl-vep-105.0
./vep

##Listar diretório para verificar se tudo esta certo

!ls /content/drive/Shareddrives/T4-2022/homo_sapiens_refseq/105_GRCh37/WP312.filtered.vcf.gz

##Descompactar arquivos

!zcat  /content/drive/Shareddrives/T4-2022/homo_sapiens_refseq/105_GRCh37/WP312.filtered.vcf.gz | head -n 180 > teste.vcf
! cat teste.vcf

##Usar o ensembl-vep-105.0 para fazer as anotações
###-i | --input_file. -o | --output_file. --force_overwrite  vai forçar a substituição de um arquivo existente com mesmo nome.
###--species [species]    Species to use [default: "human"]
###--everything           Shortcut switch to turn on commonly used options   
###--fork [num_forks]     Use forking to improve script runtime


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
  
  ##Instalar o Pandas
  
  !pip install pandas
  
  ##
  
import pandas as pd
import csv
tabela = pd.read_csv('/content/WP312.filtered.vcf.tsv', sep='\t', skiprows=38)
df = pd.DataFrame(tabela)
df
  
  
  
  
  
  
  
  
  
  
  
