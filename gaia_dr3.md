# Gaia Data Release 3 

Contents:

- [Introduction](#introduction)
- [How to access](#how-to-access)
- [Dataset information](#dataset-information)
- [Technical information](#technical-information)
- [Reference](#reference)


## Introduction

[EN]


The Gaia mission, conducted by the European Space Agency (ESA), was launched on December 19, 2013, with the aim of mapping the Milky Way in three dimensions. Currently, it is in its third phase, known as Gaia Data Release 3 (Gaia DR3), launched on June 13, 2022. This release provided high-precision astrometry of the positions and movements of a wide range of celestial objects. Gaia DR3 offers data on sky position, right ascension (α), declination (δ), parallax, and proper motion for approximately 1.46 billion sources.

Gaia DR3 uses three photometric bands to measure the magnitudes of sources: G, G<sub>BP</sub>, and G<sub>RP</sub>, present in previous releases. The major innovation in this phase is the addition of G<sub>RVS</sub> photometry. To access the official Gaia DR3 documentation, you can follow this link: https://gea.esac.esa.int/archive/documentation/GDR3/index.html

[PT-BR]


A missão Gaia, conduzida pela Agência Espacial Europeia (ESA), foi lançada em 19 de dezembro de 2013 com o objetivo de mapear a Via Láctea em três dimensões. Atualmente, encontra-se em sua terceira fase, conhecida como Gaia Data Release 3 (Gaia DR3) lançada em 13 de junho de 2022. Este lançamento proporcionou uma astrometria de alta precisão das posições e movimentos de uma vasta gama de objetos celestes. O Gaia DR3 oferece dados sobre a posição no céu, ascensão reta (α) e declinação (δ), paralaxe e movimento próprio para aproximadamente 1,46 bilhão de fontes.

Gaia DR3 utiliza três bandas fotométricas para medir as magnitudes das fontes: G, G<sub>BP</sub> e G<sub>RP</sub>, presentes nos lançamentos anteriores. A grande novidade desta fase é a adição da fotometria G<sub>RVS</sub>. A documentação oficial do Gaia DR3 está disponível no link: https://gea.esac.esa.int/archive/documentation/GDR3/index.html

## How to Acess 

[EN]

<p>The Gaia DR3 data can be accessed on the website <a href="http://docs.linea.org.br/">http://docs.linea.org.br/</a> under the "Data" section.</p>

<p>The <a href="https://www2.linea.org.br/">LIneA</a> (Interinstitutional Laboratory for e-Astronomy) provides a Python library called <code>dblinea</code> that allows you to access their own database. This library is useful for retrieving data within the <a href="https://jupyter.org/hub">JupyterHub</a> platform. To use this library, you need to have the <code>dblinea</code> package installed in your Python environment. You can install it using the following command:</p>

<pre><code>pip install dblinea</code></pre>

<p>The connection to the database is established through the <code>DBBase</code> class. An example of usage is creating an instance of <code>DBBase</code> to perform queries on the database:</p>

<pre><code># Import the DBBase class from the dblinea package
from dblinea import DBBase

# Create an instance of DBBase. This instance will be used to connect to the database.
db = DBBase()

# Describes the name and type of columns in a table.
db.describe_table("dr3", schema="gaia")
</code></pre>

<p>For the official and comprehensive documentation, visit: <a href="https://dblinea.readthedocs.io/en/latest/quickstart.html">https://dblinea.readthedocs.io/en/latest/quickstart.html</a></p>


[PT-BR]   


Os dados do Gaia DR3 podem ser acessados site http://docs.linea.org.br/ na seção "Dados".

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

Consulte a documentação oficial e completa no site: https://dblinea.readthedocs.io/en/latest/quickstart.html


## Examples

### Data Preview

<pre><code>import numpy as np
import matplotlib.pyplot as plt
from dblinea import DBBase

# Create an instance of DBBase
db = DBBase()

# Specify the table name and schema
table_name = "dr3"
schema_name = "gaia"

# Fetch data from the specified table
query = f"SELECT ra, dec, phot_bp_mean_mag, phot_rp_mean_mag, phot_g_mean_mag, pmra, pmdec, parallax FROM {schema_name}.{table_name}"
data = db.execute(query)

# Create subplots
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

### Search and Analysis of Overdensity in the Vector Point Diagram (VPD) of the Proper Motions:

Here is an example code for searching and analyzing a region in space where the density of objects is higher than the expected average density. Overdensities may indicate star clusters, galaxy groups, or other cosmic structures. The method used for this calculation is ‘gaussian_kde’ (Kernel Density Estimation), which is a technique used to estimate the probability density of a random variable. It works by smoothing the data, creating a “cloud” of Gaussian distributions centered around each data point. These Gaussian distributions are then summed to obtain a smooth estimate of the probability density function.

<pre><code>import numpy as np
import matplotlib.pyplot as plt
from scipy.stats import gaussian_kde
from dblinea import DBBase  # Assuming you have the dblinea package installed

# Create an instance of DBBase
db = DBBase()

# Specify the table name and schema
table_name = "dr3"
schema_name = "gaia"

# Fetch data from the specified table (replace with actual query)
query = f"SELECT pmra, pmdec FROM {schema_name}.{table_name}"
data = db.execute(query)

# Calculate data density using Gaussian Kernel Density Estimation
kde = gaussian_kde(data.T)  # Transpose data for proper shape

# Generate an array of values for the density plot
pmra_values = np.linspace(data[:, 0].min(), data[:, 0].max(), 100)
pmdec_values = np.linspace(data[:, 1].min(), data[:, 1].max(), 100)
pmra_grid, pmdec_grid = np.meshgrid(pmra_values, pmdec_values)
coords = np.vstack([pmra_grid.ravel(), pmdec_grid.ravel()])
density_values = kde(coords).reshape(pmra_grid.shape)

# Create the overdensity plot
plt.figure(figsize=(10, 6))
plt.imshow(density_values.T, origin='lower', aspect='auto', cmap='viridis',
           extent=(pmra_values.min(), pmra_values.max(), pmdec_values.min(), pmdec_values.max()))
plt.colorbar(label='Density')
plt.xlabel('Proper Motion in RA')
plt.ylabel('Proper Motion in DEC')
plt.title('Overdensity in Proper Motion Diagram (VPD)')
plt.show()
</code></pre>
  
  
## Dataset information 

The Gaia Data Release 3 (DR3) comprises several tables with 152 columns and over 500 thousand rows. Additionally, 11 indexes were created using the q3c spatial index. These indexes are associated with essential fields, such as:
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



### Data Product

[EN]
The following table provides a description of the columns in Gaia DR3, including their associated data types.

| Column                          | Data Type    | Description                                                                                        |
|---------------------------------|--------------|----------------------------------------------------------------------------------------------------|
| SOLUTION_ID                     | int8         | Solution Identifier                                                                                |
| DESIGNATION                     | varchar(255) | Unique source designation (unique across all Data Releases)                                        |
| SOURCE_ID *                     | int8         | Unique source identifier (unique within a particular Data Release)                                  |
| RANDOM_INDEX                    | int8         | Random index for use when selecting subsets                                                         |
| REF_EPOCH                       | float8       | Reference epoch (Time[Julian Years])                                                                |
| RA *                            | float8       | Right ascension (Angle[deg])                                                                       |
| RA_ERROR                        | float4       | Standard error of right ascension (Angle[mas])                                                      |
| DEC *                           | float8       | Declination (double, Angle[deg])                                                                    |
| DEC_ERROR                       | float4       | Standard error of declination (Angle[mas])                                                          |
| PARALLAX *                      | float8       | Parallax (Angle[mas])                                                                              |
| PARALLAX_ERROR                  | float4       | Standard error of parallax (Angle[mas])                                                             |
| PARALLAX_OVER_ERROR             | float4       | Parallax divided by its standard error                                                              |
| PM *                            | float4       | Total proper motion (Angular Velocity[mas/year])                                                    |
| PMRA *                          | float8       | Proper motion in right ascension direction (dngular Velocity[mas/year] )                            |
| PMRA_ERROR                      | float4       | Standard error of proper motion in right ascension direction (Angular Velocity[mas/year] )          |
| PMDEC *                         | float8       | Proper motion in declination direction (Angular Velocity[mas/year] )                                |
| PMDEC_ERROR                     | float4       | Standard error of proper motion in declination direction (Angular Velocity[mas/year] )              |
| RA_DEC_CORR                     | float4       | Correlation between right ascension and declination (Dimensionless)                                  |
| RA_PARALLAX_CORR                | float4       | Correlation between right ascension and parallax (Dimensionless)                                     |
| RA_PMRA_CORR                    | float4       | Correlation between right ascension and proper motion in right ascension (Dimensionless)             |
| RA_PMDEC_CORR                   | float4       | Correlation between right ascension and proper motion in declination (Dimensionless)                 |
| DEC_PARALLAX_CORR               | float4       | Correlation between declination and parallax (float, Dimensionless)                                   |
| DEC_PMRA_CORR                   | float4       | Correlation between declination and proper motion in right ascension (Dimensionless)                  |
| DEC_PMDEC_CORR                  | float4       | Correlation between declination and proper motion in declination (Dimensionless)                      |
| PARALLAX_PMRA_CORR             | float4       | Correlation between parallax and proper motion in right ascension (Dimensionless)                     |
| PARALLAX_PMDEC_CORR            | float4       | Correlation between parallax and proper motion in declination (Dimensionless)                         |
| PMRA_PMDEC_CORR                 | float4       | Correlation between proper motion in right ascension and proper motion in declination (Dimensionless) |
| ASTROMETRIC_N_OBS_AL            | int2         | Total number of observations in the along-scan (AL) direction                                 |
| ASTROMETRIC_N_OBS_AC            | int2         | Total number of observations in the across-scan (AC) direction                                 |
| ASTROMETRIC_N_GOOD_OBS_AL       | int2         | Number of good observations in the along-scan (AL) direction                                   |
| ASTROMETRIC_N_BAD_OBS_AL        | int2         | Number of bad observations in the along-scan (AL) direction                                    |
| ASTROMETRIC_GOF_AL              | float4       | Goodness of fit statistic of model wrt along-scan observations                                       |
| ASTROMETRIC_CHI2_AL             | float4       | AL chi-square value                                                                                |
| ASTROMETRIC_EXCESS_NOISE        | float4       | Excess noise of the source (Angle[mas])                                                             |
| ASTROMETRIC_EXCESS_NOISE_SIG    | float4       | Significance of excess noise                                                                       |
| ASTROMETRIC_PARAMS_SOLVED       | int2         | Which parameters have been solved for?                                                              |
| ASTROMETRIC_PRIMARY_FLAG        | bool         | Primary or seconday                                                                                |
| NU_EFF_USED_IN_ASTROMETRY       | float4       | Effective wavenumber of the source used in the astrometric solution (Misc[μm<sup>-1</sup>])         |
| PSEUDOCOLOUR                    | float4       | Astrometrically estimated pseudocolour of the source (Misc[μm<sup>-1</sup>])                         |
| PSEUDOCOLOUR_ERROR              | float4       | Standard error of the pseudocolour of the source (Misc[μm<sup>-1</sup>])                              |
| RA_PSEUDOCOLOUR_CORR            | float4       | Correlation between right ascension and pseudocolour                                                |
| DEC_PSEUDOCOLOUR_CORR           | float4       | Correlation between declination and pseudocolour                                                     |
| PARALLAX_PSEUDOCOLOUR_CORR      | float4       | Correlation between parallax and pseudocolour                                                       |
| PMRA_PSEUDOCOLOUR_CORR          | float4       | Correlation between proper motion in right asension and pseudocolour                                 |
| PMDEC_PSEUDOCOLOUR_CORR         | float4       | Correlation between proper motion in declination and pseudocolour                                     |
| ASTROMETRIC_MATCHED_TRANSITS    | int2         | Matched FOV transits used in the AGIS solution                                                      |
| VISIBILITY_PERIODS_USED         | int2         | Number of visibility periods used in Astrometric solution                                            |
| ASTROMETRIC_SIGMA5D_MAX                 | float4 | The longest semi-major axis of the 5-d error ellipsoid (Angle[mas]) |
| MATCHED_TRANSITS                         | int2   | The number of transits matched to this source               |
| NEW_MATCHED_TRANSITS                     | int2   | The number of transits newly incorporated into an existing source in the current cycle |
| MATCHED_TRANSITS_REMOVED                 | int2   | The number of transits removed from an existing source in the current cycle |
| IPD_GOF_HARMONIC_AMPLITUDE              | float4 | Amplitude of the IPD GoF versus position angle of scan      |
| IPD_GOF_HARMONIC_PHASE                  | float4 | Phase of the IPD GoF versus position angle of scan (Angle[deg]) |
| IPD_FRAC_MULTI_PEAK                     | int2   | Percent of successful-IPD windows with more than one peak  |
| IPD_FRAC_ODD_WIN                        | int2   | Percent of transits with truncated windows or multiple gate |
| RUWE                                     | float4 | Renormalised unit weight error                              |
| SCAN_DIRECTION_STRENGTH_K1              | float4 | Degree of concentration of scan directions across the source |
| SCAN_DIRECTION_STRENGTH_K2              | float4 | Degree of concentration of scan directions across the source |
| SCAN_DIRECTION_STRENGTH_K3              | float4 | Degree of concentration of scan directions across the source |
| SCAN_DIRECTION_STRENGTH_K4              | float4 | Degree of concentration of scan directions across the source |
| SCAN_DIRECTION_MEAN_K1                  | float4 | Mean position angle of scan directions across the source (Angle[deg]) |
| SCAN_DIRECTION_MEAN_K2                  | float4 | Mean position angle of scan directions across the source (Angle[deg]) |
| SCAN_DIRECTION_MEAN_K3                  | float4 | Mean position angle of scan directions across the source (Angle[deg]) |
| SCAN_DIRECTION_MEAN_K4                  | float4 | Mean position angle of scan directions across the source (Angle[deg]) |
| DUPLICATED_SOURCE *                     | bool   | Source with multiple source identifiers                    |
| PHOT_G_N_OBS                             | int2   | Number of observations contributing to G photometry         |
| PHOT_G_MEAN_FLUX *                      | float8 | G-band mean flux (Flux[e<sup>-</sup> s<sup>-1</sup>])                           |
| PHOT_G_MEAN_FLUX_ERROR                  | float4 | Error on G-band mean flux (Flux[e<sup>-</sup> s<sup>-1</sup>])                  |
| PHOT_G_MEAN_FLUX_OVER_ERROR             | float4 | G-band mean flux divided by its error                       |
| PHOT_G_MEAN_MAG *                        | float4 | G-band mean magnitude (Magnitude[mag])                      |
| PHOT_BP_N_OBS                            | int2   | Number of observations contributing to BP photometry        |
| PHOT_BP_MEAN_FLUX                       | float8 | Integrated BP mean flux (Flux[e<sup>-</sup> s<sup>-1</sup>])                    |
| PHOT_BP_MEAN_FLUX_ERROR                 | float4 | Error on the integrated BP mean flux (Flux[e<sup>-</sup> s<sup>-1</sup>])       |
| PHOT_BP_MEAN_FLUX_OVER_ERROR            | float4 | Integrated BP mean flux divided by its error                |
| PHOT_BP_MEAN_MAG                        | float4 | Integrated BP mean magnitude (Magnitude[mag])               |
| PHOT_RP_N_OBS                           | int2   | Number of observations contributing to RP photometry        |
| PHOT_RP_MEAN_FLUX                       | float8 | Integrated RP mean flux (Flux[e<sup>-</sup> s<sup>-1</sup>])                    |
| PHOT_RP_MEAN_FLUX_ERROR                 | float4 | Error on the integrated RP mean flux (Flux[e<sup>-</sup> s<sup>-1</sup>])       |
| PHOT_RP_MEAN_FLUX_OVER_ERROR            | float4 | Integrated RP mean flux divided by its error                |
| PHOT_RP_MEAN_MAG                        | float4 | Integrated RP mean magnitude (Magnitude[mag])               |
| PHOT_BP_RP_EXCESS_FACTOR                | float4 | BP/RP excess factor                                         |
| PHOT_BP_N_CONTAMINATED_TRANSITS         | int2   | Number of BP contaminated transits                          |
| PHOT_BP_N_BLENDED_TRANSITS              | int2   | Number of BP blended transits                               |
| PHOT_RP_N_CONTAMINATED_TRANSITS         | int2   | Number of RP contaminated transits                          |
| PHOT_RP_N_BLENDED_TRANSITS              | int2   | Number of RP blended transits                               |
| PHOT_PROC_MODE                          | int2   | Photometry processing mode                                  |
| BP_RP                                    | float4 | BP - RP colour (Magnitude[mag])                             |
| BP_G                                     | float4 | BP - G colour (Magnitude[mag])                              |
| G_RP                                     | float4 | G - RP colour (Magnitude[mag])                              |
| RADIAL_VELOCITY                         | float4 | Radial velocity (Velocity[km s<sup>-1</sup>])                         |
| RADIAL_VELOCITY_ERROR                   |        | Missing description                                         |
| RV_METHOD_USED                               | int2          | Method used to obtain the radial velocity                   |
| RV_NB_TRANSITS                               | int2          | Number of transits used to compute the radial velocity      |
| RV_NB_DEBLENDED_TRANSITS                     | int2          | Number of valid transits that have undergone deblending     |
| RV_VISIBILITY_PERIODS_USED                   | int2          | Number of visibility periods used to estimate the radial velocity |
| RV_EXPECTED_SIG_TO_NOISE                     | float4        | Expected signal to noise ratio in the combination of the spectra used to obtain the radial velocity |
| RV_RENORMALISED_GOF                          | float4        | Radial velocity renormalised goodness of fit                |
| RV_CHISQ_PVALUE                              | float4        | P-value for constancy based on a chi-squared criterion     |
| RV_TIME_DURATION                             | float4        | Time coverage of the radial velocity time series            |
| RV_AMPLITUDE_ROBUST                          | float4        | Total amplitude in the radial velocity time series after outlier removal |
| RV_TEMPLATE_TEFF                             | float4        | Teff of the template used to compute the radial velocity    |
| RV_TEMPLATE_LOGG                             | float4        | Logg of the template used to compute the radial velocity (GravitySurface[log cgs]) |
| RV_TEMPLATE_FE_H                             | float4        | [Fe/H] of the template used to compute the radial velocityy (Abundances[dex]) |
| RV_ATM_PARAM_ORIGIN                          | int2          | Origin of the atmospheric parameters associated with the template |
| VBROAD                                       | float4        | Spectral line broadening parameter (Velocity[km s<sup>-1</sup>])     |
| VBROAD_ERROR                                 | float4        | Uncertainty on the spectral line broadening (Velocity[km s<sup>-1</sup>]) |
| VBROAD_NB_TRANSITS                           | int2          | Number of transits used to compute vbroad                  |
| GRVS_MAG                                     | float4        | Integrated G<sub>RVS</sub> magnitude (Magnitude[mag])                  |
| GRVS_MAG_ERROR                               | float4        | G<sub>RVS</sub> magnitude uncertainty (Magnitude[mag])                 |
| GRVS_MAG_NB_TRANSITS                         | int2          | Number of transits used to compute G<sub>RVS</sub>                     |
| RVS_SPEC_SIG_TO_NOISE                        | float4        | rvs_spec_sig_to_noise                                       |
| PHOT_VARIABLE_FLAG *                        | varchar(255) | Photometric variability flag                                |
| L                                            | float8        | Galactic longitude (Angle[deg])                             |
| B                                            | float8        | Galactic latitude (Angle[deg])                              |
| ECL_LON                                      | float8        | Ecliptic longitude (Angle[deg])                             |
| ECL_LAT                                      | float8        | Ecliptic latitude (Angle[deg])                              |
| IN_QSO_CANDIDATES                            | bool          | Flag indicating the availability of additional information in the QSO candidates table |
| IN_GALAXY_CANDIDATES                        | bool          | Flag indicating the availability of additional information in the galaxy candidates table |
| NON_SINGLE_STAR                              | int2          | Flag indicating the availability of additional information in the various Non-Single Star tables |
| HAS_XP_CONTINUOUS                            | bool          | Flag indicating the availability of mean BP/RP spectrum in continuous representation for this source |
| HAS_XP_SAMPLED                               | bool          | Flag indicating the availability of mean BP/RP spectrum in sampled form for this source |
| HAS_RVS                                      | bool          | Flag indicating the availability of mean RVS spectrum for this source |
| HAS_EPOCH_PHOTOMETRY                         | bool          | Flag indicating the availability of epoch photometry for this source |
| HAS_EPOCH_RV                                 | bool          | Flag indicating the availability of epoch radial velocity for this source |
| HAS_MCMC_GSPPHOT                             | bool          | Flag indicating the availability of GSP-Phot MCMC samples for this source |
| HAS_MCMC_MSC                                 | bool          | Flag indicating the availability of MSC MCMC samples for this source |
| IN_ANDROMEDA_SURVEY                          | bool          | Flag indicating that the source is present in the Gaia Andromeda Photometric Survey (GAPS) |
| CLASSPROB_DSC_COMBMOD_QUASAR                | float4        | Probability from DSC-Combmod of being a quasar (data used: BP/RP spectrum, photometry, astrometry) |
| CLASSPROB_DSC_COMBMOD_GALAXY                | float4        | Probability from DSC-Combmod of being a galaxy (data used: BP/RP spectrum, photometry, astrometry) |
| CLASSPROB_DSC_COMBMOD_STAR                  | float4        | Probability from DSC-Combmod of being a single star (but not a white dwarf) (data used: BP/RP spectrum, photometry, astrometry) |
| TEFF_GSPPHOT                                 | float4        | Effective temperature from GSP-Phot Aeneas best library using BP/RP spectra (Temperature[K]) |
| TEFF_GSPPHOT_LOWER                           | float4        | Lower confidence level (16%) of effective temperature from GSP-Phot Aeneas best library using BP/RP spectra (Temperature[K]) |
| TEFF_GSPPHOT_UPPER                           | float4        | Upper confidence level (84%) of effective temperature from GSP-Phot Aeneas best library using BP/RP spectra (Temperature[K]) |
| LOGG_GSPPHOT                                 | float4        | Surface gravity from GSP-Phot Aeneas best library using BP/RP spectra (GravitySurface[log cgs])                       |
| LOGG_GSPPHOT_LOWER                           | float4        | Lower confidence level (16%) of surface gravity from GSP-Phot Aeneas best library using BP/RP spectra (GravitySurface[log cgs]) |
| LOGG_GSPPHOT_UPPER                           | float4        | Upper confidence level (84%) of surface gravity from GSP-Phot Aeneas best library using BP/RP spectra (GravitySurface[log cgs]) |
| MH_GSPPHOT                                   | float4        | Iron abundance from GSP-Phot Aeneas best library using BP/RP spectra (Abundances[dex])                               |
| MH_GSPPHOT_LOWER                             | float4        | Lower confidence level (16%) of iron abundance from GSP-Phot Aeneas best library using BP/RP spectra (Abundances[dex]) |
| MH_GSPPHOT_UPPER                             | float4        | Upper confidence level (84%) of iron abundance from GSP-Phot Aeneas best library using BP/RP spectra (Abundances[dex]) |
| DISTANCE_GSPPHOT                             | float4        | Distance from GSP-Phot Aeneas best library using BP/RP spectra (Length & Distance[pc])                                |
| DISTANCE_GSPPHOT_LOWER                       | float4        | Lower confidence level (16%) of distance from GSP-Phot Aeneas best library using BP/RP spectra (Length & Distance[pc]) |
| DISTANCE_GSPPHOT_UPPER                       | float4        | Upper confidence level (84%) of distance from GSP-Phot Aeneas best library using BP/RP spectra (Length & Distance[pc]) |
| AZERO_GSPPHOT                                | float4        | Monochromatic extinction A_0, at 541.4 nm from GSP-Phot Aeneas best library using BP/RP spectra (Magnitude[mag])      |
| AZERO_GSPPHOT_LOWER                          | float4        | Lower confidence level (16%) of monochromatic extinction A_0 at 541.4 nm from GSP-Phot Aeneas best library using BP/RP spectra (Magnitude[mag]) |
| AZERO_GSPPHOT_UPPER                          | float4        | Upper confidence level (84%) of monochromatic extinction A_0 at 541.4 nm from GSP-Phot Aeneas best library using BP/RP spectra (Magnitude[mag]) |
| AG_GSPPHOT                                   | float4        | Extinction in G band from GSP-Phot Aeneas best library using BP/RP spectra (Magnitude[mag])                           |
| AG_GSPPHOT_LOWER                             | float4        | Lower confidence level (16%) of extinction in G band from GSP-Phot Aeneas best library using BP/RP spectra (Magnitude[mag]) |
| AG_GSPPHOT_UPPER                             | float4        | Upper confidence level (84%) of extinction in G band from GSP-Phot Aeneas best library using BP/RP spectra (Magnitude[mag]) |
| EBPMINRP_GSPPHOT                             | float4        | Reddening E(G<sub>BP</sub>  - G<sub>RP</sub> ) from GSP-Phot Aeneas best library using BP/RP spectra (Magnitude[mag])                       |
| EBPMINRP_GSPPHOT_LOWER                       | float4        | Lower confidence level (16%) of reddening E(G<sub>BP</sub>  - G<sub>RP</sub> ) from GSP-Phot Aeneas best library using BP/RP spectra (Magnitude[mag]) |
| EBPMINRP_GSPPHOT_UPPER                       | float4        | Upper confidence level (84%) of reddening E(G<sub>BP</sub>  - G<sub>RP</sub> ) from GSP-Phot Aeneas best library using BP/RP spectra (Magnitude[mag]) |
| LIBNAME_GSPPHOT                              | varchar(255)  | Name of library that achieves the highest mean log-posterior in MCMC samples and was used to derive GSP-Phot parameters in this table |

\* Indexed Columns 





[PT-BR]   
A tabela a seguir fornece uma descrição das colunas no Gaia DR3, incluindo seus tipos de dados ssociados.

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
| RADIAL_VELOCITY_ERROR                   |        | Descrição ausente                                          |
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
| RVS_SPEC_SIG_TO_NOISE                        | float4 | rvs_spec_sig_to_noise                                            |
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


## Technical information

### Download and ingestion details


In the following table, the time periods required for downloading Gaia DR3 files and ingesting data into our database are detailed.

| Action | Time  |
|---|---|
| Download*| 12:40:02 |
| Ingestion** | 20:38:52 |

\* The total download size was approximately 702G. <br>
\** The data ingestion process commenced at 17:01:49 on April 5, 2024, and concluded at 13:40:41 on April 6, 2024. <br>

<br>

The tables below display the time dedicated to creating indexes for the Gaia DR3 table and the number of rows and columns.

| Indexes | Time  |       
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


\*** Spatial q3c index

<br>

| Others | -  |
|---|---|
| Number of rows | 1.811.709.771 | 
| Number of columns | 152 | 




## Reference 

 

- https://gea.esac.esa.int/archive/documentation/GDR1/datamodel/
- https://www.cosmos.esa.int/web/gaia/dr3
- https://dblinea.readthedocs.io/en/latest/quickstart.html
