# Préparons notre base documentaire


> "Morpheus is fighting Neo", The matrix, Les Wachowski, 1999

Objectifs:
- Comprendre comment nous allons faire rentrer une base documentaire dans le bot, concept du Retrieval Augmented Generation
- Récupérer un ensemble documentaire, le transformer dans le bon format
- Utiliser les tool d'ingestion pour le vectoriser et charger dans la base documentaire de notre bot

## Sommaire

- [Qu'est-ce que le RAG ?](#qu-est--ce-que-le-rag-?)
- Récupérer une base documentaire (via kaggle)
- Découverte du tooling et du format
  - Config d'emdedding
- Conversion de la base documentaire dans le bon format avec la langage de votre choix (exemple python)
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


=> todo
=> Récu
