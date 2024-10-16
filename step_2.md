# Les premiers entrainements du bot

[<img src="img/tock-studio-entrainement.jpg"  alt="morpheus">](https://www.youtube.com/watch?v=fhrNgXJ__n8)

> "I know Kung fu.", The matrix, Les Wachowski, 1999


<br/>
<u>Objectifs:</u>

- Découvrir l'interface de Tock Studio
- Discuter avec le bot et voir où les messages sont affichés
- Créer une intention
- Créer une story
- Qualifier une phrase ou un mot

## Sommaire

- [Aperçu de l'interface Tock Studio](#aperçu-de-linterface-tock-studio)


- [Discuter avec bot](#discuter-avec-bot)
- [Ajuster l'URL du bot API](#ajuster-lurl-du-bot-api)
- [Dialoger avec le bot](#dialoger-avec-le-bot)
- [Créer FAQ](#créer-faq)
- [Tester la FAQ](#tester-la-faq)
- [Désactiver la FAQ](#désactiver-la-faq)


- [Ressources](#ressources)
- [Étape suivante](#étape-suivante)

## Aperçu de l'interface Tock Studio

<img src="img/tock-studio-dashboard.png"  alt="tock studio dashboard">

L'interface de Tock Studio est composée de plusieurs parties :

- <u>**Language Understanding**:</u> C'est la partie où l'on peut voir les phrases que le bot a reçu et les intentions qui ont été qualifiées.
- <u>**Stories & Answers**:</u> C'est la partie qui permet de créer des stories. Les stories sont des scénarios qui permettent de répondre à une intention.
- <u>**Gen AI**:</u> C'est la partie qui permet de gérer les paramètres de l'IA Générative.
- <u>**Test**:</u> C'est la partie qui permet de tester votre bot. Vous pouvez tester votre bot en écrivant des phrases et voir comment il vous répond.
- <u>**Analytics**:</u> C'est la partie qui permet de voir les statistiques de votre bot. 
- <u>**Custom Metrics**:</u> C'est la partie qui permet de créer des métriques personnalisées. Vous pouvez créer des métriques personnalisées pour suivre les performances de votre bot.
- <u>**Model Quality**:</u> C'est la partie qui permet de voir la qualité du modèle de votre bot.
- <u>**Settings**:</u> C'est la partie qui permet de configurer votre bot.

## Discuter avec bot

Dans un premier temps, nous allons commencer par discuter avec le bot.
Depuis votre page de test nommée [index.html](index.html)

### Ajuster l'URL du bot API

Si votre bot ne répond pas ajuster l'URL, les messages du front de tchat sont envoyés au Bot API qui expose le 
connecteur web, un API Rest (cf architecture ci-dessous).

Vous trouverez l'URL du connecteur web de votre bot dans le Studio : **Settings** > **Configurations** > déplier le connecteur web > Relative REST path.

Adapter l'url `http://localhost:8080/io/app/devfest2024/web` dans le code au niveau suivant :
```html
<script>
    TockReact.renderChat(document.getElementById('chat'), 'http://localhost:8080/io/app/devfest2024/web', '', {}, { disableSse: true });
    //....
</script>
```

*Si vous utilisez une stack TOCK non présente sur votre machine (celle exposée sur le post du codelab) ajuster le fqdn (Fully Qualified Domain) en remplaçant `http://localhost:8080/io/app/devfest2024/web` par `http://tock.lan:8080/io/VOTRE_NAMESPACE/VOTRE_BOT_ID/web`, le chemin est présent dans le studio sur la config du connecteur web.
### Dialoger avec le bot
Vous pouvez directement écrire le message bonjour et voir ce qu'il vous répond.


<img src="img/not-understand.png"  alt="not-understand">

Si vous recommencez, vous verrez que le bot vous répond toujours la même chose. C'est normal, il ne comprend pas encore la question.


### Créer FAQ

Allez dans la partie **Stories & Answers** > **FAQs stories** pour créer notre première interaction avec le bot.

<img src="img/creation-faqs-stories.png" alt="faq stories">


Cliquez sur le bouton bleu **+NEW FAQ STORY** pour voir apparaitre cet écran

<img src="img/step-1-faqs.png" alt="step1">

Donnez un nom à votre FAQ, pour nous se sera : **demo faq codelab**

<img src="img/title-faqs.png" alt="title">

Cliquez ensuite sur l’onglet **QUESTION**

Dans le champ **Question**, écrivez **bonjour** puis cliquer sur **ADD**

<img src="img/add-question.png" alt="question">

Vous devriez avoir ce rendu :

<img src="img/resultat-add.png" alt= "resultat-add">

Ensuite, cliquez sur **ANSWER** pour ajouter une réponse à la question **bonjour**.
Copiez-collez le texte suivant dans le champ **Answer** :

```
Bonjour le Devfest 2024,
Vous êtes au codelab:  RAG against the Machine 😎🤖: créez votre propre bot IAGen sans Internet
```

Vous devriez avoir ce rendu :

<img src="img/answer-faqs.png" alt="faqs">

Puis cliquez sur le bouton **SAVE** pour enregistrer votre FAQ et ainsi avoir ce rendu :

<img src="img/final-result-faqs.png" alt="final-result-faqs">

### Tester la FAQ

Depuis la page de test [index.html](index.html), si vous retester à nouveau en écrivant **Bonjour**, vous verrez que le 
bot vous répondra ce qu’il a appris.

<img src="img/resultat-faqs-with-chatbot.png" alt="resultat-faqs-with-chatbot">



>Note : si vous souhaitez retrouver les exemples de phrases que vous avez donné à votre bot, vous pouvez aller dans 
> la partie **Language Understanding** > **Search sentences**.


<img src="img/search-sentences-example.png" alt="search-sentences-example">


### Désactiver la FAQ

Pour les besoins du codelab, nous vous conseillons de désactiver votre FAQ. Pour se faire, retournez dans 
**Stories & Answers** > **FAQs stories**, puis cliquez sur « **Disable** » afin de griser ce toggle.

<img src="img/disable-faq.png" alt="disable-faq">

## Ressources

| Information                              | Lien |
|------------------------------------------|------|
| Tock Studio                              | [https://doc.tock.ai/tock/](https://doc.tock.ai/tock/) |
| NLP (Traitement automatique des langues) | [https://fr.wikipedia.org/wiki/Traitement_automatique_des_langues](https://fr.wikipedia.org/wiki/Traitement_automatique_des_langues) |


## Étape suivante

[Étape 3](step_3.md)
