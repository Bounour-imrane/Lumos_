# 🪄 HP-RAG-Quiz

Générateur de quiz intelligent sur l'univers Harry Potter, basé sur une architecture **RAG (Retrieval-Augmented Generation)**. Le système indexe l'intégralité des livres, récupère les passages pertinents selon un thème donné, et génère des quiz à choix multiples avec justifications ancrées dans le texte original.

## ✨ Fonctionnalités

- 📚 Ingestion et indexation des 7 livres Harry Potter
- 🔍 Recherche sémantique par personnage, chapitre, thème ou événement
- 🧠 Génération de quiz (QCM) via LLM avec sortie JSON structurée
- ✅ Réponses justifiées par des extraits réels des livres (pas d'hallucination)
- 🌐 API REST (FastAPI) prête à être consommée par un frontend
- 🐳 Conteneurisé et déployable en quelques commandes

## 🏗️ Architecture

```
┌─────────────┐     ┌──────────────┐     ┌─────────────────┐
│   Livres     │ ──▶ │   Ingestion   │ ──▶ │   Vectorstore    │
│ (PDF/EPUB)   │     │  (chunking)   │     │   (ChromaDB)     │
└─────────────┘     └──────────────┘     └─────────────────┘
                                                    │
                                                    ▼
┌─────────────┐     ┌──────────────┐     ┌─────────────────┐
│  Frontend    │ ◀── │   FastAPI     │ ◀── │  RAG + Génération │
│ (Streamlit)  │     │   (API REST)  │     │  de quiz (LLM)    │
└─────────────┘     └──────────────┘     └─────────────────┘
```

## 📁 Structure du projet

```
hp-rag-quiz/
├── data/
│   ├── raw/                # Livres bruts (PDF/EPUB/TXT)
│   └── processed/          # Chunks nettoyés (JSON/Parquet)
├── src/
│   ├── ingestion/          # Extraction texte + découpage en chunks
│   ├── embeddings/         # Génération et stockage des vecteurs
│   ├── rag/                # Logique de retrieval
│   ├── quiz/               # Génération de quiz (prompts + parsing)
│   ├── api/                # Endpoints FastAPI
│   └── config.py           # Configuration centralisée (Pydantic Settings)
├── vectorstore/             # Base ChromaDB persistante (générée)
├── frontend/                # Application Streamlit ou React
├── tests/                   # Tests unitaires et d'intégration
├── .env.example              # Variables d'environnement à dupliquer
├── .gitignore
├── pyproject.toml            # Dépendances (géré avec uv)
├── Dockerfile
├── docker-compose.yml
└── README.md
```

## 🚀 Installation

### Prérequis

- Python 3.11+
- [uv](https://github.com/astral-sh/uv) (gestionnaire d'environnement/dépendances)
- Une clé API OpenAI (ou Azure OpenAI)
- Docker (optionnel, pour le déploiement)

### Étapes

```bash
# Cloner le repo
git clone https://github.com/<ton-user>/hp-rag-quiz.git
cd hp-rag-quiz

# Créer l'environnement et installer les dépendances
uv venv
uv sync

# Configurer les variables d'environnement
cp .env.example .env
# Puis éditer .env avec ta clé API
```

### Variables d'environnement (`.env`)

```env
OPENAI_API_KEY=sk-...
EMBEDDING_MODEL=text-embedding-3-small
LLM_MODEL=gpt-4o-mini
VECTORSTORE_PATH=./vectorstore
CHUNK_SIZE=700
CHUNK_OVERLAP=150
```

## 🛠️ Utilisation

### 1. Ingestion des livres

Placer les fichiers des livres dans `data/raw/`, puis lancer :

```bash
uv run python -m src.ingestion.run
```

Cette étape extrait le texte, le découpe en chunks et génère les embeddings dans ChromaDB.

### 2. Lancer l'API

```bash
uv run uvicorn src.api.main:app --reload --port 8000
```

L'API est accessible sur `http://localhost:8000`, avec documentation interactive sur `http://localhost:8000/docs`.

### 3. Générer un quiz (exemple)

```bash
curl -X POST http://localhost:8000/generate-quiz \
  -H "Content-Type: application/json" \
  -d '{"theme": "Le Tournoi des Trois Sorciers", "nb_questions": 5, "difficulty": "medium"}'
```

### 4. Lancer le frontend

```bash
uv run streamlit run frontend/app.py
```

## 🐳 Déploiement avec Docker

```bash
docker-compose up --build
```

Cela démarre l'API et le frontend dans des conteneurs séparés. Voir `docker-compose.yml` pour la configuration des ports et volumes.

## 🧪 Tests

```bash
uv run pytest tests/ -v
```

## 📊 Stack technique

| Composant | Technologie |
|---|---|
| Langage | Python 3.11+ |
| Framework RAG | LangChain |
| Vectorstore | ChromaDB |
| Embeddings | OpenAI `text-embedding-3-small` |
| LLM | GPT-4o-mini (ou Azure OpenAI) |
| API | FastAPI |
| Frontend | Streamlit |
| Conteneurisation | Docker / Docker Compose |# 🪄 HP-RAG-Quiz

Générateur de quiz intelligent sur l'univers Harry Potter, basé sur une architecture **RAG (Retrieval-Augmented Generation)**. Le système indexe l'intégralité des livres, récupère les passages pertinents selon un thème donné, et génère des quiz à choix multiples avec justifications ancrées dans le texte original.

## ✨ Fonctionnalités

- 📚 Ingestion et indexation des 7 livres Harry Potter
- 🔍 Recherche sémantique par personnage, chapitre, thème ou événement
- 🧠 Génération de quiz (QCM) via LLM avec sortie JSON structurée
- ✅ Réponses justifiées par des extraits réels des livres (pas d'hallucination)
- 🌐 API REST (FastAPI) prête à être consommée par un frontend
- 🐳 Conteneurisé et déployable en quelques commandes

## 🏗️ Architecture

```
┌─────────────┐     ┌──────────────┐     ┌─────────────────┐
│   Livres     │ ──▶ │   Ingestion   │ ──▶ │   Vectorstore    │
│ (PDF/EPUB)   │     │  (chunking)   │     │   (ChromaDB)     │
└─────────────┘     └──────────────┘     └─────────────────┘
                                                    │
                                                    ▼
┌─────────────┐     ┌──────────────┐     ┌─────────────────┐
│  Frontend    │ ◀── │   FastAPI     │ ◀── │  RAG + Génération │
│ (Streamlit)  │     │   (API REST)  │     │  de quiz (LLM)    │
└─────────────┘     └──────────────┘     └─────────────────┘
```

## 📁 Structure du projet

```
hp-rag-quiz/
├── data/
│   ├── raw/                # Livres bruts (PDF/EPUB/TXT)
│   └── processed/          # Chunks nettoyés (JSON/Parquet)
├── src/
│   ├── ingestion/          # Extraction texte + découpage en chunks
│   ├── embeddings/         # Génération et stockage des vecteurs
│   ├── rag/                # Logique de retrieval
│   ├── quiz/               # Génération de quiz (prompts + parsing)
│   ├── api/                # Endpoints FastAPI
│   └── config.py           # Configuration centralisée (Pydantic Settings)
├── vectorstore/             # Base ChromaDB persistante (générée)
├── frontend/                # Application Streamlit ou React
├── tests/                   # Tests unitaires et d'intégration
├── .env.example              # Variables d'environnement à dupliquer
├── .gitignore
├── pyproject.toml            # Dépendances (géré avec uv)
├── Dockerfile
├── docker-compose.yml
└── README.md
```

## 🚀 Installation

### Prérequis

- Python 3.11+
- [uv](https://github.com/astral-sh/uv) (gestionnaire d'environnement/dépendances)
- Une clé API OpenAI (ou Azure OpenAI)
- Docker (optionnel, pour le déploiement)

### Étapes

```bash
# Cloner le repo
git clone https://github.com/<ton-user>/hp-rag-quiz.git
cd hp-rag-quiz

# Créer l'environnement et installer les dépendances
uv venv
uv sync

# Configurer les variables d'environnement
cp .env.example .env
# Puis éditer .env avec ta clé API
```

### Variables d'environnement (`.env`)

```env
OPENAI_API_KEY=sk-...
EMBEDDING_MODEL=text-embedding-3-small
LLM_MODEL=gpt-4o-mini
VECTORSTORE_PATH=./vectorstore
CHUNK_SIZE=700
CHUNK_OVERLAP=150
```

## 🛠️ Utilisation

### 1. Ingestion des livres

Placer les fichiers des livres dans `data/raw/`, puis lancer :

```bash
uv run python -m src.ingestion.run
```

Cette étape extrait le texte, le découpe en chunks et génère les embeddings dans ChromaDB.

### 2. Lancer l'API

```bash
uv run uvicorn src.api.main:app --reload --port 8000
```

L'API est accessible sur `http://localhost:8000`, avec documentation interactive sur `http://localhost:8000/docs`.

### 3. Générer un quiz (exemple)

```bash
curl -X POST http://localhost:8000/generate-quiz \
  -H "Content-Type: application/json" \
  -d '{"theme": "Le Tournoi des Trois Sorciers", "nb_questions": 5, "difficulty": "medium"}'
```

### 4. Lancer le frontend

```bash
uv run streamlit run frontend/app.py
```

## 🐳 Déploiement avec Docker

```bash
docker-compose up --build
```

Cela démarre l'API et le frontend dans des conteneurs séparés. Voir `docker-compose.yml` pour la configuration des ports et volumes.

## 🧪 Tests

```bash
uv run pytest tests/ -v
```

## 📊 Stack technique

| Composant | Technologie |
|---|---|
| Langage | Python 3.11+ |
| Framework RAG | LangChain |
| Vectorstore | ChromaDB |
| Embeddings | OpenAI `text-embedding-3-small` |
| LLM | GPT-4o-mini (ou Azure OpenAI) |
| API | FastAPI |
| Frontend | Streamlit |
| Conteneurisation | Docker / Docker Compose |
| Gestion des dépendances | uv |

## 🗺️ Roadmap

- [ ] Ingestion des 7 livres avec métadonnées (livre, chapitre)
- [ ] Pipeline de retrieval avec filtres par livre/personnage
- [ ] Génération de quiz avec sortie JSON validée (Pydantic)
- [ ] API FastAPI avec endpoints documentés
- [ ] Frontend Streamlit fonctionnel
- [ ] Dockerisation complète
- [ ] Déploiement (Railway / Render / Azure)
- [ ] Tests unitaires (ingestion, retrieval, parsing quiz)
- [ ] CI/CD avec GitHub Actions

## ⚖️ Note sur les droits d'auteur

Ce projet est un exercice technique personnel à but pédagogique (apprentissage du RAG et du GenAI engineering). Les textes des livres Harry Potter sont protégés par le droit d'auteur (J.K. Rowling / éditeurs) : ils ne sont pas inclus dans ce dépôt et ne doivent pas être redistribués. Utiliser uniquement des exemplaires possédés légalement, à des fins d'usage personnel et non commercial.

## 📄 Licence

MIT — voir le fichier `LICENSE`
| Gestion des dépendances | uv |

## 🗺️ Roadmap

- [ ] Ingestion des 7 livres avec métadonnées (livre, chapitre)
- [ ] Pipeline de retrieval avec filtres par livre/personnage
- [ ] Génération de quiz avec sortie JSON validée (Pydantic)
- [ ] API FastAPI avec endpoints documentés
- [ ] Frontend Streamlit fonctionnel
- [ ] Dockerisation complète
- [ ] Déploiement (Railway / Render / Azure)
- [ ] Tests unitaires (ingestion, retrieval, parsing quiz)
- [ ] CI/CD avec GitHub Actions