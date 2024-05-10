# Dark Energy Survey - Data Release 2 (DES DR2)

Contents:

- [Introdução](#introduction)
- [Como acessar](#how-to-access)
- [Informação dos Dados](#dataset-information)
- [Informação Técnicas](#technical-information)
- [Referências](#reference)


## Introdução


O Dark Energy Survey Data Release 2 (DES DR2) é um amplo levantamento de imagens terrestres abrangendo uma área de aproximadamente 5000 graus quadrados do céu do hemisfério sul da Via Láctea, do espectro visível e do infravermelho próximo, em cinco bandas fotométricas amplas: grizY. O principal objetivo do DES é aprimorar nossa compreensão da aceleração cósmica e da natureza da energia escura. 

Para atender aos objetivos científicos, a Colaboração DES desenvolveu e empregou a Dark Energy Camera (DECam), uma câmera de 570 megapixels com um campo de visão de 3 graus quadrados. Esta câmera foi instalada no topo do telescópio Blanco de 4 metros, localizado no Observatório Interamericano Cerro Tololo (CTIO), situada no norte do Chile.

O DES DR2 abrange aproximadamente 691 milhões de objetos astronômicos distintos, identificados em 10.169 imagens coadicionadas, cada um com uma área de 0,534 graus quadrados, derivadas de um total de 76.217 exposições. A precisão fotométrica é de cerca de 10 milimagnitudes, e a precisão astrométrica interna média é de aproximadamente 27 milissegundos de arco. Para mais informações, consulte o documentação oficial [aqui](https://datalab.noirlab.edu/des/)

## Como acessar

O <a href="https://www2.linea.org.br/">LIneA</a> (Laboratório Interinstitucional de e-Astronomia) disponibiliza uma biblioteca Python chamada `dblinea` que permite acessar o banco de dados da própria instituição. Essa biblioteca é útil para recuperar dados dentro da plataforma <a href="https://jupyter.org/hub">JupyterHub</a>. Para utilizar essa bibilioteca é necessário ter o pacote dblinea instalado em seu ambiente Python. Para instalá-lo use o comando:

<pre><code>pip install dblinea</code></pre>

A conexão com o banco de dados é feita pela classe DBBase. Um exemplo de utilização é criar uma instância do DBBase para fazer consultas ao banco de dados.

<pre><code># Importe a classe DBBase do pacote dblinea
from dblinea import DBBase
  
# Crie uma instância do DBBase. Essa instância será usada para se conectar ao banco de dados.
 db = DBBase()
    
# Descreve o nome e o tipo das colunas de uma tabela.
db.describe_table("dr2", schema = "des")
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

O Dark Energy Survey Data Release 2 é composto por diversas tabelas com 215 colunas e mais de 600 milhões de linhas. Além disso, foram criados 2 índices utilizando o índice espacial q3c. Esses índices estão associados a campos essenciais, como:
- `RA`
- `DEC`

### DES DR2 Main

A tabela abaixo fornece uma descrição das colunas encontradas no DES DR2, juntamente com seus tipos de dados associados. É importante observar que a terceira coluna desta tabela especifica o número de colunas associadas a cada elemento. Por exemplo, para FLAGS_G,R,I,Z,Y, existem cinco colunas associadas: FLAGS_G, FLAGS_R, FLAGS_I, FLAGS_Z e FLAGS_Y.

| Nome da Coluna | Tipo de Dados | Descrição | Colunas |
|---|---|---|---|
| A_IMAGE | Float | Tamanho do eixo principal com base em um modelo isofotal [pixel] | 1  |
| ALPHAWIN_J2000 | Double| Ascensão reta para o objeto, J2000 no sistema ICRS (precisão total mas não indexada) [deg] | 1 |
| AWIN_IMAGE_G,R,I,Z,Y |Float | Tamanho do eixo principal, a partir de medições de momento em janela de 2ª ordem [pixels] | 5 |
| BACKGROUND_G,R,I,Z,Y     | Float |Nível de fundo pelo amplificador CCD [mag] | 1       |
| B_IMAGE | Float | Tamanho do eixo menor com base em um modelo isofotal [pixel] | 1 |
| BWIN_IMAGE_G,R,I,Z,Y | Float | X-centroide das medidas em janela na imagem de banda coadicionada para o eixo menor [pixel] | 5 |
| COADD_OBJECT_ID | Bigint | Identificador único para os objetos coadicionados | 1 |
| CLASS_STAR_G,R,I,Z,Y | Float |Classificador de fonte estendida morfológica simples. Valores entre 0 (galáxias) e 1 (estrelas). | 5 |
| DEC* | Double | Declinação, com precisão quantizada para indexação (DELTAWIN_J2000 tem precisão total, mas não é indexada) [deg] | 1 |
| DELTAWIN_J2000 | Double| decl. para o objeto, J2000 no sistema ICRS (precisão total mas não indexada) [deg] |1 |
| EBV_SFD98                | Double | Coeficiente de avermelhamento E(B-V) de Schlegel, Finkbeiner & Davis, 1998 [mag] | 1 |
| ERRA_IMAGE | Float | Incerteza no tamanho do eixo principal, a partir do modelo isofotal [pixel] |
| ERRAWIN_IMAGE_G,R,I,Z,Y | Float | Incerteza no tamanho do eixo principal, a partir de medições em janelas convergentes, assumindo ruído não correlacionado [pixel] | 5 |
| ERRB_IMAGE | Float | Incerteza no tamanho do eixo menor, do modelo isofotal [pixel] | 1 |
| ERRBWIN_IMAGE_G,R,I,Z,Y | Float | Incerteza no X-centroide das medidas em janela na imagem de banda coadicionada para o eixo menor [pixel] | 5 | 
| ERRTHETA_IMAGE | Float | Incerteza na posição da fonte, do modelo isofotal | 1 |  
| ERRTHETAWIN_IMAGE_G,R,I,Z,Y | Float | Incerteza no ângulo de posição (theta) para o objeto, da medição em janela convergida [deg] | 5 |
| ERRX2WIN_IMAGE_G,R,I,Z,Y | Double | Incerteza no segundo momento do centróide da distribuição em x, das medidas em janela convergidas [pixel2] | 5 |
| ERRXYWIN_IMAGE_G,R,I,Z,Y | Double | Incerteza no segundo momento da distribuição xy, das medidas em janela convergidas [pixel2] | 5 |
| ERRY2WIN_IMAGE_G,R,I,Z,Y | Double | Incerteza no segundo momento do centróide da distribuição em y, das medidas em janela convergidas [pixel2] | 5 |
| EXTENDED_CLASS_COADD     | SMALLINT | O: estrelas de alta confiança; 1: estrelas candidatas; 2: principalmente galáxias; 3: galáxias de alta confiança; -9: Sem dados; Fotometria Sextractor | 1       |
| EXTENDED_CLASS_WAVG      | SMALLINT | O: estrelas de alta confiança ; 1: estrelas candidatas; 2: principalmente galáxias; 3: galáxias de alta confiança; -9:Sem dados; Fotometria WAVG | 1|
| FLAGS_G,R,I,Z,Y          | SMALLINT | Sinalizador descrevendo conselhos cautelosos sobre o processo de extração de fontes (SINAIS <4 para objetos bem comportados) | 1       |
| FLUX_AUTO_G,R,I,Z,Y | Float | Medição de fluxo de abertura, modelo elíptico com base no raio de Kron [ADU] | 5 |
| FLUX_RADIUS_G,R,I,Z,Y | Float | Raio de meia luz para o objeto, a partir de uma curva de crescimento elíptica, modelada em duas dimensões [pixel] |  5  |
| FLUXERR_AUTO_G,R,I,Z,Y | Float | Incerteza na medição de fluxo de abertura, modelo elíptico com base no raio de Kron [ADU] | 5 |
| FWHM_IMAGE_G,R,I,Z,Y | Float | Largura total à meia-altura (FWHM) para cada banda [pixel] | 5 |
| GALACTIC_B | Float | Latitude Galáctica [deg] | 1 |
| GALACTIC_L | Float | Longitude Galáctica [deg] | 1 |
|HPIX_32,64,1024,4096,16384  | BIGINT | Identificador Healpix para seu tamanho de grade nside, em um esquema NESTED | 5 |
| IMAFLAGS_ISO_G,R,I,Z,Y   | Integer | Sinalizador identificando fontes com pixels ausentes/marcados considerando todas as imagens de única época (sem viés) | 1       |
| ISOAREA_IMAGE_G,R,I,Z,Y | Integer | Área isofotal da fonte coadicionada [pixel2] | 5 |
| KRON_RADIUS | Float | Raio de Kron medido a partir da imagem coadicionada [pixel] | 1 |
| KRON_RADIUS_G,R,I,Z,Y | Float | Raio de Kron medido a partir da imagem coadicionada  | 5 |
| MAG_AUTO_G,R,I,Z,Y | Float | Estimativa de magnitude, para um modelo elíptico baseado no raio de Kron [mag] | 5 |
| MAG_AUTO_G,R,I,Z,Y_DERED | Float | Estimativa de magnitude descorrigida (usando SFD98), para um modelo elíptico baseado no raio de Kron [mag] | 5 |
| MAGERR_AUTO_G,R,I,Z,Y | Float | Incerteza na estimativa de magnitude, para um modelo elíptico baseado no raio de Kron [mag] | 5 |
| NEPOCHS_G,R,I,Z,Y  | Integer | Número de épocas em que a fonte é detectada em imagens de época única | 5 |
| NITER_MODEL_G,R,I,Z,Y | Integer | Número de iterações em medições fotométricas de ajuste de modelo | 5 |
| RA* | Double | Ascensão reta, com precisão quantizada para indexação (ALPHAWIN_J2000 tem precisão total mas não indexada) [deg] | 1 |
| SPREAD_MODEL_G,R,I,Z,Y | Float | Classificador morfológico baseado na comparação entre o modelo PSF versus exponencial-PSF. Valores mais próximos de 0 correspondem a estrelas, valores maiores correspondem a galáxias | 5 |
| SPREADERR_MODEL_G,R,I,Z,Y | Float | Incerteza no classificador morfológico baseado na comparação entre o modelo PSF versus exponencial-PSF | 5 |
| THETAWIN_IMAGE_G,R,I,Z,Y | Float | Ângulo de posição (theta) para a fonte, para medição em janela convergida crescente de x para y [deg] |  5 |
| THETA_J2000 | Float | Ângulo de posição (theta) do eixo principal para o objeto no sistema ICRS (J2000) [deg] | 1 |
| TILENAME | Char | Identificador de cada um dos blocos onde está quadriculado o levantamento | 1 |
| WAVG_FLUX_PSF_G,R,I,Z,Y | Float |  Fluxo médio ponderado, de detecções de época única ajustadas ao PSF [ADU] | 5 |
| WAVG_FLUXERR_PSF_G,R,I,Z,Y | Float | Incerteza da medição de fluxo médio ponderado de PSF ajusta detecções de época única [ADU] | 5 |
| WAVG_MAG_PSF_G,R,I,Z,Y | Float | Magnitude média ponderada, de detecções de época única ajustadas ao PSF [mag] | 5 |
| WAVG_MAG_PSF_G,R,I,Z,Y_DERED | Float | Magnitude média ponderada reduzida (usando SFD98) de PSF ajusta detecções de época única [mag] | 5 |
| WAVG_MAGERR_PSF_G,R,I,Z,Y | Float | Incerteza da magnitude média ponderada, de detecções de época única ajustadas ao PSF [mag] | 5 |
| WAVG_SPREAD_MODEL_G,R,IZ,Y | Float | MODELO SPREAD usando os valores médios ponderados das detecções de única época | 5 |
| WAVG_SPREADERR_MODEL_G.R.I.Z.Y | Float | Incerteza no MODELO SPREAD usando os valores médios ponderados das detecções de única época | 5 |
| X2WIN_IMAGE_G,R,I,Z,Y | Double | Segundo momento na direção x, das medidas convergidas em janela [pixel2] | 5 |
| XYWIN_IMAGE_G,R,I,Z,Y |  Double | Segundo momento na direção xy, das medidas convergidas em janela [pixel2] | 5 |
| XWIN_IMAGE |  Double | X-centroide das medidas em janela na imagem coadicionada [pixel] | 1 |
| XWIN_IMAGE_G,R,I,Z,Y | Double | X-centroide das medidas em janela na imagem de banda coadicionada [pixel] | 5 |
| Y2WIN_IMAGE_G,R,I,Z,Y |  Double | Segundo momento na direção y, das medidas convergidas em janela [pixel2] | 5 |
| YWIN_IMAGE |  Double | Y-centroide das medidas em janela na imagem coadicionada [pixel] | 1 |
| YWIN_IMAGE_G,R,I,Z,Y |  Double | Y-centroide das medidas em janela na imagem de banda coadicionada [pixel] | 5 |

\* Colunas indexadas    

## Informação Técnicas

### Download e Ingestão de Dados


Na tabela a seguir, são detalhados os períodos de tempo necessários para baixar arquivos DES DR2 e ingerir dados em nosso banco de dados.

| Ação | Tempo  |
|---|---|
| Download**| 678 minutos |
| Ingestão | 824 minutos |

\** O tamanho total do download foi de aproximadamente 9,2TB. <br>


<br>

As tabelas abaixo exibem o tempo dedicado à criação de índices para a tabela DES DR2 e o número de linhas e colunas.

| Índices | Tempo  |       
|---|---|
| `RA` e `DEC` | 1037 minutos | 

<br>

| Outros | -  |
|---|---|
| Número de linhas | 691.498.505 | 
| Número de colunas | 215 | 



## Referências

- https://datalab.noirlab.edu/des/
- https://dblinea.readthedocs.io/en/latest/quickstart.html
- https://arxiv.org/abs/2101.05765
