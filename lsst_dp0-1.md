# Title (nome do survey - release) 

Contents:

- [Introduction](#introduction)
- [How to access](#how-to-access)
- [Dataset information](#dataset-information)
- [Technical information](#technical-information)
- [Reference](#reference)


## Introduction

[EN]

introductory paragraph / general description (here you can have links to the official documentation in the body of the text – list of links at the end, in the Reference section)

[PT-BR]

_parágrafo introdutório / descrição geral  (aqui pode ter links para a documentação oficial no corpo do texto – lista de links no final, na seção Reference)_   


## How to Access 

[EN]

eligibility - public? members?  (highlighted - tag)

Access instructions

- path for the files
- the format of the files
- the name of the database, schema and table and how to access these data. Include some examples of queries.
- examples of how to read the data in both formats: file and table
- examples of using different applications to read the data
  - jupyter (e.g., using dblinea) 
  - cluster apollo
  - etc
    
[PT-BR]   

_elegibilidade - público? membros?  (com destaque - tag)_

_instruções para acesso (...)_

  
## Dataset information 

index of data products belonging to the release, e.g. images, catalogs, maps, etc.

### Data Product 1 

[EN]

description  

[PT-BR]   

descrição  

Exemplo para dados tabulares: 

| Column  | Data Type  | Description  |
|---|---|---|
| colA | bigint  | Lorem ipsilum  |
| colB | bool    | Lorem ipsilum  |
|   |   |   |


### Data Product 2 

[EN]

description  

[PT-BR]   

descrição  

…

### Data Product N

[EN]

description  

[PT-BR]   

descrição  
    

## Technical information

### Download and ingestion details

*If necessary you can add more information in this section eg.: who, when, number of the ticket, source url, tools used to download, md5sum, etc*

| Action | Time  |
|---|---|
| Download | xx:xx:xx |
| Ingestion | xx:xx:xx |
| Indexing* | xx:xx:xx | 

| Others | -  |
|---|---|
| Number of rows | nnnn | 
| Number of columns | nnnn | 


\* time to indexing 12 columns including `ra` and `dec` with spatial q3c index.

Here you can add more information about the process of download and ingestion and how they were done, eg: scripts, commands, etc. The scripts can be added in the idac_dw repository.

## Reference 

lista de links para documentação oficial e outras fontes de informação relevantes 

- ref 1
- ref 2
- ref 3
