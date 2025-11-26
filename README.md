# DamageControl AI - L'Expert en Sinistres Automatisé

[![GitHub](https://img.shields.io/badge/GitHub-machichiotte%2Fdamage--control--ai-blue?logo=github)](https://github.com/machichiotte/damage-control-ai)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](./LICENSE)

## 🎯 Concept

DamageControl AI est une Progressive Web App (PWA) révolutionnaire qui automatise l'évaluation des dommages automobiles et domestiques. En utilisant l'intelligence artificielle pour l'analyse d'images (profondeur, segmentation, détection) et le traitement du langage naturel (analyse de contrats), elle accélère le processus de déclaration de sinistre (FNOL) et réduit la fraude.

## ✨ Fonctionnalités Actuelles

### ✅ Implémenté (Sprint 1 & 2)

1.  **Upload d'Images Interactif** 📸

    - Drag & drop ou sélection de fichier
    - Prévisualisation instantanée
    - Interface moderne avec animations

2.  **Depth Estimation (Vision 3D)** 🎯
    - Analyse de la gravité des impacts via des cartes de profondeur
    - Modèle IA : Depth Anything (Hugging Face)
    - Visualisation côte à côte (original vs depth map)
    - Statistiques de profondeur (min/max/moyenne)
    - Colormap INFERNO pour meilleure lisibilité

### 🔄 À Venir (Sprint 3 & 4)

3.  **Object Detection** 🔍
    - Identification précise des pièces endommagées (YOLO)
    - Bounding boxes sur l'image
4.  **Analyse de Contrat (NLP)** 📄
    - Extraction automatique des franchises et garanties depuis des PDF
    - Table Question Answering avec TAPAS
5.  **Visualisation 3D Interactive** 🧊

    - Affichage 3D de la depth map avec TresJS
    - Rotation et zoom interactifs

6.  **Rapport Automatisé** 📊
    - Croisement des données visuelles et contractuelles
    - Estimation immédiate du coût

## 🛠 Stack Technique

- **Frontend** : Vue.js 3 (Vite) + TailwindCSS + TresJS (à venir)
- **Backend** : Python (FastAPI)
- **IA/ML** : Hugging Face Transformers
  - Depth Anything (depth estimation) ✅
  - YOLO (object detection) 🔄
  - TAPAS (table QA) 🔄
- **Stockage** : Local (fichiers) pour le développement
- **Déploiement** : Prévu sur Vercel (frontend) + Railway (backend)

## 📂 Structure du Projet

```
/damage_control_ai
├── /frontend          # Application Vue.js
│   ├── /src
│   │   ├── /components
│   │   │   └── ImageUploader.vue  # Composant d'upload
│   │   ├── App.vue
│   │   └── main.js
│   └── package.json
├── /backend           # API FastAPI
│   ├── main.py        # Endpoints REST
│   ├── /services
│   │   └── depth_estimator.py  # Service IA
│   └── requirements.txt
└── /docs              # Documentation
    ├── ARCHITECTURE.md
    ├── SPRINTS.md
    └── SETUP.md
```

## 🏁 Démarrage Rapide

### Prérequis

- Node.js 18+
- Python 3.9+
- ~3GB d'espace disque (modèles IA)

### Installation

**Frontend :**

```bash
cd frontend
npm install
npm run dev
```

👉 Frontend accessible sur http://localhost:5173

**Backend :**

```bash
cd backend
pip install -r requirements.txt
uvicorn main:app --reload
```

👉 Backend accessible sur http://127.0.0.1:8000

⚠️ **Note :** Au premier lancement, le modèle Depth Anything (~400MB) sera téléchargé depuis Hugging Face.

### Documentation API

Documentation interactive Swagger : http://127.0.0.1:8000/docs

## 🎨 Captures d'écran

_(À venir : Screenshots de l'interface et des depth maps)_

## 📊 Progression du Projet

- ✅ **Sprint 1** : Fondations & Infrastructure (100%)
- ✅ **Sprint 2** : Vision & 3D - Depth Estimation (100%)
- 🔄 **Sprint 3** : Intelligence Contractuelle (0%)
- 🔄 **Sprint 4** : UI/UX Premium & Finalisation (0%)

**Progression totale : 50%**

Voir [SPRINTS.md](./SPRINTS.md) pour plus de détails.

## 📖 Documentation

- [Architecture](./ARCHITECTURE.md) - Détails techniques et choix d'architecture
- [Sprints](./SPRINTS.md) - Planification et roadmap du projet
- [Setup](./SETUP.md) - Guide d'installation détaillé

## 🎯 Pourquoi ce projet ?

Ce projet démontre des compétences avancées en :

- **Full-Stack Development** : Vue.js + Python/FastAPI
- **Intelligence Artificielle** : Intégration de modèles Hugging Face
- **Computer Vision** : Depth Estimation (top 1% des développeurs)
- **UX/UI moderne** : Design premium avec Tailwind
- **Architecture propre** : Services, API REST, gestion d'état

## 🤝 Contribution

Les contributions sont les bienvenues ! N'hésitez pas à ouvrir une issue ou une pull request.

## 📝 License

MIT License - voir [LICENSE](./LICENSE)

---

**Développé par** [@machichiotte](https://github.com/machichiotte) | **2025**
