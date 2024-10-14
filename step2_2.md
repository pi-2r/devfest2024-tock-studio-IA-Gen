# Préparons notre base documentaire


> "Morpheus is fighting Neo", The matrix, Les Wachowski, 1999

Objectifs:
- Comprendre comment nous allons faire rentrer une base documentaire dans le bot, concept du Retrieval Augmented Generation
- Récupérer un ensemble documentaire, le transformer dans le bon format
- Utiliser les tool d'ingestion pour le vectoriser et charger dans la base documentaire de notre bot

## Sommaire

- [Qu'est-ce que le RAG ?](#qu-est--ce-que-le-rag-?)
- Récupérer une base documentaire (via kaggle)
- Conversion de la base documentaire dans le bon format avec la langage de votre choix (exemple python)
- Découverte du tooling et du format
  - Config d'emdedding
- Ingestion avec le tooling (cli docker a donner)


## Qu'est-ce que le RAG
<img src="img/rag.png" alt="RAG">

Imaginez que vous lissez un livre, à chaque idée dans le livre, vous allez arracher la ou les pages pour les placer 
dans un coin de votre piece de votre maison ou appartement (ou un chateau, cela depend de votre budget !). 
Quand un(e) ami(e) vous demande par exemple de quoi parle le chapitre 5 du livre que vous avez-lu, vous allez chercher 
dans le coin de la pièce les pages correspondantes pour lui répondre avec vos propres mots.

Là vous avez lu un livre, imaginez maintenant que vous avez lu des milliers de livres, et que vous avez arraché des 
pages de chaque livre pour les placer dans un coin des différentes piéces de votre domicile. Vous devez incollable !

Maintenant, c'est un programme informatique qui va lire les livres, extraire les idées des pages pour les placer non pas 
dans un coin de votre domicile mais dans une base de données dite vectorielle.
L'extraction d'information utilise une techniques de découpage (embedding) pour placer les idées dans la base de 
données vectorielle (une base de données avec des coins).

Plus les idées sont proches, plus elles sont proches dans la base de données vectorielle.

<img src="img/vector_database.png" alt="base de données vectorielles">

On a évoqué les livres, mais cela fonctionne parfaitement avec vos documents numérique tel que des articles, des pages web, des documents PDF, des fichiers texte, des bases de données, etc.

Dés lors, le RAG (Retrieving augmented Generation) est une méthode qui permet de répondre (formulé par une IA ou LLM) à une question en se basant sur une base de connaissances, placées dans une base de données vectorielle.


## Trouver un data set sur kaggle

Vous pouvez rechercher n'importe quel dataset de la thématique qui vous intéresse idéalement un dataset avec des URL ou références vous permettant de relier les documents à des versions en ligne.

Dataset proposé sur Kaggle : [Horror Movies dataset](https://www.kaggle.com/datasets/sujaykapadnis/horror-movies-dataset/data) 

Télécharger le dataset en zip et le décompresser dans le dossier `data/documents_csv/`.
Sinon fichier disponibles en local ici **TODO**.

![Kaggle interface](./img/kaggle_download_as_zip.png "Télécharger le dataset")


## Découverte du tooling et du format

Pour ingérer ce dataset nous allons utiliser le script index_documents.py de TOCK documenté ici, il prend en entrée un csv à 3 colonnes dans ce format. Il va donc falloir convertir votre fichier dans ce format nous vous conseiller également de filtre le fichier et vous limiters à moins de documents / entrée pour accélérer le processus d'ingestion.

Voici un script python qui convertie la dataset donnée en exemple dans ce format de sortie en en gardant qu'une portion. Vous pouvez effectuer ces opération dans le langage de votre choix.

```python
import pandas as pd

# Load the CSV file
df = pd.read_csv('/app/data/documents_csv/horror_movies.csv')

# Set the number of random rows you want to keep
n = 5  # Example value

# Randomly select n rows
df_sampled = df.sample(n=n, random_state=42)  # random_state ensures reproducibility

print(len(df_sampled))

# Keep only the specified columns
columns_to_keep = ['id', 'title', 'overview']
df_filtered = df_sampled[columns_to_keep]

# Map id column to URLs
df_filtered['id'] = df_filtered['id'].map(lambda x: f"https://www.themoviedb.org/movie/{x}")

# Rename columns
df_filtered.rename(columns={'id': 'source', 'overview': 'text', 'title': 'title'}, inplace=True)

# Save the filtered DataFrame to a CSV file
df_filtered.to_csv('data/documents_csv/filtered_horror_movies.csv', index=False, sep='|')

```

If you don't have any local python interpretor you can run it inside the tooling container like this :
```bash
docker run --rm -it \
    -v "$(pwd)/data":/app/data \
    -e NO_PROXY="host.docker.internal,ollama-server,postgres-db,localhost" \
    -e no_proxy="host.docker.internal,ollama-server,postgres-db,localhost" \
    --add-host=host.docker.internal:host-gateway \
    --add-host=ollama-server:192.168.20.41 \
    --add-host=postgres-db:host-gateway \
    tock/llm-indexing-tools:24.9.3 \
    /bin/bash

# Then ..
python /app/data/scripts/transform_horror_movie.py

# Vérifiez le contenu du CSV filtré
cat data/documents_csv/filtered_horror_movies.csv
```

## Ingestion avec le tooling

```bash
docker run --rm -it \
    -v "$(pwd)/data":/app/data \
    -e NO_PROXY="host.docker.internal,ollama-server,postgres-db,localhost" \
    -e no_proxy="host.docker.internal,ollama-server,postgres-db,localhost" \
    --add-host=host.docker.internal:host-gateway \
    --add-host=ollama-server:192.168.20.41 \
    --add-host=postgres-db:host-gateway \
    tock/llm-indexing-tools:24.9.3 \
    /bin/bash

python tock-llm-indexing-tools/index_documents.py data/documents_csv/filtered_horror_movies.csv devfest devfest data/configurations/embeddings_ollama_settings.json data/configurations/vector_store_pgvector_settings.json 5000 -v
```
