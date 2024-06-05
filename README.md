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

O objetivo do tutorial é apontar o uso do HMMER para a observação de alinhamento e homólogos em sequências de aminoácidos.

# Criação do modelo HMM
Modelos baseados em Hidden Markov Models (HMMs) procuram sequências em databases para achar homólogos e fazer alinhamentos nas sequêcias. Utilizamos sequências de aminoácidos para criar os modelos HMM e para comparar nossas sequências na busca, de forma que seja uma busca de aminoácidos x aminoácidos (proteínas versus um modelo HMM baseado em proteínas).

O primeiro passo para a criação do modelo HMM é a obtenção de sequências de uma mesma família de proteínas. O objetivo é caracterizar uma sequência específica nossa, logo alimentamos nosso modelo com proteínas semelhantes.
São necessários alguns passos para a criação de um modelo:
- 1) Obtenção das sequências de proteínas semelhantes com a proteína alvo (~mesma família);
- 2) Alinhamento das proteínas (normalmente utilizamos a ferramenta Aliview, mas existem outras [MAFFT, Jalview, etc]);
- 3) Trimagem das sequências;
- 4) Criação do modelo com **hmmbuild***.

## 1) Obtendo as sequências




