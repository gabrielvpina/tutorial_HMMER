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
- Alinhamento das proteínas (normalmente utilizamos a ferramenta Aliview, mas existem outras como: MAFFT-MSA e Jalview);
- Trimagem das sequências;
- Criação do modelo com **hmmbuild***.

## 1) Obtendo as sequências
A partir do momento que se tem uma proteína alvo para a criação de um modelo HMM, podemos começar a curadoria para selecionar nosso banco de dados. É necessário ter atenção nessa etapa, pois a escolha errada de protínas podem enviesar o modelo. Existem alguns aspectos importantes para selecionar as sequências:
- Selecionar proteínas que sejam da mesma família do nosso alvo, preferencialmente vindas de uma espécie próxima à expécie do organismo(s) trabalhado;
- Observar se há anotação de proteínas revisadas (isoladas em bancada), essas são prioridade na nossa escolha. Caso não seja possível obter sequências revisadas, usa-se as demais sequências;
- Restringir somente ao básico. Caso um banco seja feito de muitas proteínas diferentes, pode ocorrer uma variação grande nas sequências, o que empobreceria o modelo criado e enviesaria os resultados.

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

A partir da submissão das sequências, o output da plataforma será dividido em várias seções. Uma delas, que deve-se fazer o download, é a seção *Output Files*. Mais especificamente o arquivo *Alignment in FASTA format* (o com o *.aln-fasta* no final), esse arquivo será o input dos softwares de alinhamento, como o Aliview.
Na plataforma MAFFT é possível também observar os alinhamentos na sessão *Alignments* caso queira observar os alinhamentos mais rapidamente.

Os arquivos **narnavirus-RNA2.fasta** e **narnavirus-RNA3.fasta** podem ser usados como exemplos de inputs para a ferramenta MSA.

## 3) Trimagem das sequências

Com os arquivos do alinhamento em mãos, vamos importá-los para o software de alinhamento. Usaremos o [Aliview](https://ormbunkar.se/aliview/) como exemplo.

### INPUT

Ao abrir o Aliview, pressione `crtl + o` para selecionar o arquivo `*.aln-fasta` de alinhamento. Com o arquivo selecionado, será aberta a janela com as sequências do banco de dados alinhadas.

### Trimagem (corte) das sequências não alinhadas

Para o corte dessas sequências vamos selecionar as partes das extremidades de todas as sequências. Normalmente vamos encontrar algo como apontado na imagem:

![alt text](https://github.com/gabrielvpina/my_images/blob/main/Aliview_Trimming.png)

Essas regiões no início e no final do alinhamento devem ser removidas. Sua remoção ocorre no prórpio software do Aliview.

**Para retirar as partes não alinhadas** vamos selecionar com o mouse a parte superior, acima da primeira sequência e clicar. Assim a coluna inteira será selecionada, a partir daí com a tecla `shift` pressionada podemos e selecionando todos os itens do alinhamento com `shift + seta esq./dir.` e ir selecionando a faixa de sequências até a parte alinhada. Ao selecionar a parte de corte, basta pressionar `del` para retirar a área.

No caso dessa imagem também podemos observar que algumas sequências não possuem muitos alinhamentos com o resto do nosso banco, elas são:

- WNA22209.1 MAG: RdRp [Plasmopara viticola lesion associated narnavirus 3]
- WKR37706.1 hypothetical protein [Leptosphaeria biglobosa narnavirus 13]
- WKR37704.1 hypothetical protein, partial [Leptosphaeria biglobosa narnavirus 12]

Nesse caso deve-se avaliá-las e considerar sua deleção do modelo.

Com o início e final do alinhamento cortados, podemos salvar o arquivo final em formato fasta. Na barra superior vamos em *File* e de lá seleciona-se *Save as Fasta* e o local do arquivo output.

É com esse arquivo de output que criaremos o modelo HMM. Na página temos os arquivos **mafft-RNA2.aln-fasta** e **mafft-RNA3.aln-fasta** como exemplos.

Para mais informações sobre trimagem, existe esse tutorial de trimagem automática usando bibliotecas de Python, para quem tiver interesse.
Tutorial trimagem automática = https://pytrimal.readthedocs.io/en/stable/examples/hmmer.html

## 4) Criação do modelo

Agora que temos nosso arquivo final já alinhado e trimado, podemos iniciar o processo de criação do modelo HMM. Para isso vamos usar a aplicação **hmmbuild** do pacote HMMER.

```
# Construindo modelo hmm
# Sintaxe do comando: hmmbuild [-options] <hmmfile_out> <msafile>

# Query
hmmbuild meuModelo.hmm mafft-RNA2.aln-fasta
```

Com isso temos o nosso modelo HMM pronto. Também é possível achar o modelos HMM de domínios proteicos já feitos em bancos como o PFAM, na plataforma InterPro.
Modelo pronto PFAM = https://www.ebi.ac.uk/interpro/entry/pfam/PF00680/curation/

# Utilizando o arquivo hmm para pesquisa de Domínios

Para a pesquisa de domínios baseada no nosso modelo HMM vamos utilizar a aplicação **hmmscan**. Mas antes de usar essa aplicação é preciso criar um index com a fnução *hmmpress* do nosso modelo .hmm:

```
# Criando index do modelo com hmmpress

hmmpress meuModelo.hmm
```

Com o index criado, podemos assim usar o hmmscan para fazer a busca das nossas sequências.



