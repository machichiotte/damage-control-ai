# Guide d'Installation & Démarrage

## Pré-requis

- Node.js (v18+) installé
- Python (v3.9+) installé
- ~3GB d'espace disque libre (pour les modèles IA)

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

**Important :** L'installation de PyTorch et Transformers peut prendre plusieurs minutes (~2-3GB).

```bash
# Installer PyTorch (CPU version)
pip install torch torchvision --index-url https://download.pytorch.org/whl/cpu

# Installer les autres dépendances
pip install transformers timm fastapi uvicorn[standard] python-multipart pillow numpy opencv-python python-dotenv aiofiles
```

Ou simplement :

```bash
pip install -r requirements.txt
```

### Lancer le backend

```bash
uvicorn main:app --reload
```

Le backend sera accessible sur **http://127.0.0.1:8000**

⚠️ **Au premier lancement**, le modèle Depth Anything (~400MB) sera téléchargé depuis Hugging Face. Cela peut prendre 1-2 minutes.

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
2. Uploadez une image (drag & drop ou clic)
3. Cliquez sur "🎯 Analyser la profondeur (3D)"
4. Admirez la depth map générée ! 🎨

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
