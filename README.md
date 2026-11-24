# TP RAG agentique

Dans ce TP, vous allez construire un agent qui explore un corpus d'articles scientifiques pour répondre à des questions, et mesurer ce qu'il apporte par rapport à un RAG classique. Tout s'exécute sur le GPU gratuit de Google Colab.

En sortie de ce module, vous serez capables de :
- Extraire, découper et indexer des documents PDF dans une base vectorielle (EmbeddingGemma 2, ChromaDB)
- Évaluer une recherche (hit@k, MRR) et l'améliorer (hybride, reranking)
- Construire un RAG de référence, avec citations et abstention
- Construire un agent (Qwen3.5-4B, LangGraph) : outils, budget, mémoire, vérification des réponses
- Évaluer un agent : résultat et trajectoire
- Protéger un agent de l'injection de prompt indirecte
- (Partie 2) Construire et évaluer un système multi-agents de données

Prérequis : le TP de recherche d'information et le TP d'initiation aux LLM.

## Partie 1 : le RAG agentique

Répondre aux questions dans le notebook [tp-rag.ipynb](./tp-rag.ipynb) (exercices 1 à 16), en l'ouvrant dans Colab avec le bouton *Open in Colab* en haut du notebook. Le corpus est dans `data/arxiv`, le notebook le télécharge lui-même.

La notation prendra également en compte la qualité de votre code.

| Besoin | Solution gratuite |
|---|---|
| GPU (principal) | [Google Colab](https://colab.research.google.com) : `Exécution -> Modifier le type d'exécution -> T4 GPU` |
| GPU (si Colab n'a plus de GPU disponible) | [Kaggle Notebooks](https://www.kaggle.com/code) : `Settings -> Accelerator -> GPU T4 x2` |

## Partie 2 : agents de données

**Exercice 17 : évaluation**


- Suivre le cours [https://www.deeplearning.ai/short-courses/building-and-evaluating-data-agents/](https://www.deeplearning.ai/short-courses/building-and-evaluating-data-agents/)
    - Support de cours [COURS_MULTI_AGENTS_DATA.md](./multi_agent_data/COURS_MULTI_AGENTS_DATA.md) 
- Modifier le notebook [L6](./multi_agent_data/notebooks/L6/L6_évolution_Monitoring,_au_RAG_SQL,_à_l'Optimisation.ipynb) pour 
    1. Ajouter un RAG Local :  à la place de la recherche Snowflake, intégrer les retriever de votre code RAG  et étudier la possibilité d'indexation hiérarchique type ParentDocumentRetriever pouvoir comparer 2 stratégies et ajuster les lengths des 2
    2. Ajouter un module SQL local :  remplacer la partie données structurées par un DuckDB.
    3. Compléter la partie Optimisation
- **Clé API à demander** 


**Conseils**
- Utiliser le provider Groq et les modèles gratuits comme `llama-3.1-8b-instant`
- L'idée est de modifier le nœud `cortex_researcher` Snowflake du graphe pour le remplacer par :
    1. Un RAG Local :  à la place de la recherche Snowflake, intégrer les retrievers de votre code RAG et étudier la possibilité d'indexation hiérarchique type ParentDocumentRetriever pouvoir comparer 2 stratégies et ajuster les lengths des 2.
    2. Du SQL Local :  Remplacer la partie données structurées par un DuckDB.
    3. Stabilisation : Utiliser les feedbacks de TruLens (RAG Triad + GPA) pour ajuster les prompts et la taille des chunks afin que l'agent performe aussi bien en local qu'avec Cortex.

**Exercice 18 : recherche web (Optionnel)**

Remplacer l'outil de recherche Web Tavily (il a tout de même 1000 requetes/mois) par DuckDuckGo ou SearXNG pour être full open-source/local.
