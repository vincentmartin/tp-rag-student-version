# TP RAG agentique

Dans ce TP, vous allez construire un agent qui explore un corpus d'articles scientifiques pour répondre à des questions, et mesurer ce qu'il apporte par rapport à un RAG classique. Tout s'exécute sur le GPU gratuit de Google Colab.

En sortie de ce module, vous serez capables de :
- Extraire, découper et indexer des documents PDF dans une base vectorielle (EmbeddingGemma 2, ChromaDB)
- Évaluer une recherche (hit@k, MRR) et l'améliorer (hybride, reranking)
- Construire un RAG de référence, avec citations et abstention
- Construire un agent (Qwen3.5-4B, LangGraph) : outils, budget, mémoire
- Évaluer un agent : résultat et trajectoire
- Protéger un agent de l'injection de prompt indirecte

Prérequis : le TP de recherche d'information et le TP d'initiation aux LLM.

## Travail demandé

Répondre aux questions dans le notebook [tp-rag.ipynb](./tp-rag.ipynb) (exercices 1 à 10, plus deux exercices bonus), en l'ouvrant dans Colab avec le bouton *Open in Colab* en haut du notebook. Le corpus est dans `data/arxiv`, le notebook le télécharge lui-même.

Durée prévue : environ 5 h ; le notebook donne des repères de temps par partie. La notation prendra également en compte la qualité de votre code.

| Besoin | Solution gratuite |
|---|---|
| GPU (principal) | [Google Colab](https://colab.research.google.com) : `Exécution -> Modifier le type d'exécution -> T4 GPU` |
| GPU (si Colab n'a plus de GPU disponible) | [Kaggle Notebooks](https://www.kaggle.com/code) : `Settings -> Accelerator -> GPU T4 x2` |

## Mini-projet : une équipe d'agents de veille scientifique

Le TP construit **un** agent. Le mini-projet en fait une **équipe** : plusieurs agents spécialisés qui se répartissent le travail, se transmettent des résultats et se contrôlent mutuellement. La question centrale : **une équipe d'agents fait-elle mieux qu'un agent unique, et à quel coût ?**

Projet **individuel**. Date de rendu communiquée en cours. Point de départ : le notebook [mini-projet.ipynb](./mini-projet.ipynb), qui fournit les corpus indexés, les outils des spécialistes et les outils d'évaluation. Rien d'autre à installer : tout tourne sur Colab (T4), avec les modèles du TP.

### Le but

À partir d'une question de recherche ouverte, l'équipe produit une **note de synthèse d'une page**, dont chaque affirmation est citée. Par exemple : *« Le corpus contient-il à la fois un article et un brevet sur la mesure de la pression dans le corps humain ? Comparez ce qu'ils mesurent et comment. »*

Trois corpus, de natures différentes :
- `data/arxiv` : les 10 synthèses scientifiques du TP ;
- `data/patents` : 25 brevets européens (description et revendications) ;
- `data/latex` : les bibliographies de 9 de ces synthèses, avec le nombre de citations de chaque référence.

### L'équipe (minimum imposé)

| Agent | Rôle | Outils |
|---|---|---|
| Superviseur | Décompose la question, répartit le travail, décide quand c'est fini | Aucun outil de recherche : il délègue |
| Spécialiste articles | Répond aux sous-questions sur les synthèses | `search_articles`, `list_articles`, `read_page` |
| Spécialiste brevets | Répond aux sous-questions sur les brevets | `search_patents`, `list_patents`, `read_claims` |
| Spécialiste bibliographie | Références, auteurs, citations | `search_references`, `most_cited`, `reference_stats` |
| Rédacteur | Écrit la note, une citation par affirmation | Aucun : il ne voit que ce que les spécialistes ont rapporté |
| Relecteur | Vérifie les citations et renvoie au travail si besoin (une fois au plus) | Lecture des sources |

Un seul LLM joue tous les rôles. Qu'est-ce qui en fait plusieurs agents ? Vous devrez répondre à cette question dans le rapport.

### Ce qui est attendu

1. **Une référence** : l'agent unique du TP, avec les neuf outils réunis, sur les mêmes questions et avec un budget comparable.
2. **Votre équipe**, dans l'architecture de votre choix : superviseur dynamique, chaîne fixe (planifier, chercher, rédiger, relire), ou débat entre deux rédacteurs départagés par le relecteur. Justifiez ce choix.
3. **Une variante**, au choix : une seconde architecture parmi les trois, **ou** un autre format de transmission (extraits bruts, résumé, faits structurés en Pydantic avec leur citation), en mesurant ce qui se perd.
4. **Une ablation du relecteur** : combien de citations fausses restent sans lui ?
5. **Les pannes** : documenter, trace à l'appui, au moins deux échecs de coordination (boucle, tâche déléguée deux fois, agent qui sort de son rôle, information perdue dans une transmission...) et les classer avec la taxonomie MAST de [Cemri et al., *Why Do Multi-Agent LLM Systems Fail?*](https://arxiv.org/abs/2503.13657) (NeurIPS 2025).

### L'évaluation

Vous écrivez **8 à 10 questions** qui obligent à croiser les corpus, avec les sources attendues et les faits qu'une bonne note doit contenir, vérifiés dans les documents (trois exemples sont dans le notebook). Pour chaque système :
- **précision des citations** (`citation_precision`) : la source citée appuie-t-elle la phrase ?
- **couverture** (`fact_coverage`) : part des faits attendus présents dans la note ;
- **rappel des sources** : part des sources attendues consultées ;
- **coût par rôle** (`usage_summary`) : appels au LLM, tokens, secondes.

Le juge est le même modèle que les agents : relisez une partie de ses verdicts à la main et dites ce que vaut la mesure.

### Livrables

- le notebook, exécutable sur Colab de bout en bout ;
- la trace commentée d'une exécution complète (qui a fait quoi, avec quels outils) ;
- un rapport de 4 pages : architecture et choix, résultats chiffrés, analyse des pannes, limites ;
- une soutenance de 10 min avec démo, suivie de questions sur votre code.

### Barème indicatif

| Critère | Points |
|---|---|
| Jeu de questions (croisement des corpus, faits vérifiés) | 3 |
| Équipe d'agents (fonctionne, rôles nets, code lisible) | 5 |
| Comparaisons (référence, variante, ablation), chiffrées | 5 |
| Analyse des pannes (traces, taxonomie) | 4 |
| Rapport et soutenance | 3 |
