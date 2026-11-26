# TP RAG agentique

Dans ce TP, vous allez construire un agent qui explore un corpus d'articles scientifiques pour répondre à des questions, et mesurer ce qu'il apporte par rapport à un RAG classique. Tout s'exécute sur le GPU gratuit de Google Colab.

En sortie de ce module, vous serez capables de :
- Extraire, découper et indexer des documents PDF dans une base vectorielle (EmbeddingGemma 2, ChromaDB)
- Évaluer une recherche (hit@k, MRR) et l'améliorer (hybride, reranking)
- Construire un RAG de référence, avec citations et abstention
- Construire un agent (Qwen3.5-4B, LangGraph) : outils, budget, mémoire, vérification des réponses
- Évaluer un agent : résultat et trajectoire
- Protéger un agent de l'injection de prompt indirecte

Prérequis : le TP de recherche d'information et le TP d'initiation aux LLM.

## Travail demandé

Répondre aux questions dans le notebook [tp-rag.ipynb](./tp-rag.ipynb) (exercices 1 à 16), en l'ouvrant dans Colab avec le bouton *Open in Colab* en haut du notebook. Le corpus est dans `data/arxiv`, le notebook le télécharge lui-même.

La notation prendra également en compte la qualité de votre code.

| Besoin | Solution gratuite |
|---|---|
| GPU (principal) | [Google Colab](https://colab.research.google.com) : `Exécution -> Modifier le type d'exécution -> T4 GPU` |
| GPU (si Colab n'a plus de GPU disponible) | [Kaggle Notebooks](https://www.kaggle.com/code) : `Settings -> Accelerator -> GPU T4 x2` |
