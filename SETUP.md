# Guide d'Installation & Démarrage

## Pré-requis

- Node.js (v18+) installé
- Python (v3.9+) installé
- ~4GB d'espace disque libre (pour les modèles IA)

## 1. Cloner le Projet

```bash
git clone https://github.com/machichiotte/damage-control-ai.git
cd damage-control-ai
```

## 2. Installation du Backend

### Créer un environnement virtuel (recommandé)

```bash
cd backend
python -m venv venv

# Windows
.\venv\Scripts\activate

# Linux/Mac
source venv/bin/activate
```

### Installer les dépendances

**Important :** L'installation de PyTorch, Transformers et Ultralytics peut prendre plusieurs minutes (~3-4GB).

```bash
# Installer PyTorch (CPU version)
pip install torch torchvision --index-url https://download.pytorch.org/whl/cpu

# Installer les autres dépendances
pip install -r requirements.txt
```

**Note :** Si vous rencontrez des erreurs avec `numpy` ou `ultralytics`, installez-les avec `--only-binary :all:` :

```bash
pip install numpy --only-binary :all:
pip install ultralytics --only-binary :all:
```

### Lancer le backend

```bash
uvicorn main:app --reload
```

Le backend sera accessible sur **http://127.0.0.1:8000**

⚠️ **Au premier lancement**, les modèles IA seront téléchargés depuis Hugging Face :

- Depth Anything (~400MB) : 1-2 minutes
- YOLOv8 nano (~6MB) : quelques secondes
- OWL-ViT (~600MB) : 2-3 minutes

## 3. Installation du Frontend

```bash
cd frontend
npm install
```

### Lancer le frontend

```bash
npm run dev
```

Le frontend sera accessible sur **http://localhost:5173**

## 4. Utilisation

1. Ouvrez http://localhost:5173 dans votre navigateur
2. **Onglet "Analyse d'Image"** :
   - Uploadez une image (drag & drop ou clic)
   - Choisissez une analyse :
     - **🎯 Analyser la profondeur (3D)** : Génère une depth map et visualisation 3D interactive
     - **🔍 Détecter les objets (YOLO)** : Détecte les objets génériques (voitures, personnes)
     - **🧩 Analyser les pièces (Zero-Shot)** : Détecte les pièces spécifiques (roues, pare-chocs, etc.)
3. **Onglet "Analyse de Contrat"** :
   - Uploadez un contrat d'assurance (PDF ou image)
   - Cliquez sur "Analyser le contrat"
   - Consultez les garanties, franchise et plafond extraits
4. **Onglet "Évaluation de Sinistre"** :
   - Sélectionnez une image analysée et un contrat
   - Cliquez sur "Évaluer le sinistre"
   - Obtenez la décision automatique (couvert/non couvert), le coût estimé et le remboursement
5. Admirez les résultats ! 🎨

## 5. Structure des Dossiers

```
/damage_control_ai
├── /backend              # API FastAPI
│   ├── main.py           # Point d'entrée
│   ├── /services         # Services IA (Depth Estimation)
│   ├── /uploads          # Images uploadées (généré automatiquement)
│   └── requirements.txt  # Dépendances Python
└── /frontend             # Application Vue.js
    ├── /src
    │   ├── /components   # Composants Vue (ImageUploader)
    │   ├── App.vue       # Composant principal
    │   └── main.js       # Point d'entrée
    └── package.json      # Dépendances Node.js
```

## 6. Documentation API

Une fois le backend lancé, accédez à la documentation interactive Swagger :
**http://127.0.0.1:8000/docs**

Endpoints disponibles :

- `GET /` - Message de bienvenue
- `GET /health` - Vérification de santé
- `POST /upload` - Upload d'une image
- `POST /analyze/{filename}` - Analyse de profondeur 3D
- `POST /detect/objects/{filename}` - Détection d'objets (YOLO)
- `POST /detect/parts/{filename}` - Détection de pièces (OWL-ViT)
- `POST /upload/contract` - Upload d'un contrat
- `POST /analyze/contract/{filename}` - Analyse de contrat
- `POST /evaluate/claim` - Évaluation complète de sinistre
- `GET /files/{filename}` - Récupération des fichiers

## 🐛 Dépannage

### Le modèle ne se télécharge pas

- Vérifiez votre connexion internet
- Le modèle est stocké dans `~/.cache/huggingface/`

### Erreur "module cv2 has no attribute..."

- Réinstallez OpenCV : `pip uninstall opencv-python && pip install opencv-python`

### Le frontend ne se connecte pas au backend

- Vérifiez que le backend tourne sur le port 8000
- Vérifiez la configuration CORS dans `backend/main.py`
