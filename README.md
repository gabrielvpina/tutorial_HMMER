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
- Obtenção das sequências de proteínas semelhantes com a proteína alvo (~mesma família);
- Alinhamento das proteínas (normalmente utilizamos a ferramenta Aliview, mas existem outras [MAFFT, Jalview, etc]);
- Trimagem das sequências;
- Criação do modelo com **hmmbuild***.

## 1) Obtendo as sequências
A partir do momento que se tem uma proteína alvo para a criação de um modelo HMM, podemos começar a curadoria para selecionar nosso banco de dados. É necessário ter atenção nessa etapa, pois a escolha errada de protínas podem enviesar o modelo. Existem alguns aspectos importantes para selecionar as sequências:
- Selecionar proteínas que sejam da mesma família do nosso alvo, preferencialmente vindas de uma espécie próxima à expécie do organismo(s) trabalhado;
- Observar se há anotação de proteínas revisadas (isoladas em bancada), essas são prioridade na nossa escolha. Caso não seja possível obter sequências revisadas, usa-se as demais sequências;
- Restringir a somente o básico. Caso um banco seja feito de muitas proteínas, pode ocorrer uma variação grande nas sequências, o que empobreceria o modelo criado e enviesaria os resultados.

Existem algumas fontes de pesquisa para preteínas. As mais usadas são o banco de dados proteicos do NCBI (https://www.ncbi.nlm.nih.gov/protein/) e o UniProt (https://www.uniprot.org/). Após pesquisar as proteínas, é necessário obter o arquivo fasta de cada.
```
# Estrutura do arquivo fasta com as sequências de Proteínas

>Protein_1
ABCDEFGHIJKLMN
>Protein_2
ABCDEKLMKN
.
.
.
>Protein_N
ABCJKLMPQ
```

## 2) Alinhamento das sequências
Essa etapa serve para checar as regiões de alinhamento entre as sequências escolhidas. Para isso utiliza-se softwares de alinhamento como Jalview ou Aliview. As regiões alinhadas podem ser uma provável **região conservada** entre as sequências do banco de dados. É importante prestar atenção nisso.

![alt text](https://github.com/gabrielvpina/my_images/blob/main/Aliview_example.png)

Também observa-se aqui as variações das sequências, como tamanho total, regiões exclusivas de algumas sequências, diferenças de aminoácidos em certas regiões, etc.

**Alinhamento na plataforma MAFFT (MSA)**

Para fazer o alinhamento das sequências do banco de dados, vamos utilizar a ferramenta *Multiple Sequence Aligner* (MSA) disponível online pelo seguinte link - https://www.ebi.ac.uk/jdispatcher/msa/mafft. Nela vamos colocar nossa sequência FASTA como input e selecionar o *Sequence Type* como *Protein*.
Com o input pronto, submete-se o trabalho.
A partir da submissão das sequências, o output da plataforma será dividido em várias seções. Uma delas, que deve-se fazer o download, é a seção *Output Files*. Mais especificamente o arquivo *Alignment in FASTA format*, esse arquivo será o input dos softwares de alinhamento, como o Aliview.
Na plataforma MAFFT é possível também observar os alinhamentos na sessão *Alignments* caso queira observar os alinhamentos mais rapidamente.

Os arquivos **narnavirus-RNA2.fasta** e **narnavirus-RNA3.fasta** podem ser usados como exemplos de inputs para a ferramenta MSA.

## 3) Trimagem das sequências



Tutorial trimagem automática = https://pytrimal.readthedocs.io/en/stable/examples/hmmer.html

## 4) Criação do modelo
Modeo pronto PFAM = https://www.ebi.ac.uk/interpro/entry/pfam/PF00680/curation/




