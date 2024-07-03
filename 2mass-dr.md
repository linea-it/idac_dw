# Two Micron All Sky Survey (2MASS) Data Release 

Conteúdos:

- [Introdução](#introduction)
- [Como acessar](#how-to-access)
- [Informação dos Dados](#dataset-information)
- [Informação Técnicas](#technical-information)
- [Referências](#reference)



## Introdução

O projeto Two Micron All Sky Survey (2MASS) forneceu fotometria e astrometria precisas e uniformes para toda a esfera celeste nas bandas do infravermelho próximo J (1,24 µm), H (1,66 µm) e K<sub>s</sub> (2,16 µm). O 2MASS utilizou dois telescópios altamente automatizados de 1,3 metros, localizados em Mt. Hopkins, Arizona, e Cerro Tololo, Chile. Cada telescópio foi equipado com uma câmera de três canais, cada um contendo um conjunto de detectores de HgCdTe com resolução de 256×256 pixels.

A instalação do 2MASS em Mt. Hopkins iniciou suas operações de pesquisa em junho de 1997 e completou a varredura do céu do norte em dezembro de 2000. A instalação no hemisfério sul começou a coletar dados em março de 1998 e concluiu suas atividades em fevereiro de 2001. Durante esse período, foram coletados aproximadamente 24,5 terabytes de dados brutos de imagens, cobrindo 99,998% do céu. Para mais informações, consulte a documentação oficial [aqui](https://irsa.ipac.caltech.edu/data/2MASS/docs/releases/allsky/doc/sec1_1.new.html)


## Como acessar 

O <a href="https://www2.linea.org.br/">LIneA</a> (Laboratório Interinstitucional de e-Astronomia) disponibiliza uma biblioteca Python chamada `dblinea` que permite acessar o banco de dados da própria instituição. Essa biblioteca é útil para recuperar dados dentro da plataforma <a href="https://jupyter.org/hub">JupyterHub</a>. Para utilizar essa bibilioteca é necessário ter o pacote dblinea instalado em seu ambiente Python. Para instalá-lo use o comando:

<pre><code>pip install dblinea</code></pre>

A conexão com o banco de dados é feita pela classe DBBase. Um exemplo de utilização é criar uma instância do DBBase para fazer consultas ao banco de dados.

<pre><code># Importe a classe DBBase do pacote dblinea
from dblinea import DBBase

# Crie uma instância do DBBase. Essa instância será usada para se conectar ao banco de dados.
db = DBBase()
    
# Descreve o nome e o tipo das colunas de uma tabela.
db.describe_table("dr", schema = "2mass")
</code></pre>

Os métodos `fetchall(query)`, `fetchall_dict(query)` e `fetchall_df(query)` fazem a consulta referente ao conteúdo atribuído ao argumento (no exemplo abaixo, à variável `query`, uma string com um comando SQL) no banco de dados e retornam os dados, respectivamente, nos formatos: lista de tuplas, dicionário, objeto do tipo pandas.DataFrame. Por exemplo, vamos consultar o identificador único e as coordenadas dos objetos nas 10 primeiras linhas da tabela:

```python
query = "SELECT coadd_object_id, ra, dec FROM des_dr2.coadd_objects limit 10"
dataframe_10_objetos = db.fetchall_df(query)
```

Alguns exemplos de como utilizar a biblioteca dblinea para buscar objedos em uma dada região do céu estão presentes no _Jupyter notebook_ tutorial `2-acesso-a-dados.ipynb` disponível no repositório GitHub jupyterhub-tutorial. Para acessar o notebook, uma vez logado no LIneA JupyterHub, clone o repositório de tutoriais. No terminal do Jupyter Lab, execute:

```bash
$ git clone https://github.com/linea-it/jupyterhub-tutorial.git
```

A documentação oficial e completa da biblioteca `dblinea` está no site: https://dblinea.readthedocs.io/en/latest/quickstart.html
  

## Informação dos Dados

O projeto 2MASS resultou na criação de um Catálogo de Fontes Pontuais com 470.992.970 entradas e um Catálogo de Fontes Estendidas contendo 1.647.599 entradas, além de 4.121.439 imagens no formato FITS.

## 2MASS Point Source Catalog (PSC)

| Column Name      | Datatype             | Description                                                                                      |
|------------------|----------------------|--------------------------------------------------------------------------------------------------|
| `ra`             | float                | Ascensão reta J2000 em relação ao ICRS (em graus).                                               |
| `dec/decl`       | float                | Declinação J2000 em relação ao ICRS (em graus).                                                  |
| `err_maj`        | float                | Comprimento do semi-eixo maior da elipse de incerteza de posição de um sigma (em segundos de arco).           |
| `err_min`        | float                | Comprimento do semi-eixo menor da elipse de incerteza de posição de um sigma (em segundos de arco).           |
| `err_ang`        | float                | Ângulo de posição no céu do semieixo maior da elipse de incerteza de posição (Leste do Norte) |
| `designation`    | string               | Nome da fonte baseado na posição equatorial sexagesimal (por exemplo, "2MASS Jhhmmssss + ddmmsss [ABC...]"). |
| `j_m`            | float                | Magnitude padrão da banda J (ou limite superior de confiança a 95% se a fonte não for detectada na banda J). |
| `j_cmsig`        | float                | Incerteza da magnitude J na banda J (em milissegundos de arco).                                   |
| `j_msigcom`      | float                | Incerteza fotométrica corrigida para a magnitude padrão da banda J. (em milissegundos de arco).                         |
| `j_snr`          | float                | Relação sinal-ruído da detecção na banda J.                                                       |
| `h_m`            | float                | Magnitude padrão na banda H.                                                                      |
| `h_cmsig`        | float                | Incerteza da magnitude H na banda H (em milissegundos de arco).                                   |
| `h_snr`          | float                | Relação sinal-ruído da detecção na banda H.                                                       |
| `k_m`            | float                | Magnitude padrão na banda K.                                                                      |
| `k_cmsig`        | float                | Incerteza da magnitude K na banda K (em milissegundos de arco).                                   |
| `k_msigcom`      | float                | Incerteza combinada da magnitude K na banda K (em milissegundos de arco).                         |
| `k_snr`          | float                | Relação sinal-ruído da detecção na banda K.                                                       |
| `ph_qual`        | string               | Qualidade fotométrica da detecção (por exemplo, "AAA", "AB", "BC", etc.).                         |
| `rd_flg`         | string               | Flag de confiabilidade da detecção (por exemplo, "1", "2", "3", etc.).                            |
| `bl_flg`         | string               | Flag de contaminação por fontes brilhantes (por exemplo, "0", "1", "2", etc.).                    |
| `cc_flg`         | string               | Flag de confiabilidade cruzada (por exemplo, "0", "1", "2", etc.).                                |
| `ndet`           | int                  | Número de detecções na banda K.                                                                   |
| `prox`           | float                | Distância angular até a fonte mais próxima no catálogo 2MASS (em segundos de arco).               |
| `pxpa`           | float                | Ângulo de posição entre a fonte e a fonte mais próxima no catálogo 2MASS (em graus).              |
| `pxcntr`         | int                  | Índice da fonte mais próxima no catálogo 2MASS.                                                   |
| `gal_contam`     | string               | Flag de contaminação por fontes galácticas (por exemplo, "0", "1", "2", etc.).                    |
| `mp_flg`         | string               | Flag de qualidade do ponto de origem (0 = melhor qualidade, 1 = qualidade inferior, etc.)         |
| `pts_key/cntr`   | int                  | Chave única para identificar cada fonte no catálogo                                               |
| `hemis`          | string               | Hemisfério celeste (Norte ou Sul)                                                                 |
| `date`           | string               | Data da observação                                                                               |
| `scan`           | int                  | Número de varredura                                                                              |
| `glon`           | float                | Longitude galáctica                                                                              |
| `glat`           | float                | Latitude galáctica                                                                               |
| `x_scan`         | float                | Varredura cruzada                                                                                |
| `jdate`          | float                | Data juliana                                                                                    |
| `j_psfchi`       | float                | Estatística de qualidade da PSF (Função de Espalhamento Pontual) para a banda J                  |
| `h_psfchi`       | float                | Estatística de qualidade da PSF (Função de Espalhamento Pontual) para a banda H                  |
| `k_psfchi`       | float                | Estatística de qualidade da PSF (Função de Espalhamento Pontual) para a banda K                  |
| `j_m_stdap`      | float                | Magnitude padrão da banda J (fluxo integrado dentro de um raio de 4")                            |
| `j_msig_stdap`   | float                | Incerteza na magnitude padrão da banda J                                                         |
| `h_m_stdap`      | float                | Magnitude padrão da banda H (fluxo integrado dentro de um raio de 4")                            |
| `h_msig_stdap`   | float                | Incerteza na magnitude padrão da banda H                                                         |
| `k_m_stdap`      | float                | Magnitude padrão da banda K (fluxo integrado dentro de um raio de 4")                            |
| `k_msig_stdap`   | float                | Incerteza na magnitude padrão da banda K                                                         |
| `dist_edge_ns`   | float                | Distância da fonte à borda norte-sul da imagem (em segundos de arco)                             |
| `dist_edge_ew`   | float                | Distância da fonte à borda leste-oeste da imagem (em segundos de arco)                           |
| `dist_edge_flg`  | string               | Indicador de proximidade à borda da imagem.                                                     |
| `dup_src`        | string               | Indicador de duplicação de fonte.                                                               |
| `use_src`        | string               | Indicador de uso da fonte.                                                                      |
| `a`              | float                | Comprimento do eixo semi-maior da elipse de incerteza de posição (em segundos de arco).          |
| `dist_opt`       | float                | Distância ótima da fonte em relação ao centro da imagem.                                        |
| `phi_opt`        | float                | Ângulo de posição ótimo da fonte (leste em relação ao norte).                                    |
| `b_m_opt`        | float                | Magnitude J padrão (ou limite superior de confiança a 95%) para a banda J.                       |
| `vr_m_opt`       | float                | Magnitude K padrão (ou limite superior de confiança a 95%) para a banda K.                       |
| `nopt_mchs`      | int                  | Número de combinações de observações utilizadas para calcular as magnitudes padrão.              |
| `ext_key`        | int                  | Número de identificação único do registro no XSC que corresponde a esta fonte pontual.          |
| `scan_key`       | int                  | Número de identificação único do registro na Tabela de Informações da Varredura que corresponde à varredura do levantamento em que esta fonte foi detectada. |
| `coadd_key`      | int                  | Número de identificação único do registo na Tabela de Informação de Imagem do Atlas que corresponde à Imagem em que se enquadra a posição desta fonte. |
| `coadd`          | int                  | Número de sequência da Imagem Atlas em que se enquadra a posição desta fonte.                    |
  

## Informação Técnicas

### Download e Ingestão de Dados

Na tabela a seguir, são detalhados os períodos de tempo necessários para baixar arquivos 2Mass PSC e ingerir dados em nosso banco de dados.

| Action | Time  |
|---|---|
| Download* | 02:45:00 |
| Ingestion | xx:xx:xx |
| Indexing* | xx:xx:xx | 

| Others | -  |
|---|---|
| Number of rows | nnnn | 
| Number of columns | nnnn | 


\* O tamanho total do download foi de aproximadamente 43GB.
Here you can add more information about the process of download and ingestion and how they were done, eg: scripts, commands, etc. The scripts can be added in the idac_dw repository.

## Referências 



- https://irsa.ipac.caltech.edu/data/2MASS/docs/releases/allsky/doc/sec1_1.new.html


- https://iopscience.iop.org/article/10.1086/498708/meta


- https://dblinea.readthedocs.io/en/latest/quickstart.html




```python

```
