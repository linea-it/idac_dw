# Gaia Data Release 3 

Conteúdo:

- [Introdução](#introduction)
- [Como Acessar](#how-to-access)
- [Informação dos Dados](#dataset-information)
- [Informação Técnicas](#technical-information)
- [Referências](#reference)


## Introdução

A missão Gaia, conduzida pela Agência Espacial Europeia (ESA), foi lançada em 19 de dezembro de 2013 com o objetivo de mapear a Via Láctea em três dimensões. Atualmente, encontra-se em sua terceira fase, conhecida como Gaia Data Release 3 (Gaia DR3) lançada em 13 de junho de 2022. Este lançamento proporcionou uma astrometria de alta precisão das posições e movimentos de uma vasta gama de objetos celestes. O Gaia DR3 oferece dados sobre a posição no céu, ascensão reta (α) e declinação (δ), paralaxe e movimento próprio para aproximadamente 1,46 bilhão de fontes.

Gaia DR3 utiliza três bandas fotométricas para medir as magnitudes das fontes: G, G<sub>BP</sub> e G<sub>RP</sub>, presentes nos lançamentos anteriores. A grande novidade desta fase é a adição da fotometria G<sub>RVS</sub>. A documentação oficial do Gaia DR3 está disponível no link: https://gea.esac.esa.int/archive/documentation/GDR3/index.html

## Como Acessar

O <a href="https://www2.linea.org.br/">LIneA</a> (Laboratório Interinstitucional de e-Astronomia) disponibiliza uma biblioteca Python chamada `dblinea` que permite acessar o banco de dados da própria instituição. Essa biblioteca é útil para recuperar dados dentro da plataforma <a href="https://jupyter.org/hub">JupyterHub</a>. Para utilizar essa bibilioteca é necessário ter o pacote dblinea instalado em seu ambiente Python. Para instalá-lo use o comando:

<pre><code>pip install dblinea</code></pre>

A conexão com o banco de dados é feita pela classe DBBase. Um exemplo de utilização é criar uma instância do DBBase para fazer consultas ao banco de dados.

<pre><code># Importe a classe DBBase do pacote dblinea
from dblinea import DBBase
  
# Crie uma instância do DBBase. Essa instância será usada para se conectar ao banco de dados.
 db = DBBase()
    
# Descreve o nome e o tipo das colunas de uma tabela.
db.describe_table("dr3", schema = "gaia")
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


## Exemplos

### Pré-visualização dos dados

<pre><code>import numpy as np
import matplotlib.pyplot as plt
from dblinea import DBBase

# Cria uma instância do DBBase
db = DBBase()

# Especifique o nome da tabela e o schema
table_name = "dr3"
schema_name = "gaia"

# Buscar dados da tabela especificada
query = f"SELECT ra, dec, phot_bp_mean_mag, phot_rp_mean_mag, phot_g_mean_mag, pmra, pmdec, parallax FROM {schema_name}.{table_name} limit 10000"
data = db.fetchall_df(query)

# Criar subplots
fig, ((ax1, ax2), (ax3, ax4)) = plt.subplots(2, 2, figsize=(10,  10))

# Plot 1
ax1.scatter(data['ra'], data['dec'], s=1, c='black', alpha=0.2)
ax1.set_xlabel('Right Ascension (deg)')
ax1.set_ylabel('Declination (deg)')

# Plot 2
ax2.scatter(data['phot_bp_mean_mag'] - data['phot_rp_mean_mag'], data['phot_g_mean_mag'], s=1, c='black', alpha=0.2)
ax2.set_xlim(0, 4)
ax2.set_xlabel('BP-RP')
ax2.set_ylabel('G')
ax2.invert_yaxis()

# Plot 3
ax3.scatter(data['pmra'], data['pmdec'], s=1, c='black', alpha=0.2)
ax3.set_xlim(-20, 15)
ax3.set_ylim(-20, 15)
ax3.set_xlabel('Proper Motion in Right Ascension (deg)')
ax3.set_ylabel('Proper Motion in Declination (deg)')

# Plot 4
ax4.scatter(data['phot_g_mean_mag'], data['parallax'], s=1, c='black', alpha=0.2)
ax4.set_ylim(-2, 3)
ax4.set_xlabel('G')
ax4.set_ylabel('Parallax')

plt.tight_layout()
plt.show()
</code></pre>

  
## Informação dos Dados 

O Gaia Data Release 3 (DR3) é composto por diversas tabelas com 152 colunas e mais de 500 mil linhas. Além disso, foram criados 11 índices utilizando o índice espacial q3c. Esses índices estão associados a campos essenciais, como:
- `source_id`
- `ra`
- `dec`
- `phot_g_mean_mag`
- `parallax`
- `pm`
- `pmra`
- `pmdec`
- `duplicated_source`
- `phot_g_mean_flux`
- `phot_variable_flag`



### Gaia Source


A tabela a seguir fornece uma descrição das colunas no Gaia DR3, incluindo seus data types associados.

| Coluna                   | Tipo de Dados | Descrição                                                                                        |
|--------------------------|---------------|--------------------------------------------------------------------------------------------------|
| SOLUTION_ID              | int8          | Identificador da Solução                                                                        |
| DESIGNATION              | varchar(255)  | Designação única da fonte (única em todas as Liberações de Dados)                                |
| SOURCE_ID *              | int8          | Identificador único da fonte (único dentro de uma determinada Liberação de Dados)                |
| RANDOM_INDEX             | int8          | Índice aleatório para uso ao selecionar subconjuntos                                              |
| REF_EPOCH                | float8        | Época de referência (Tempo [Anos Julianos])                                                       |
| RA *                     | float8        | Ascensão reta (Ângulo [graus])                                                                   |
| RA_ERROR                 | float4        | Erro padrão da ascensão reta (Ângulo [milissegundos de arco])                                     |
| DEC *                    | float8        | Declinação (Ângulo [graus])                                                               |
| DEC_ERROR                | float4        | Erro padrão da declinação (Ângulo [milissegundos de arco])                                        |
| PARALLAX *               | float8        | Paralaxe (Ângulo [milissegundos de arco])                                                        |
| PARALLAX_ERROR           | float4        | Erro padrão da paralaxe (Ângulo [milissegundos de arco])                                          |
| PARALLAX_OVER_ERROR      | float4        | Paralaxe dividida pelo seu erro padrão                                                           |
| PM *                     | float4        | Movimento próprio total (Velocidade Angular [milissegundos de arco/ano])                          |
| PMRA *                   | float8        | Movimento próprio na direção da ascensão reta (Velocidade Angular [milissegundos de arco/ano])   |
| PMRA_ERROR               | float4        | Erro padrão do movimento próprio na direção da ascensão reta (Velocidade Angular [milissegundos de arco/ano]) |
| PMDEC *                  | float8        | Movimento próprio na direção da declinação (Velocidade Angular [milissegundos de arco/ano])      |
| PMDEC_ERROR                     | float4       | Erro padrão do movimento próprio na direção da declinação (Velocidade Angular[mas/ano])             |
| RA_DEC_CORR                     | float4       | Correlação entre ascensão reta e declinação (Adimensional)                                         |
| RA_PARALLAX_CORR                | float4       | Correlação entre ascensão reta e paralaxe (Adimensional)                                            |
| RA_PMRA_CORR                    | float4       | Correlação entre ascensão reta e movimento próprio na direção da ascensão reta (Adimensional)       |
| RA_PMDEC_CORR                   | float4       | Correlação entre ascensão reta e movimento próprio na direção da declinação (Adimensional)          |
| DEC_PARALLAX_CORR               | float4       | Correlação entre declinação e paralaxe (Adimensional)                                        |
| DEC_PMRA_CORR                   | float4       | Correlação entre declinação e movimento próprio na direção da ascensão reta (Adimensional)          |
| DEC_PMDEC_CORR                  | float4       | Correlação entre declinação e movimento próprio na direção da declinação (Adimensional)             |
| PARALLAX_PMRA_CORR             | float4       | Correlação entre paralaxe e movimento próprio na direção da ascensão reta (Adimensional)            |
| PARALLAX_PMDEC_CORR            | float4       | Correlação entre paralaxe e movimento próprio na direção da declinação (Adimensional)               |
| PMRA_PMDEC_CORR                 | float4       | Correlação entre movimento próprio na direção da ascensão reta e movimento próprio na direção da declinação (Adimensional) |
| ASTROMETRIC_N_OBS_AL            | int2         | Número total de observações na direção do along-scan (AL)                         |
| ASTROMETRIC_N_OBS_AC            | int2         | Número total de observações na direção do across-scan (AC)                     |
| ASTROMETRIC_N_GOOD_OBS_AL       | int2         | Número de observações válidas na direção do along-scan (AL)                       |
| ASTROMETRIC_N_BAD_OBS_AL        | int2         | Número de observações inválidas na direção do along-scan (AL) |
| ASTROMETRIC_GOF_AL              | float4       | Estatística de qualidade do ajuste do modelo em relação às observações do AL     |
| ASTROMETRIC_CHI2_AL             | float4       | Valor de chi-quadrado de AL                                                                       |
| ASTROMETRIC_EXCESS_NOISE        | float4       | Ruído excessivo da fonte (Ângulo [mas])                                                            |
| ASTROMETRIC_EXCESS_NOISE_SIG    | float4       | Significância do ruído excessivo                                                                  |
| ASTROMETRIC_PARAMS_SOLVED       | int2         | Quais parâmetros foram resolvidos?                                                                |
| ASTROMETRIC_PRIMARY_FLAG        | bool         | Primário ou secundário                                                                            |
| NU_EFF_USED_IN_ASTROMETRY       | float4       | Número efetivo de ondas da fonte usadas na solução astrométrica (Misc[μm<sup>-1</sup>])            |
| PSEUDOCOLOUR                    | float4       | Pseudocor do estimado astrometricamente da fonte (Misc[μm<sup>-1</sup>])                            |
| PSEUDOCOLOUR_ERROR              | float4       | Erro padrão do pseudocor da fonte (Misc[μm<sup>-1</sup>])                                           |
| RA_PSEUDOCOLOUR_CORR            | float4       | Correlação entre ascensão reta e pseudocor                                                         |
| DEC_PSEUDOCOLOUR_CORR           | float4       | Correlação entre declinação e pseudocor                                                           |
| PARALLAX_PSEUDOCOLOUR_CORR      | float4       | Correlação entre paralaxe e pseudocor                                                             |
| PMRA_PSEUDOCOLOUR_CORR          | float4       | Correlação entre movimento próprio na ascensão reta e pseudocor                                     |
| PMDEC_PSEUDOCOLOUR_CORR         | float4       | Correlação entre movimento próprio na declinação e pseudocor                                         |
| ASTROMETRIC_MATCHED_TRANSITS    | int2         | Trânsitos FOV correspondentes usados na solução AGIS                                               |
| VISIBILITY_PERIODS_USED         | int2         | Número de períodos de visibilidade usados na solução Astrométrica                                   |
| ASTROMETRIC_SIGMA5D_MAX         | float4       | O maior semieixo da elipse de erro de 5-d (Ângulo[mas])                                             |
| MATCHED_TRANSITS                | int2         | O número de trânsitos correspondidos a esta fonte                                                   |
| NEW_MATCHED_TRANSITS            | int2         | O número de trânsitos incorporados recentemente em uma fonte existente no ciclo atual              |
| MATCHED_TRANSITS_REMOVED        | int2         | O número de trânsitos removidos de uma fonte existente no ciclo atual                                |
| IPD_GOF_HARMONIC_AMPLITUDE     | float4       | Amplitude do IPD GoF versus ângulo de posição do escaneamento                                       |
| IPD_GOF_HARMONIC_PHASE         | float4       | Fase do IPD GoF versus ângulo de posição do escaneamento (Ângulo[graus])                             |
| IPD_FRAC_MULTI_PEAK            | int2         | Percentual de janelas de IPD bem-sucedidas com mais de um pico                                      |
| IPD_FRAC_ODD_WIN               | int2         | Percentual de trânsitos com janelas truncadas ou portões múltiplos                                  |
| RUWE                            | float4       | Erro de peso unitário renormalizado                                                               |
| SCAN_DIRECTION_STRENGTH_K1       | float4 | Grau de concentração das direções de escaneamento em torno da fonte                                      |
| SCAN_DIRECTION_STRENGTH_K2       | float4 | Grau de concentração das direções de escaneamento em torno da fonte                                      |
| SCAN_DIRECTION_STRENGTH_K3       | float4 | Grau de concentração das direções de escaneamento em torno da fonte                                      |
| SCAN_DIRECTION_STRENGTH_K4       | float4 | Grau de concentração das direções de escaneamento em torno da fonte                                      |
| SCAN_DIRECTION_MEAN_K1           | float4 | Ângulo médio de posição das direções de escaneamento em torno da fonte (Ângulo[graus])                  |
| SCAN_DIRECTION_MEAN_K2           | float4 | Ângulo médio de posição das direções de escaneamento em torno da fonte (Ângulo[graus])                  |
| SCAN_DIRECTION_MEAN_K3           | float4 | Ângulo médio de posição das direções de escaneamento em torno da fonte (Ângulo[graus])                  |
| SCAN_DIRECTION_MEAN_K4           | float4 | Ângulo médio de posição das direções de escaneamento em torno da fonte (Ângulo[graus])                  |
| DUPLICATED_SOURCE *              | bool   | Fonte com múltiplos identificadores de fonte                                                              |
| PHOT_G_N_OBS                     | int2   | Número de observações que contribuem para a fotometria G                                                 |
| PHOT_G_MEAN_FLUX *               | float8 | Fluxo médio na banda G (Fluxo[e<sup>-</sup> s<sup>-1</sup>])                                                                  |
| PHOT_G_MEAN_FLUX_ERROR           | float4 | Erro no fluxo médio na banda G (Fluxo[e<sup>-</sup> s<sup>-1</sup>])                                                         |
| PHOT_G_MEAN_FLUX_OVER_ERROR      | float4 | Fluxo médio na banda G dividido pelo seu erro                                                             |
| PHOT_RP_MEAN_MAG                        | float4 | Magnitude média integrada RP (Magnitude[mag])               |
| PHOT_BP_RP_EXCESS_FACTOR                | float4 | Fator de excesso BP/RP                                     |
| PHOT_BP_N_CONTAMINATED_TRANSITS         | int2   | Número de trânsitos contaminados BP                        |
| PHOT_BP_N_BLENDED_TRANSITS              | int2   | Número de trânsitos mesclados BP                           |
| PHOT_RP_N_CONTAMINATED_TRANSITS         | int2   | Número de trânsitos contaminados RP                        |
| PHOT_RP_N_BLENDED_TRANSITS              | int2   | Número de trânsitos mesclados RP                           |
| PHOT_PROC_MODE                          | int2   | Modo de processamento da fotometria                       |
| BP_RP                                    | float4 | Cor BP - RP (Magnitude[mag])                               |
| BP_G                                     | float4 | Cor BP - G (Magnitude[mag])                                |
| G_RP                                     | float4 | Cor G - RP (Magnitude[mag])                                |
| RADIAL_VELOCITY                         | float4 | Velocidade radial (Velocidade[km s<sup>-1</sup>])                    |
| RADIAL_VELOCITY_ERROR                   |        | Erro de velocidade radial                                          |
| RV_METHOD_USED                          | int2   | Método utilizado para obter a velocidade radial            |
| RV_NB_TRANSITS                               | int2   | Número de trânsitos usados para calcular a velocidade radial      |
| RV_NB_DEBLENDED_TRANSITS                     | int2   | Número de trânsitos válidos que passaram pelo desdobramento       |
| RV_VISIBILITY_PERIODS_USED                   | int2   | Número de períodos de visibilidade usados para estimar a velocidade radial |
| RV_EXPECTED_SIG_TO_NOISE                     | float4 | Relação sinal-ruído esperada na combinação dos espectros usados para obter a velocidade radial |
| RV_RENORMALISED_GOF                          | float4 | Boa adequação renormalizada da velocidade radial                  |
| RV_CHISQ_PVALUE                              | float4 | Valor-p para constância com base em um critério chi-quadrado      |
| RV_TIME_DURATION                             | float4 | Cobertura temporal da série temporal da velocidade radial         |
| RV_AMPLITUDE_ROBUST                          | float4 | Amplitude total na série temporal da velocidade radial após remoção de valores discrepantes |
| RV_TEMPLATE_TEFF                             | float4 | Teff do modelo utilizado para calcular a velocidade radial        |
| RV_TEMPLATE_LOGG                             | float4 | Logg do modelo utilizado para calcular a velocidade radial (GravidadeSuperficial[log cgs]) |
| RV_TEMPLATE_FE_H                             | float4 | [Fe/H] do modelo utilizado para calcular a velocidade radial (Abundâncias[dex]) |
| RV_ATM_PARAM_ORIGIN                          | int2   | Origem dos parâmetros atmosféricos associados ao modelo           |
| VBROAD                                       | float4 | Parâmetro de alargamento da linha espectral (Velocidade[km s<sup>-1</sup>]) |
| VBROAD_ERROR                                 | float4 | Incerteza no alargamento da linha espectral (Velocidade[km s<sup>-1</sup>]) |
| VBROAD_NB_TRANSITS                           | int2   | Número de trânsitos usados para calcular vbroad                  |
| GRVS_MAG                                     | float4 | Magnitude G<sub>RVS</sub> integrada (Magnitude[mag])                         |
| GRVS_MAG_ERROR                               | float4 | Incerteza na magnitude G<sub>RVS</sub> (Magnitude[mag])                      |
| GRVS_MAG_NB_TRANSITS                         | int2   | Número de trânsitos usados para calcular G<sub>RVS</sub>                     |
| RVS_SPEC_SIG_TO_NOISE                        | float4 | Relação sinal-ruído no espectro médio do RVS                                            |
| PHOT_VARIABLE_FLAG *                        | varchar(255) | Sinalizador de variabilidade fotométrica                         |
| L                                            | float8 | Longitude galáctica (Ângulo[graus])                               |
| B                                            | float8 | Latitude galáctica (Ângulo[graus])                                |
| ECL_LON                                      | float8 | Longitude eclíptica (Ângulo[graus])                                 |
| ECL_LAT                                      | float8 | Latitude eclíptica (Ângulo[graus])                                  |
| IN_QSO_CANDIDATES                            | bool   | Sinalizador indicando a disponibilidade de informações adicionais na tabela de candidatos a QSO (Quasi-Stellar Object) |
| IN_GALAXY_CANDIDATES                        | bool   | Sinalizador indicando a disponibilidade de informações adicionais na tabela de candidatos a galáxias                     |
| NON_SINGLE_STAR                              | int2   | Sinalizador indicando a disponibilidade de informações adicionais nas várias tabelas de Estrelas Não Singulares          |
| HAS_XP_CONTINUOUS                            | bool   | Sinalizador indicando a disponibilidade do espectro médio BP/RP em representação contínua para esta fonte              |
| HAS_XP_SAMPLED                               | bool   | Sinalizador indicando a disponibilidade do espectro médio BP/RP na forma amostrada para esta fonte                     |
| HAS_RVS                                      | bool   | Sinalizador indicando a disponibilidade do espectro médio RVS para esta fonte                                         |
| HAS_EPOCH_PHOTOMETRY                         | bool   | Sinalizador indicando a disponibilidade de fotometria de época para esta fonte                                          |
| HAS_EPOCH_RV                                 | bool   | Sinalizador indicando a disponibilidade de velocidade radial de época para esta fonte                                   |
| HAS_MCMC_GSPPHOT                             | bool   | Sinalizador indicando a disponibilidade de amostras MCMC do GSP-Phot para esta fonte                                   |
| HAS_MCMC_MSC                                 | bool   | Sinalizador indicando a disponibilidade de amostras MCMC do MSC para esta fonte                                         |
| IN_ANDROMEDA_SURVEY                          | bool   | Sinalizador indicando que a fonte está presente no Levantamento Fotométrico da Andromeda Gaia (GAPS)                    |
| CLASSPROB_DSC_COMBMOD_QUASAR                | float4 | Probabilidade de ser um quasar (quasares) a partir do DSC-Combmod (dados usados: espectro BP/RP, fotometria, astrometria) |
| CLASSPROB_DSC_COMBMOD_GALAXY                | float4 | Probabilidade de ser uma galáxia a partir do DSC-Combmod (dados usados: espectro BP/RP, fotometria, astrometria)         |
| CLASSPROB_DSC_COMBMOD_STAR                  | float4 | Probabilidade de ser uma estrela única (mas não uma anã branca) a partir do DSC-Combmod (dados usados: espectro BP/RP, fotometria, astrometria) |
| TEFF_GSPPHOT                                 | float4 | Temperatura efetiva do melhor conjunto de bibliotecas do GSP-Phot usando espectros BP/RP (Temperatura[K])                |
| TEFF_GSPPHOT_LOWER                           | float4 | Nível de confiança inferior (16%) da temperatura efetiva do melhor conjunto de bibliotecas do GSP-Phot usando espectros BP/RP (Temperatura[K]) |
| TEFF_GSPPHOT_UPPER                           | float4 | Nível de confiança superior (84%) da temperatura efetiva do melhor conjunto de bibliotecas do GSP-Phot usando espectros BP/RP (Temperatura[K]) |
| LOGG_GSPPHOT                                 | float4 | Gravidade superficial do melhor conjunto de bibliotecas do GSP-Phot usando espectros BP/RP (GravitySurface[log cgs])  |
| LOGG_GSPPHOT_LOWER                           | float4 | Nível de confiança inferior (16%) da gravidade superficial do melhor conjunto de bibliotecas do GSP-Phot usando espectros BP/RP (GravitySurface[log cgs]) |
| LOGG_GSPPHOT_UPPER                           | float4 | Nível de confiança superior (84%) da gravidade superficial do melhor conjunto de bibliotecas do GSP-Phot usando espectros BP/RP (GravitySurface[log cgs]) |
| MH_GSPPHOT                                   | float4 | Abundância de ferro do melhor conjunto de bibliotecas do GSP-Phot usando espectros BP/RP (Abundâncias[dex])                               |
| MH_GSPPHOT_LOWER                             | float4 | Nível de confiança inferior (16%) da abundância de ferro do melhor conjunto de bibliotecas do GSP-Phot usando espectros BP/RP (Abundâncias[dex]) |
| MH_GSPPHOT_UPPER                             | float4 | Nível de confiança superior (84%) da abundância de ferro do melhor conjunto de bibliotecas do GSP-Phot usando espectros BP/RP (Abundâncias[dex]) |
| DISTANCE_GSPPHOT                             | float4 | Distância do melhor conjunto de bibliotecas do GSP-Phot usando espectros BP/RP (Comprimento & Distância[pc])                                |
| DISTANCE_GSPPHOT_LOWER                       | float4 | Nível de confiança inferior (16%) da distância do melhor conjunto de bibliotecas do GSP-Phot usando espectros BP/RP (Comprimento & Distância[pc]) |
| DISTANCE_GSPPHOT_UPPER                       | float4 | Nível de confiança superior (84%) da distância do melhor conjunto de bibliotecas do GSP-Phot usando espectros BP/RP (Comprimento & Distância[pc]) |
| AZERO_GSPPHOT                                | float4 | Extinção monocromática A_0, em 541,4 nm do melhor conjunto de bibliotecas do GSP-Phot usando espectros BP/RP (Magnitude[mag])                |
| AZERO_GSPPHOT_LOWER                          | float4 | Nível de confiança inferior (16%) da extinção monocromática A_0 em 541,4 nm do melhor conjunto de bibliotecas do GSP-Phot usando espectros BP/RP (Magnitude[mag]) |
| AZERO_GSPPHOT_UPPER                          | float4 | Nível de confiança superior (84%) da extinção monocromática A_0 em 541,4 nm do melhor conjunto de bibliotecas do GSP-Phot usando espectros BP/RP (Magnitude[mag]) |
| AG_GSPPHOT                                   | float4 | Extinção na banda G do melhor conjunto de bibliotecas do GSP-Phot usando espectros BP/RP (Magnitude[mag])                                         |
| AG_GSPPHOT_LOWER                             | float4 | Nível de confiança inferior (16%) da extinção na banda G do melhor conjunto de bibliotecas do GSP-Phot usando espectros BP/RP (Magnitude[mag])    |
| AG_GSPPHOT_UPPER                             | float4 | Nível de confiança superior (84%) da extinção na banda G do melhor conjunto de bibliotecas do GSP-Phot usando espectros BP/RP (Magnitude[mag])    |
| EBPMINRP_GSPPHOT                             | float4 | Vermelhamento E(G<sub>BP</sub>  - G<sub>RP</sub> ) do melhor conjunto de bibliotecas do GSP-Phot usando espectros BP/RP (Magnitude[mag])                                |
| EBPMINRP_GSPPHOT_LOWER                       | float4 | Nível de confiança inferior (16%) do vermelhamento E(G<sub>BP</sub>  - G<sub>RP</sub> ) do melhor conjunto de bibliotecas do GSP-Phot usando espectros BP/RP (Magnitude[mag]) |
| EBPMINRP_GSPPHOT_UPPER                       | float4 | Nível de confiança superior (84%) do vermelhamento E(G<sub>BP</sub> - G<sub>RP</sub> ) do melhor conjunto de bibliotecas do GSP-Phot usando espectros BP/RP (Magnitude[mag]) |
| LIBNAME_GSPPHOT                              | varchar(255) | Nome da biblioteca que alcança o maior log-posterior médio nas amostras MCMC e foi usada para derivar os parâmetros GSP-Phot nesta tabela |

\* Colunas indexadas


## Informação Técnicas

### Download e Ingestão de Dados


Na tabela a seguir, são detalhados os períodos de tempo necessários para baixar arquivos Gaia DR3 e ingerir dados em nosso banco de dados.

| Ação | Tempo  |
|---|---|
| Download*| 12:40:02 |
| Ingestão** | 20:38:52 |

\* O tamanho total do download foi de aproximadamente 702G. <br>
\** O processo de ingestão de dados começou às 17:01:49 do dia 5 de abril de 2024, e foi concluído às 13:40:41 do dia 06 de abril de 2024. <br>

<br>

As tabelas abaixo exibem o tempo dedicado à criação de índices para a tabela Gaia DR3 e o número de linhas e colunas.

| Índices | Tempo  |       
|---|---|
| qrc*** | 12:56:49 | 
| source_id | 00:36:23 |
| phot_g_mean_mag | 00:35:31 |
| parallax | 00:38:28 | 
| pm | 00:36:21 |
| pmra | 00:38:34 | 
| pmdec | 00:38:34 | 
| duplicated_source | 00:27:04 | 
| phot_g_mean_flux | 00:41:38 | 
| phot_variable_flag | 00:29:02 | 


\*** Índice espacial q3c

<br>

| Outros | -  |
|---|---|
| Números de linhas | 1.811.709.771 | 
| Números de colunas | 152 | 




## Referências 

 

- https://gea.esac.esa.int/archive/documentation/GDR1/datamodel/
- https://www.cosmos.esa.int/web/gaia/dr3
- https://dblinea.readthedocs.io/en/latest/quickstart.html
