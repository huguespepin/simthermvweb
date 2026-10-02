# SimTherm — Version Web

Simulateur thermique quasi-statique pour bâtiments en Afrique tropicale.
Version web (Streamlit) du simulateur SimTherm, développée dans le cadre
d'un mémoire de Master 2 Génie Civil BTP (INSTI Natitingou / UNSTIM, Bénin).

**Moteur de calcul strictement identique à la version Excel** — voir
`tests/test_thermal_calc.py` (37 tests, tous passants) pour la preuve
de cohérence croisée.

---

## 🌡️ Ce que fait SimTherm

- Calcule le coefficient de transmission thermique **U** (ISO 6946:2017)
- Calcule le déphasage thermique **φ** et le facteur d'amortissement **f**
  (ISO 13786:2017 + Asan 1998)
- Estime les températures intérieures moyenne, max et min selon la
  méthode d'admittance CIBSE
- Classe le confort thermique (A/B/C/D) selon un modèle adaptatif
  calibré pour l'Afrique tropicale
- Prend en compte les **enduits de finition** (intérieur et extérieur) dans le
  calcul de U par mise en série des résistances (ISO 6946, clause 6.2)
- Intègre l'**absorptivité solaire de la toiture (α)** et le **facteur solaire
  des fenêtres (FS)** dans les apports solaires, avec des repères indicatifs
- Compare automatiquement les matériaux disponibles pour 11 pays
  (Bénin, Burkina Faso, Cameroun, Côte d'Ivoire, Ghana, Kenya, Mali,
  Niger, Nigeria, Sénégal, Togo)

---

## 🚀 Lancer l'application en local

### Prérequis
- Python 3.10 ou supérieur installé sur votre machine
  ([télécharger ici](https://www.python.org/downloads/) si besoin)

### Installation (une seule fois)

Ouvrez un terminal (invite de commandes) dans le dossier du projet, puis :

```bash
pip install -r requirements.txt
```

### Lancement

```bash
streamlit run app.py
```

Une page s'ouvre automatiquement dans votre navigateur à l'adresse
`http://localhost:8501`. Si elle ne s'ouvre pas, copiez cette adresse
dans votre navigateur manuellement.

---

## 🧪 Lancer les tests

Pour vérifier que le moteur de calcul fonctionne correctement :

```bash
pytest tests/ -v
```

Vous devriez voir `37 passed` à la fin — c'est la preuve que le moteur
respecte les normes ISO 6946:2017, ISO 13786:2017 et la formulation
d'Asan (1998).

---

## 📁 Structure du projet

```
simtherm-web/
├── app.py                  # Application Streamlit (point d'entrée)
├── requirements.txt        # Dépendances Python
├── data/
│   └── SimTherm_BD_v6.xlsx # Base de données (climat, matériaux, bâtiments)
├── core/
│   ├── constants.py        # Constantes physiques et seuils normatifs
│   ├── thermal_calc.py     # Moteur de calcul (ISO 6946, ISO 13786, Asan 1998)
│   └── db_loader.py        # Chargement de la base de données Excel
└── tests/
    └── test_thermal_calc.py  # Tests unitaires (validation scientifique)
```

---

## 🌐 Support multilingue

L'interface est rédigée en français uniquement. Les utilisateurs anglophones,
hispanophones, etc. peuvent utiliser la fonction de traduction intégrée à
leur navigateur (Chrome : clic droit → « Traduire en... » ; Firefox : icône
de traduction dans la barre d'adresse).

**Limitations connues** (propres aux outils de traduction navigateur, non
spécifiques à SimTherm) :
- Le texte narratif (titres, résultats, diagnostics, recommandations) se
  traduit normalement.
- Le contenu des menus déroulants (villes, matériaux) peut résister à la
  traduction automatique selon le navigateur — Streamlit utilise des
  composants d'interface personnalisés plutôt que des listes HTML natives.
  Un texte d'écho ("Sélection : ...") est affiché sous chaque menu critique
  pour garantir que le choix reste compréhensible même si le menu lui-même
  n'est pas traduit.
- Le texte à l'intérieur des graphiques (axes, légendes) peut ne pas être
  traduit, ces éléments étant rendus dans un cadre isolé.
- Les résultats numériques (températures, classes A/B/C/D, coefficients)
  sont universels et ne nécessitent aucune traduction.

## 🌍 Déploiement en ligne (Streamlit Community Cloud)

1. Allez sur [share.streamlit.io](https://share.streamlit.io)
2. Connectez-vous avec votre compte GitHub
3. Cliquez sur **"New app"**
4. Sélectionnez votre nouveau dépôt, la branche `main` et le
   fichier `app.py`
5. (Optionnel) Dans **Advanced settings > Secrets**, ajoutez `OPENAI_API_KEY = "sk-..."`
   pour activer les recommandations GPT ; sans clé, les règles hors ligne prennent le relais
6. Cliquez sur **"Deploy"**

L'application sera accessible en ligne en 2-3 minutes, à une adresse
du type `https://votre-nom-simtherm.streamlit.app`.

---

## ⚠️ Note sur la base de données

Une incohérence de saisie a été identifiée et corrigée automatiquement
dans le code (`core/db_loader.py`) : pour certaines lignes de
`BD_CLIMAT`, la colonne "Précipitations annuelles" n'a jamais été
renseignée, ce qui décale la position de la température de neutralité
(Tn) d'une colonne selon les lignes. Le chargeur résout cette
incohérence par une heuristique robuste (Tn doit être une température
plausible entre 15°C et 35°C), validée sur les 156 villes de la base.
Une correction à la source dans le fichier Excel est recommandée pour
une prochaine révision de la base de données.

---

## 📚 Références scientifiques

- ISO 6946:2017 — *Performance thermique des composants et parois de
  bâtiments — Résistance thermique et coefficient de transmission
  thermique*
- ISO 13786:2017 — *Performance thermique des composants de bâtiments
  — Caractéristiques thermiques dynamiques*
- Asan H. (1998), *Effects of wall's insulation thickness and position
  on time lag and decrement factor*, Energy and Buildings 28(3):299-305,
  DOI:10.1016/S0378-7788(98)00030-9
- CIBSE Guide A, chapitre 5 (Admittance Procedure) — Danter (1960),
  Loudon (1968)

---

## 📄 Licence et contexte

Projet développé dans le cadre d'un mémoire de fin de cycle Master 2,
INSTI Natitingou / UNSTIM, Bénin. Soutenance prévue décembre 2026.
