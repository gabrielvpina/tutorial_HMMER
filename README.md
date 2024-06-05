# Tutorial HMMER
Esse tutorial é baseado no uso das ferramentas pertencentes ao pacote HMMER (http://hmmer.org/). Serão usadas as seguintes ferramentas do pacote:
- hmmbuild
- hmmpress
- hmmscan
Para a instalação do HMMER local, é possível ter acesso via `apt` em sistemas baseados em Debian:
```
# instalar HMMER no Ubuntu
sudo apt update
sudo apt install hmmer

# instalar HMMER via conda
conda install -c bioconda HMMER
```
O uso de tais ferramentas ocorre na seguinte ordem:

**Construção de modelo HMM com `hmmbuild`  --> Criação de Index do modelo HMM com `hmmpress` --> Alinhamento e análise com `hmmscan`**

O objetivo do tutorial é apontar o uso do HMMER para a observação de alinhamento e homólogos em sequências de aminoácidos e nucleotídeos.

# Criação do modelo HMM
Modelos baseados em Hidden Markov Models (HMMs) procuram sequências em databases para achar homólogos 
