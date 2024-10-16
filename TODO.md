# TODO Benjamin

* [X] ⏳️ Petit support avec le template du devfest : https://docs.google.com/presentation/d/1WIh_YAMMfUjPZAn5VcU8SX6IXH01jP025pKKcTBaxiI/edit#slide=id.g30ad9b2c75e_0_218
* [X] Test la registry docker local et MAJ le step 1 modifier la plateform
* [X] MAJ de la page sur le site publique TOCK avec les requirements
* [X] Relecture des steps avant
* [X] Test hors ligne avec routeur
* [ ] Prépa offline du dossier de modèle ollama, plusieurs variantes et tester. (voir step_1)
* [X] Step 2_1 finire de bouger ce qui lié au RAG qui arrive dans le Gen Sentence


## A voir avec Pierre

* [X] Mettre tout les steps sur une même branch avec step1.md .. ça va vite devenir galère pour s'y retrouver pour les participant de devoir switch de branch à chaque fois et pour nous pour la rédaction et les fichiers annexes.
* --> j'ai mis à jours les branches et cleaner les dossiers inutiles(bot, .idea, pom.xml)

### Step 1
* [X] Retrait de la dépendance mvn, plus besoin on utilise tout en dockerisé ?
* [X] Make file plus besoin avec l'image de tooling python (et nécessite internet)
* [x] Modification au retrait du make file car tout fait en 1 seules ligne de commande au step d'ingestion des données, plus besoin de build d'image car image de tooling
* [X] Indiquer à chaque participant de bien créer un namespace tock avec son nom/prénom ou pseudo ? On pourrait lancer un PAD en début de codelab tout le monde y mets son nom / pseudo pour éviter les collision ça permettra de voir si on a des risques.
* [ ] Création d'un namespace passer par namespace, bien le cocher puis bouton de création de l'app.
* [ ] Récupérer le relative reste path et indiquer dans le step 2 de le coller sur page web.

## Step 2
Ok pour moi rien a voir.

## Step 2_1 : Accélérons l'entrainement avec de l'IAGen
Ce step n'est que sur la génération de phrase, j'ai commencé à décaler tout ce qui est lié au RAG arrive après néanmoins toutes les explication autour de LLM  / prompt / config Ollama / Open AI doivent rester. J'ai entamé le taff mais pas eu le temps de finir.
* [X] Lnagfuse + toc studio OK
* [X] Screenshot à revoir voir les TODO

## Step 2_2
* [X] @benjamin rédiger le step
* [ ] Héberger le dataset sur la machine GPU.
* [X] Revoir IP dans extra host ...

### Step 3
* [X] Image a reprendre d'un step avant
* [X] Ajouter la config d'embedding + envoyer un message qui déclanche le RAG pour tester

### Step 4
* [X] modification image pour matrix ok
* [X] exemple de jealbreak

### Step 5
* [X] explication Token + tokenisatioon
* [ ] Finaliser explication context
* [X] Inviter les gens à jouer avec le prompt
* [X] explication embedding vector

### Step 6
* [X] Brancher le tock reactkit sur le bot avec explication

Tester avec ce dataset ? 
https://www.kaggle.com/datasets/asaniczka/tmdb-movies-dataset-2023-930k-movies
