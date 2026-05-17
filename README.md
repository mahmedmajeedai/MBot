<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:050510,40:0d0d2e,70:1a1a5e,100:050510&height=220&section=header&text=M-Bot&fontSize=80&fontColor=ffffff&fontAlignY=38&fontStyle=bold&desc=Five%20Knowledge%20Sources.%20One%20Conversation.&descSize=20&descAlignY=62&descColor=818cf8" width="100%"/>

<br/>

<p align="center">
  <img src="https://img.shields.io/badge/AIML-Pattern%20Matching-6366f1?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/SpaCy-NLP%20Pipeline-09A3D5?style=for-the-badge&logo=spacy&logoColor=white"/>
  <img src="https://img.shields.io/badge/Neo4j-Knowledge%20Graph-008CC1?style=for-the-badge&logo=neo4j&logoColor=white"/>
  <img src="https://img.shields.io/badge/Prolog-Logical%20Reasoning-8B6914?style=for-the-badge"/>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Django-Web%20Platform-092E20?style=for-the-badge&logo=django&logoColor=white"/>
  <img src="https://img.shields.io/badge/Wikipedia-Live%20Knowledge-black?style=for-the-badge&logo=wikipedia&logoColor=white"/>
  <img src="https://img.shields.io/badge/WordNet-Lexical%20DB-orange?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/MongoDB-Multi%20DB-47A248?style=for-the-badge&logo=mongodb&logoColor=white"/>
</p>

<br/>

> **M-Bot** is a hybrid AI chatbot platform that draws on five distinct knowledge sources simultaneously — AIML pattern matching, SpaCy NLP, Neo4j graph memory, Prolog logical inference, WordNet lexical enrichment, and live Wikipedia scraping — all served through a full-stack Django web application with user authentication, social networking, and personalised conversation history.

<br/>

