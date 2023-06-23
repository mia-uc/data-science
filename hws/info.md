# Estudio sobre el mercado laboral chileno (sector IT), Informe sobre el avance del proyecto de DataScience

- Daniel Orlando Ortiz Pacheco
- Marcelo Rojas
- Claudio Ruiz
- Dubraska Díaz

## Extracción de la información

Al momento en que se entrega este informe se ha extraído información de las paginas:

- https://www.getonbrd.com (1251 ofertas)
- https://www.trabajando.cl (27501 ofertas)
- https://www.laborum.cl (26627 ofertas)

Toda la información se coloco en una base de datos online en Mongo Atlas.

## Transformación y Limpieza

El primer paso de la transformación de la data fue filtrar las ofertas por las que corresponde al sector IT, para lo
cual se utilizaron las clasificaciones que cada una de las paginas tenia previamente realizadas.

Luego, se definio la siguiente estructura centralizada para los datos:

```
class JobOffer
    id: int
    name: str
    origin: "GetOnBoard"|"Laborum"|'Trabajando.cl'
    min_salary: float
    max_salary: float
    currency: 'clp'|'usd'
    seniority: str
    work_modality: str
    contract_type: str
    published_at: datetime
    hiring_organization: str,
    description: str,
    country: str,
    city: str,
    programming_languages: list[str],
    jobs_category= str
    skills: list[str]
```

para esto se creo un nuevo documento en la misma DB, el cual a dia de hoy cuenta con 1814 tuplas.

Finalmente, se ha tenido varios problemas en la ultima fase, el análisis de la información textual de las ofertas.
En el interior de la descripción y del titulo se encuentra la información de la categoría de trabajo a la que pertenece
la oferta (Backend, Frontend, ...), la lista de lenguajes de programación requeridos y la lista de skills excluyentes y no excluyentes
de la oferta.

En este sentido, se desarrollaron modelos de búsqueda textual difusa con las que se reconoce razonablemente bien las categorías y los
lenguajes de programación. Este detector busca las categorías que aprendió previamente de un dataset manualmente construido y los lenguajes
de programación extraídos de un dataset de kaggle.

Pero el casos de la detección de skills no se le pudo aplicar la misma técnica pues los dataset de skills encontrados no tenia la calidad necesaria.
Por lo que actualmente le estamos consultado a GPT cuales son los skills de cada oferta mediante un api de openai.

## Análisis

A este punto ya esta preparado un notebook con un análisis estadísticos de las principales dimensiones y distribuciones del formato centralizado.
Este servirá de punto de partida para un análisis posterior mucho mas profundo. Probablemente este futuro análisis este centrado en la correlación
de los salarios con el resto de los campos.

## Predicción

Ya se ha implementado el algoritmo PageRank con el que se pretende sugerir cual es el skill complementario ideal para un perfil dado. Una vez resuelto
el problema de la detección de los skills en la información textual se realizara una simulación para validar que este modelo sugiere skills que
aumentan las probabilidades de los cv en ser contratados.

Finalmente, en dependencia de la data final es probable que se proponga otro modelo para determinar el rango salarial de una oferta. Esto esta
motivado a que un porciento bajo de las ofertas extraídas son las que publica la banda salaria que ofrecen, pero ese es precisamente el problema,
dada la cantidad de variables, considerando los lenguajes de programación y los skills como columnas binarias, en principio se cree que no
se cuenta con la data suficiente para este análisis
