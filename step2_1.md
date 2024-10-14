# Accélérons l'entrainement avec de l'IAGen


> "Good, adaptation, improvisation. But your weakness is not your technique.", The matrix, Les Wachowski, 1999

Objectifs:
- Comprendre ce qu'est un LLM et un prompt
- Activer et configurer un LLM Engine
- Lancer les 1ères générations de phrase pour entrainer plus rapidement le model
- Découvrir une outil d'observabilité des LLM: Langfuse, créer une organisation et un projet pour obtenir les clés d’API
- Brancher Tock Studio sur Langfuse pour observer les performances du modèle
- Tester le rendu de génération de phrase et regarder le rendu dans langfuse

## Sommaire

- [Introduction](#introduction)
  - [Qu'est-ce que le LLM ?](#qu-est--ce-que-le-llm-?)  

- [Gen AI - Sentence generation](#gen-ai---sentence-generation)
  - [Configuration](#configuration-1)
  - [Ollama](#Ollama)
    - [OpenAI](#OpenAI)
      - [configuration sous Linux](#configuration-sous-linux)
      - [configuration sous MacOs](#configuration-sous-macos)
      - [Tester l'accès à Ollama](#tester-laccès-à-ollama)
    - [Azure OpenAI](#AzureOpenAI)
  

- [ Gen AI - Observability Settings](#gen-ai---observability-settings)
    - [Accéder à Langfuse](#accéder-à-langfuse)
    - [Créer une nouvelle organisation](#créer-une-nouvelle-organisation)
    - [Créer un nouveau projet](#créer-un-nouveau-projet)
    - [Récupérer les clés d'API](#récupérer-les-clés-dapi)


- [Langfuse et Tock Studio](#langfuse-et-tock-studio)
  - [Connecter Tock Studio à Langfuse](#connecter-tock-studio-à-langfuse)
  - [générer des phrases d'entraînement](#générer-des-phrases-dentraînement)
  - [Voir les traces dans Langfuse](#voir-les-traces-dans-langfuse)


- [Ressources](#ressources)

## Introduction
Cette introduction à pour objectif d’expliquer la notion de LLM et du RAG.

## Qu'est-ce que le LLM
Un modèle de langage à grande échelle (LLM, pour Large Language Model) est conçu pour comprendre et générer du texte en
utilisant un vaste ensemble de données d'apprentissage. Cette approche permet d'imiter le langage humain de 
manière convaincante et d'effectuer une variété de tâches liées au texte, telles que répondre à des questions, 
rédiger des textes, traduire des langues et bien plus encore. 

Les LLM sont entraînés sur des centaines de milliards de mots et peuvent comprendre le contexte, l'humour, 
les métaphores et même certains aspects culturels spécifiques.

Les modèles de fondation, quant à eux, sont une catégorie plus large de modèles d'intelligence artificielle qui servent
de base pour de multiples applications et adaptations. Un modèle de fondation peut être un LLM, mais il peut aussi être 
entraîné pour traiter des images, de l'audio, ou des données multimodales (c'est-à-dire des données qui combinent 
plusieurs types de médias). L'idée est de créer un modèle polyvalent qui peut ensuite être personnalisé ou affiné pour 
des tâches spécifiques sans avoir à être ré-entraîné depuis zéro.

<img src="img/fondation_model.png" alt="fondation model">

Exemple non exhaustif de modèles de fondation


Pour utiliser une analogie de la fuséologie, considérez les **LLM** comme des **moteurs de fusée** spécialisés conçus pour 
propulser des missions spécifiques (dans ce cas, le traitement du langage). Les **modèles de fondation**, en revanche, 
sont comme des **plates-formes de lancement** modulaires qui peuvent soutenir différents types de missions — qu'il s'agisse
de lancer un satellite, d'envoyer un rover sur Mars ou de mettre en orbite un télescope spatial.
<img src="img/rocket-apollo-11.png" alt="fusée apollo 11">

Vous en conviendrez que si la plates-forme de lancement (**Fondation Model**) est solide et droite, la fusée (le **LLM**) fait un strike dans l'espace, 
en revanche si le la plates-forme de lancement est bancale et que la fusée par chez le voisin, il risque d'y avoir des dégâts !

En se basant sur des modèles de fondation solides et éprouvés, les développeurs peuvent créer des applications qui utilisent l'IA de manière plus efficace et plus sûre.

## Gen AI - Sentence generation
## Configuration

Le menu **Gen AI**  > **Sentence Generation Settings** permet de configurer la fonctionnalité de génération de phrases d'entraînement pour les bots FAQ.

> Pour accéder à cette page il faut bénéficier du rôle **_botUser_**.

![Génération des phrases - Configuration](img/gen-ai-settings-sentence-generation.png "Ecran de configuration")

Pour activer la fonction de génération de phrases, vous devez choisir :

**Un provider IA :**
- Voir la [liste des fournisseurs d'IA](../providers/gen-ai-provider-llm-and-embedding.md)


**Une température :**
- C’est la température qui apparaîtra par défaut lors de la création des phrases d'entraînement.
- Elle Permet de définir le degré d’inventivité du modèle utilisé pour générer des phrases.
- Elle est situé entre 0 et 1.0.
  - 0 = pas de latitude dans la création des phrases
  - 1.0 = Plus grande latitude dans la création des phrases.

**Un prompt :**
- Encadré dans lequel inclure le prompt qui permet la génération de nouvelles phrases d'entraînement.

**Le nombre de phrases :**
- Défini le nombre de phrases d'entraînement générées par chaque requête.

**Activation :**
- Permet d'activer ou pas la fonctionnalité.


### Ollama
Si vous avez bien suivi l'[étape 1](https://github.com/pi-2r/devfest2024-tock-studio-IA-Gen/tree/step_1) du codelab, Ollama est installé avec tinyOllama sur votre machine.

Avec notre environnement Docker, Ollama doit etre accésible sur le réseau 0.0.0.0.

### Configuration sous Linux

Si vous êtes sur Linux, nous vous invitons à suivre ces étapes.

Pour exposer ollama à toutes les adresses IP, il faut aller modifier le fichier /etc/systemd/system/ollama.service.
Changer les lignes suivantes:    
```markdown
[Service]
ExecStart=/usr/local/bin/ollama serve
User=ollama
Group=ollama
Restart=always
RestartSec=3
#...
Environment="OLLAMA_HOST=0.0.0.0:11434"
```

Puis redémarrer le service ollama avec les commandes suivantes :
```bash
sudo systemctl daemon-reload
sudo systemctl restart ollama.service
```

### Configuration sous MacOs

Sur MacOs pour exposer Ollama sur l'ip 0.0.0.0, suivez les instructions de cette issue : https://github.com/ollama/ollama/issues/3581#issuecomment-2052338405


### Tester l'accès à Ollama

Assurez-vous que Ollama est bien accessible sur l'ip en vous rendant sur l'url suivante : http://0.0.0.0:11434/. 
Vous devriez avoir ce rendu.

<img src="img/ollama-is-runing.png" alt="ollam is runing">

### Configurer Ollama dans le generate sentence

Screenshot de config tock Generate sentence avec ollama.

### OpenAI
Si vous souhaitez utiliser openAI, vous devez vous inscrire sur la plateforme [OpenAI](https://platform.openai.com/docs/introduction)
pour obtenir une clé d'API. Une fois cela fait rendez-vous à cette page [https://platform.openai.com/api-keys](https://platform.openai.com/api-keys) pour générer votre clé d'API.

Dès que vous avez votre clé d'API, vous pouvez la renseigner dans le champ **API Key** et choisir le model (**Model name**) que vous souhaitez utiliser.
Par exemple vous pourriez avoir ce genre de rendu.

TODO crop le screenshot.
<img src="img/rag-settings-example.png" alt="exemple de configuration avec openAI">

### AzureOpenAI
Si vous souhaitez utiliser Azure OpenAI, vous devez vous inscrire sur la plateforme
[Azure OpenAI](https://azure.microsoft.com/fr-fr/products/ai-services/openai-service) et d'avoir un compte profesionnel 
afin d'avoir une clé d'API.
Une fois cela fait, vous pouvez renseigner votre clé d'API dans le champ **API Key** et choisir le model (**Model name**)
que vous souhaitez utiliser.

TODO crop le screenshot.
<img src="img/rag-settings-example-azure.png" alt="exemple de configuration avec Azure OpenAI">

# Gen AI - Observability Settings

- L'observabilité des modèles de langage (LLM Observability) aide à surveiller, d'analyser et de comprendre le comportement des modèles de langage à grande échelle.
- Cela inclut la collecte de données sur leurs performances, la détection d'anomalies et la compréhension des erreurs qu'ils peuvent produire.
- L'objectif est de garantir que ces modèles fonctionnent de manière fiable, transparente, en fournissant des informations qui permettent d'améliorer leur performance et de corriger les problèmes potentiels.

## Accéder à Langfuse
Pour accéder à la plateforme Langfuse, rendez à l'adresse suivante http://localhost:3000/.

<img src="img/langfuse.png" alt="langfuse">

Là vous allez devoir créer un accés, en cliquant sur le bouton **Sign Up** vous allez être redirigé vers la page de création de compte.

<img src="img/langfuse-create-account.png" alt="creation de compte">
Dans notre cas, nous allons utiliser le login **admin**, l'email **admin@app.com** et le mot de passe **password** (ces éléments sont donnés à titre d'exemple, vous pouvez utiliser les vôtres).
Une fois que vous avez rempli les champs, cliquez sur le bouton **Sign Up**.

## Créer une nouvelle organisation

Une fois que vous avez créé votre compte, vous allez être redirigé vers la page principale de l’espace admin qui ressemble à celle-ci.

<img src="img/langfuse-new-organisation.png" alt="nouvelle organisation">

Là vous allez cliquer sur **New Organization**, et lui donner un nom. Dans notre cas, se sera **codelab-tock-2024**, puis de cliquer sur **Create**.
Vous devriez voir votre nouvelle organisation apparaitre dans la liste des organisations, comme ci-dessous.

<img src="img/langfuse-finalize-organization.png" alt="nouvelle organisation">

Ensuite vous allez cliquer sur le bouton **Next**.

## Créer un nouveau projet
Là vous allez définir le nom de votre nouveau projet. Dans notre cas, se sera codelab-tock-project, puis de cliquer sur **Create**.
Vous devriez voir une nouvelle page apparaitre avec les informations de votre projet.

<img src="img/langfuse-new-project-with-all-information.png" alt="nouveau projet">

## Récupérer les clés d'API
Sur la page de votre projet, cliquer sur l’onglet API Keys, pour être rediriger sur la page qui centralise toutes les 
clés d’api de votre projet. La liste étant vide, il faut cliquer sur le bouton **+ Create new API keys**.

<img src="img/langfuse-create-api-keys.png" alt="liste api keys">

Dès lors vous allez voir apparaitre une pop-up qui contient les listes d’API-key, comme dans l’exemple suivant :

<img src="img/langfuse-new-api-keys.png" alt="nouvelle api key">

>Ne fermez pas cette pop-up car vous allez en avoir besoin pour connecter Tock à Langfuse !


## Langfuse et Tock Studio

Dans cette partie, nous allons voir comment connecter Tock Studio à Langfuse pour observer les performances du modèle.


## Connecter Tock Studio à Langfuse

Dans Tock Studio, allez dans le menu de gauche dans **Gen AI** > **Observability settings** pour arriver sur cette page

<img src="img/obersvability-settings.png" alt="tock obersvability settings">

Remplissez les différents champs, de cet écran avec la clé publique et sécrète qui est disponible depuis la pop-up 
dans l’interface Langfuse.

Pour l'url d'accès à Langfuse vous devez renseigner cette url: http://langfuse-server:3000


## Générer des phrases d'entraînement
Pour tester si Langfuse est bien connecté avec Tock Studio, allez dans **Stories & Answers** > **FAQs stories**. 
Là vous allez cliquer sur **+ NEW FAQ STORY**.

<img src="img/new-faq-with-IA.png" alt="new faq story">

Dans l’onglet **QUESTION** et dans le champ comportant le même champ.
Pour l’exemple nous avons cette phrase « bonjour le bot » que nous ajoutons comme question en appuyant sur le **+**.

<img src="img/add-question-faq-stories-with-ai.png" alt="add question">

Dès que cela est fait, cliquez sur l’icône **Generate sentences**.

<img src="img/generate-sentence-with-ai.png" alt="Generate sentence">

Cela va ouvrir une pop-up comme celle-ci vous permettant de générer des mots ou des phrases.

<img src="img/pop-up-generate-sentence.png" alt="generate sentence">

Là vous allez choisir votre phrase que vous avez renseigné juste avant puis choisir les éléments de langages que vous 
souhaitez générer. Une fois cela fait, cliquer sur **GENERATE**

<img src="img/pop-up-generate-sentence-last-step.png" alt="generate sentence">

Aprés quelques secondes vous devriez avoir ce genre de rendu.

<img src="img/result-generat-sentence-ai.png" alt="resultat gen ai">

Vous pouvez tout sélectionner puis valider, chose qui vous ramènera à la page de la FAQ. 
Là vous pourrez voir que les questions générer par l’IA ont été importées.

<img src="img/import-gen-ai-sentence.png" alt="import sentence">

Vous pouvez cliquer ensuite sur l’onglet Answer pour rédiger une réponse, puis cliquer sur **SAVE**.

## Voir les traces dans Langfuse

Maintenant allez sur tableau de bord de Langfuse (http://localhost:3000/) pour voir les traces.

<img src="img/langfuse-dashboard.png" alt="langfuse dashboard">

Dans le menu à gauche allez dans **Tracing** > **Traces** pour arriver sur ce tableau.

<img src="img/langfuse-tracing.png" alt="langfuse tracing">

Là vous allez choisir l’élément dont le nom contient **Sentence Generation**, cliquer sur l’**ID**de ce même élément 
et vous pourrez voir les détails de la génération de phrases ou de mots de cette action.

<img src="img/langfuse-tracing-details.png" alt="langfuse tracing details">

> **Note :** Les éléments varient en fonction de la demande et du type de LLM qui est utilisé pour produire un résultat demandé

## Ressources
| Titre                                                                                  | Lien                                                                                                                                                                                                                                             |
|----------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Attention Is All You Need                                                              | [https://arxiv.org/abs/1706.03762](https://arxiv.org/abs/1706.03762)                                                                                                                                                                             |
| The Illustrated Transformer                                                            | [http://jalammar.github.io/illustrated-transformer/](http://jalammar.github.io/illustrated-transformer/)                                                                                                                                         |
| Evaluating Large Language Model (LLM) systems: Metrics, challenges, and best practices | [https://medium.com/data-science-at-microsoft/evaluating-llm-systems-metrics-challenges-and-best-practices-664ac25be7e5](https://medium.com/data-science-at-microsoft/evaluating-llm-systems-metrics-challenges-and-best-practices-664ac25be7e5) |
| Le Prompt Engineering : L'art de converser avec l'intelligence artificielle            | [https://blog.lesjeudis.com/le-prompt-engineering](https://blog.lesjeudis.com/le-prompt-engineering)                                                                                                                                             |
| Influence response generation with inference parameters                                | [https://docs.aws.amazon.com/bedrock/latest/userguide/inference-parameters.html](https://docs.aws.amazon.com/bedrock/latest/userguide/inference-parameters.html)                                                                                 |
| Demystifying AI Inference Deployments for Trillion Parameter Large Language Models     | [https://developer.nvidia.com/blog/demystifying-ai-inference-deployments-for-trillion-parameter-large-language-models/](https://developer.nvidia.com/blog/demystifying-ai-inference-deployments-for-trillion-parameter-large-language-models/)   |
| An Evaluation of Vector Database Systems: Features, and Use Cases                      | [https://blog.devgenius.io/an-evaluation-of-vector-database-systems-features-and-use-cases-9a90b05eb51f](https://blog.devgenius.io/an-evaluation-of-vector-database-systems-features-and-use-cases-9a90b05eb51f)   | 
| Awesome Foundation Models                                                              | [https://github.com/uncbiag/Awesome-Foundation-Models?tab=readme-ov-file](https://github.com/uncbiag/Awesome-Foundation-Models?tab=readme-ov-file)                                                                                               | 
| Que sont les modèles de fondation ?                                                    | [https://aws.amazon.com/what-is/foundation-models/](https://aws.amazon.com/what-is/foundation-models/)                                                                                                       |