**[🧠 The Architecture](#-the-architecture) · [🔗 Five Knowledge Sources](#-five-knowledge-sources) · [⚙️ Response Pipeline](#️-response-pipeline) · [🚀 Quickstart](#-quickstart) · [📁 Structure](#-project-structure)**

</div>

---

## 🧠 The Architecture

Most chatbots draw from a single source — a language model, a rule set, or a database. M-Bot draws from five simultaneously, each contributing what it does best.

```
                        User Query
                            │
                    Django Web Platform
                    (Auth + Sessions)
                            │
              ┌─────────────┼─────────────┐
              │             │             │
         AIML Engine    SpaCy NLP    Intent Router
         (pattern match) (entity/pos)  (query type)
              │             │             │
    ┌─────────┼─────────────┼─────────────┼──────────┐
    │         │             │             │          │
  AIML     Prolog       WordNet      Wikipedia    Neo4j
 Templates  Inference   (lexical)   (web scrape)  Graph
    │         │             │             │          │
    └─────────┴─────────────┴─────────────┴──────────┘
                            │
                   Response Assembler
                            │
                   Personalised Response
                  (drawn from user graph)
```

---

## 🔗 Five Knowledge Sources

### 1. AIML — Symbolic Pattern Matching

The primary response engine. M-Bot uses three AIML implementations (`aiml`, `python-aiml`, `pyaiml21`) with a compiled `bot_brain.brn` for fast startup. AIML rules in the `aimlfiles/` folder define conversational patterns — greetings, factual responses, and structured dialogues.

```xml
<category>
  <pattern>WHO ARE YOU</pattern>
  <template>I am M-Bot, an intelligent assistant powered by a semantic knowledge network.</template>
</category>
```

### 2. SpaCy + NLTK + WordNet — Linguistic Intelligence

SpaCy's `en_core_web_sm` model processes every query for Named Entity Recognition (NER), Part-of-Speech tagging, and dependency parsing. NLTK's WordNet integration enriches responses with synonyms, antonyms, definitions, and semantic relationships — so M-Bot understands not just what you typed, but what you meant.

```python
# SpaCy extracts entities and intent
doc = nlp(user_input)
entities = [(ent.text, ent.label_) for ent in doc.ents]

# WordNet enriches with lexical knowledge
synsets = wordnet.synsets(query_word)
```

### 3. Prolog — Logical Inference Engine

When a query requires logical deduction rather than pattern lookup, M-Bot routes to its Prolog knowledge base via `pyswip` (SWI-Prolog) and `pytholog`. This allows M-Bot to reason about facts and derive answers not explicitly stored — combining known facts into new conclusions.

```prolog
% Prolog knowledge base
grandparent(X, Z) :- parent(X, Y), parent(Y, Z).
```

### 4. Wikipedia — Live World Knowledge

For factual queries about real-world entities, M-Bot uses `BeautifulSoup4` and `requests` to scrape relevant Wikipedia summaries at query time. This gives M-Bot access to an encyclopaedic knowledge base without storing it locally.

```python
# Fetch and parse Wikipedia on demand
response = requests.get(wikipedia_url)
soup     = BeautifulSoup(response.content, 'lxml')
summary  = soup.find('p').get_text()
```

### 5. Neo4j — Graph Memory and Personalisation

The most architecturally distinctive layer. Each user's profile, conversation history, and social connections are stored as a property graph in Neo4j via `neomodel` and `django-neomodel`. M-Bot traverses the graph to personalise responses — referencing previous conversations, suggesting known contacts, and adapting tone based on relationship depth.

```cypher
// Retrieve user's conversation history
MATCH (u:User {username: $name})-[:HAD_CONVERSATION]->(c:Conversation)
RETURN c ORDER BY c.timestamp DESC LIMIT 5
```

---

## 🌐 Platform Features

| Feature | Technology | Detail |
|---|---|---|
| 👤 **User Registration and Login** | Django Auth | Secure session-based authentication |
| 💬 **Conversational Interface** | Django views + AJAX | Real-time chat without page reload |
| 🕸️ **Social Network** | Neo4j graph | Users connect with each other; relationships stored as graph edges |
| 🧠 **Hybrid AI Responses** | 5-source pipeline | AIML + SpaCy + Prolog + Wikipedia + WordNet |
| 📚 **Personalisation** | Neo4j retrieval | User context retrieved from graph on every message |
| 🗄️ **Multi-Database** | SQLite + Neo4j + MongoDB | Each database used for its strength |
| 🎨 **Rich Frontend** | HTML + Sass + SCSS + JS | Polished interface with compiled stylesheet pipeline |

---

## ⚙️ Response Pipeline

```
User message received
        │
        ▼
SpaCy NLP Analysis
  Named Entity Recognition
  Part-of-Speech tagging
  Intent classification
        │
        ├──── Conversational query ──→ AIML pattern match
        │                                     │
        ├──── Word-definition query ──→ WordNet lookup
        │                                     │
        ├──── Factual/entity query ───→ Wikipedia scrape
        │                                     │
        ├──── Logical query ──────────→ Prolog inference
        │                                     │
        └──── All queries ────────────→ Neo4j personalisation
                                             │
                                    Response assembled
                                    with user context
                                             │
                                    Delivered to chat UI
                                    History written to Neo4j
```

---

## 📦 Dependency Stack

The `requirements.txt` reveals the full depth of the system:

| Layer | Libraries |
|---|---|
| **AIML** | `aiml`, `python-aiml`, `pyaiml21` |
| **NLP** | `spacy`, `en-core-web-sm`, `nltk` |
| **Prolog** | `pyswip` (SWI-Prolog), `pytholog` |
| **Web scraping** | `beautifulsoup4`, `requests`, `lxml` |
| **Graph DB** | `neo4j`, `neomodel`, `django-neomodel`, `py2neo`, `neobolt` |
| **Document DB** | `pymongo` |
| **Web Framework** | `Django`, `django-crispy-forms` |
| **Frontend** | HTML, Sass, SCSS, JavaScript |
| **Utilities** | `Pillow`, `Js2Py`, `Babel`, `tqdm` |

---

## 📁 Project Structure

```
Ai-Chatbot-using-Neo4j/
│
├── app/                        Django application core
│   ├── views.py                Chat views, auth, social network
│   ├── models.py               User models (SQLite + Neo4j)
│   └── urls.py                 URL routing
│
├── chatbot/                    AI response engine
│   ├── pipeline.py             5-source query router
│   ├── aiml_handler.py         AIML brain interface
│   ├── nlp_handler.py          SpaCy + WordNet processing
│   ├── prolog_handler.py       SWI-Prolog and pytholog interface
│   └── wikipedia_handler.py    Live Wikipedia scraper
│
├── aimlfiles/                  AIML pattern-response rules
├── bot_brain.brn               Compiled AIML brain (fast load)
│
├── manage.py                   Django management CLI
├── db.sqlite3                  User auth and session data
├── requirements.txt            Full dependency list (69 packages)
└── README.md
```

---

## 🚀 Quickstart

### 1. Clone the repository

```bash
git clone https://github.com/mahmedmajeedai/Ai-Chatbot-using-Neo4j.git
cd Ai-Chatbot-using-Neo4j
```

### 2. Create a virtual environment

```bash
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
python -m spacy download en_core_web_sm
python -m nltk.downloader wordnet omw-1.4
```

### 4. Start Neo4j

```bash
# Via Docker
docker run \
  --publish=7474:7474 --publish=7687:7687 \
  --env=NEO4J_AUTH=neo4j/password \
  neo4j:latest
```

### 5. Configure settings

Update `app/settings.py` with your Neo4j credentials:

```python
NEOMODEL_NEO4J_BOLT_URL = 'bolt://neo4j:password@localhost:7687'
```

### 6. Run migrations and start

```bash
python manage.py migrate
python manage.py runserver
```

Open `http://127.0.0.1:8000`, register an account, and start chatting.

---

## 🏆 Why This Is Different

| Typical Chatbot | M-Bot |
|---|---|
| Single LLM or rule set | Five knowledge sources orchestrated together |
| No user memory | Neo4j graph stores and traverses full conversation history |
| Generic responses | Personalised using your graph profile |
| No social layer | Users connect with each other as a social network |
| Static knowledge | Wikipedia scraped live for up-to-date facts |

---

## 📄 License

This project is open source. See [LICENSE](LICENSE) for details.

---

<div align="center">

**Built by [Muhammad Ahmed Majeed](https://github.com/mahmedmajeedai)**

*A multi-paradigm AI chatbot platform — symbolic, statistical, and graph-based intelligence in one system*

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:1a1a5e,50:0d0d2e,100:050510&height=100&section=footer" width="100%"/>

</div>
