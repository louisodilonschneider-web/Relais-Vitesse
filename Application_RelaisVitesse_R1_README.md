# ⚡ Sprint EPS — Application de gestion d'athlétisme

Application web single-page (HTML + CSS + JS) destinée aux enseignants d'EPS pour gérer les séquences de **course de relais / sprint 60m**.  
Fonctionne entièrement **dans le navigateur** — aucun serveur requis. Les données sont persistées en `localStorage`.

---

## 🚀 Démarrage rapide

1. Télécharger les 3 fichiers dans le même dossier :
   - `index.html`
   - `style.css`
   - `app.js`
2. Ouvrir `index.html` dans un navigateur (Chrome, Firefox, Edge…)
3. Aucune installation, aucune connexion internet requise *(les polices se chargent depuis Google Fonts au premier lancement)*

---

## 📋 Fonctionnalités

### Gestion des classes & élèves
- Créer plusieurs classes
- Ajouter des élèves manuellement (prénom + nom)
- Importer via **CSV** (`prénom,nom` — une ligne par élève)
- Supprimer élèves ou classes

### Gestion des leçons
- Créer une leçon par date pour chaque classe
- Saisie des **présences / absences / blessés** pour chaque leçon (clic cyclique)
- Les onglets de saisie ne proposent que les élèves présents

---

## 🗂️ Onglets

### 1. Test individuel
| Fonctionnalité | Détail |
|---|---|
| Distance | Paramétrable (m) |
| Type de départ | Arrêté / Lancé |
| Passages | 1 à 5 par élève |
| Classement | Par vitesse (km/h), meilleur passage |
| Historique | Tous les passages, horodatés, modifiables |

### 2. Entraînement 60m
| Fonctionnalité | Détail |
|---|---|
| Binôme | Donneur / Receveur |
| Chronomètreur | Sélectionnable parmi les élèves présents |
| Zone de transmission | Configurable (défaut 30–50m), découpée en 2 ou 4 sous-zones |
| Marque / repère | Mode **prédéfini** (liste évolutive) ou **nombre de pieds** |
| Qualité de transmission | Transmis / Non transmis / Départ anticipé / Bouchon / Désorganisée |
| Validation chrono | Validé (vert) / En construction (orange) / Non validé (rouge) |
| Classement | Meilleur temps par binôme, vitesse, marque, nb passages, moy. chronos |
| Historique | Tous les passages, horodatés, modifiables |

#### Couleur du chronométreur
| Couleur | Condition |
|---|---|
| 🟢 Vert | ≥ 3 validations dans la séquence |
| 🟠 Orange | Au moins 1 "en construction" |
| 🔴 Rouge | Au moins 1 "non validé" |
| ⚪ Noir | Aucune appréciation |

### 3. Passage 60m
- Basé sur la même logique que l'entraînement
- **Suivi des passages** : affiche en rouge les élèves n'ayant pas encore passé (1 passage obligatoire, objectif 2–4)
- Classement binômes
- Historique modifiable

### 4. Battle
| Fonctionnalité | Détail |
|---|---|
| Configuration | Nombre de tours, distance par tour |
| Équipes | 2+ équipes, 2+ élèves par équipe, ordre positionné |
| Chronomètre | Démarrage commun, enregistrement par tour |
| Qualité | Saisie de la qualité de transmission par tour et par équipe |
| Édition | Modification des temps et qualités après coup |
| Sauvegarde | Enregistrement de la battle dans l'historique |

### 5. Profils élèves
Pour chaque élève :
- Statistiques en tant que **donneur**, **receveur**, **chronomètreur**
- Nombre total de passages, répartition par leçon
- Meilleurs temps par partenaire
- Historique des tests individuels
- Statut chronomètreur coloré

---

## 💾 Données

Les données sont stockées en `localStorage` sous la clé préfixée `sprint_`.

### Exporter / Sauvegarder
Ouvrir la console du navigateur (`F12 → Console`) et taper :
```js
copy(JSON.stringify(Object.entries(localStorage).filter(([k])=>k.startsWith('sprint_')).reduce((a,[k,v])=>(a[k]=JSON.parse(v),a),{}), null, 2))
```
Coller dans un fichier `.json` pour archiver.

### Restaurer
```js
const data = /* coller le JSON ici */;
Object.entries(data).forEach(([k,v]) => localStorage.setItem(k, JSON.stringify(v)));
location.reload();
```

---

## 🔮 Migration vers Firebase (optionnel)

L'application est conçue pour être migrée facilement :
- Remplacer les fonctions `DB.get(k)` / `DB.set(k, v)` dans `app.js` par des appels Firestore
- Exemple :
```js
// Avant (localStorage)
const DB = {
  get: (k) => JSON.parse(localStorage.getItem('sprint_' + k) || 'null'),
  set: (k, v) => localStorage.setItem('sprint_' + k, JSON.stringify(v)),
};

// Après (Firestore)
import { doc, getDoc, setDoc } from "firebase/firestore";
const DB = {
  get: async (k) => (await getDoc(doc(db, 'sprint', k))).data() ?? null,
  set: async (k, v) => setDoc(doc(db, 'sprint', k), v),
};
```

---

## 📁 Structure des fichiers

```
sprint-eps/
├── index.html   # Structure HTML, modales, onglets
├── style.css    # Palette claire, typographie DM Sans / DM Mono
└── app.js       # Logique métier, stockage, rendu
```

---

## 🎨 Charte graphique

| Élément | Valeur |
|---|---|
| Police texte | DM Sans (Google Fonts) |
| Police chiffres | DM Mono |
| Couleur primaire | `#2563eb` (bleu ardoise) |
| Couleur accent | `#f97316` (orange vif) |
| Fond | `#f7f8fa` (blanc cassé) |
| Cartes | `#ffffff` avec bordure légère |

---

## 🗺️ Roadmap possible

- [ ] Export PDF / Excel des classements
- [ ] Mode hors-ligne complet (Service Worker / PWA)
- [ ] Synchronisation multi-appareils via Firebase
- [ ] QR Code pour saisie autonome par les élèves
- [ ] Graphiques d'évolution par élève (Chart.js)
- [ ] Comparaison entraînements vs passages

---

## 📄 Licence

Projet libre à usage pédagogique.
