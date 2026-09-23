# 🏸 BadmintonPro EPS — Documentation complète

> **Plateforme de gestion EPS – Badminton**  
> Lycée Polyvalent Charlotte DELBO · Dammartin-en-Goële · Académie de Créteil

---

## 📋 Présentation

**BadmintonPro EPS** est une application web tout-en-un destinée aux professeurs d'EPS pour gérer une classe de badminton de A à Z : import des élèves, évaluation diagnostique, organisation des poules, suivi des matchs, notation BAC (Général/Techno et Professionnel).

L'application fonctionne **entièrement dans le navigateur**, sans serveur ni installation. Un seul fichier HTML suffit — utilisable sur **ordinateur portable ou tablette** directement depuis le gymnase.

---

## ⚙️ Prérequis & Installation

| Élément | Détail |
|---|---|
| **Format** | Fichier unique `.html` (~100 Ko) |
| **Navigateur** | Chrome, Firefox, Edge, Safari (récents) |
| **Réseau** | Nécessaire au premier chargement (polices Google Fonts + SheetJS) |
| **Installation** | Aucune — ouvrir directement le fichier HTML |
| **Tablette** | Compatible iOS/Android (touch natif) |
| **Sauvegarde** | Automatique dans le navigateur (`localStorage`) |

> ⚠️ Pour que le chiffrement du mot de passe fonctionne (SHA-256), le fichier doit être ouvert en **HTTPS** ou en **localhost**. Sur un réseau local scolaire, utiliser un serveur HTTP simple (ex: `python -m http.server`).

---

## 🔐 Accès & Sécurité

Au démarrage, une **page d'accueil protégée** s'affiche. Elle est requise pour accéder à l'application.

- **Mot de passe** : communiqué séparément par l'administrateur de l'application
- Le mot de passe n'est **jamais stocké en clair** dans le code
- La vérification utilise un **hash SHA-256** via l'API Web Crypto du navigateur (irréversible mathématiquement)
- Appuyez sur **Entrée** ou cliquez **ACCÉDER** pour valider

---

## 🗂️ Onglets — Vue d'ensemble

| # | Onglet | Fonction principale |
|---|---|---|
| 1 | 👥 **Élèves** | Import, ajout manuel, liste, export |
| 2 | 📊 **Diagnostic** | Classement par niveau (N1/N2/N3), drag & drop |
| 3 | ⬆️ **Montante** | Tournoi par positions 0m→200m |
| 4 | 🎯 **Poules** | Constitution, résultats, classement |
| 5 | 📋 **Feuilles Match** | Score interactif multi-poules (tactile) |
| 6 | 🔴 **Zones Dangereuses** | Observation BAC, comptage zones |
| 7 | 🏆 **ATP Tournoi** | Classement ATP, matchs, historique |
| 8 | 🎓 **Barème GT** | Notation BAC Général & Technologique |
| 9 | 🎓 **Barème Pro** | Notation BAC Professionnel + AFLP3 Annexe 1 |

---

## 👥 Onglet Élèves

### Import de fichier
Glissez-déposez ou cliquez pour importer :
- **Excel** (`.xlsx`, `.xls`) — toute feuille avec des noms
- **CSV** (séparateur `;`, `,` ou tabulation)

L'application détecte automatiquement les colonnes `Nom`, `Prénom` et **`Niveau`** (colonne réimportable depuis le diagnostic).

### Ajout manuel
Saisissez `NOM` + `Prénom` puis cliquez **+ Ajouter**. Les élèves sont automatiquement **triés par ordre alphabétique**.

### Suppression
- Par élève : bouton **✕** en fin de ligne
- **🗑️ Tout supprimer** : efface tous les élèves (avec confirmation)

### Export
| Bouton | Contenu | Usage |
|---|---|---|
| **⬇️ Export Excel + Niveaux** | Nom, Prénom, Niveau (`.xlsx`) | Sauvegarde inter-séances — **réimportable** |
| **⬇️ Export CSV simple** | Nom, Prénom, Niveau (`.csv`) | Usage bureautique général |

> 💡 **Workflow séances** : exportez après le diagnostic → réimportez à la séance suivante → les niveaux sont restaurés automatiquement.

---

## 📊 Onglet Diagnostic

Classez vos élèves par niveau de pratique grâce au **glisser-déposer**.

### Niveaux disponibles
| Badge | Description |
|---|---|
| ⏳ Non classés | Élèves non encore évalués |
| 🟡 Niveau 1 | Débutants |
| 🔵 Niveau 2 | Intermédiaires |
| 🟢 Niveau 3 | Avancés |

### Sauvegarde
Cliquez **⬇️ Sauvegarder niveaux (Excel)** pour exporter un fichier réimportable qui restaurera les niveaux à la prochaine séance.

---

## ⬆️ Onglet Montante (0m → 200m)

Système de tournoi **"montante"** par positions sur le terrain.

### Positions
`0m · 25m · 50m · 75m · 100m · 125m · 150m · 175m · 200m`

Tous les élèves démarrent à **0m**.

### Règles
- Un élève ne peut **affronter que quelqu'un dans la même case**
- **Victoire** → monte d'une case vers 200m
- **Défaite** → reste à sa place (ne descend pas)

### Utilisation
1. **Glissez** les élèves entre les cases (ou laissez-les à 0m au départ)
2. Quand une case contient ≥ 2 élèves, le bouton **🏸 Match** apparaît
3. Sélectionnez les deux joueurs, cliquez **J1 gagne** ou **J2 gagne**
4. Le gagnant avance automatiquement

---

## 🎯 Onglet Poules

### Création manuelle
Cliquez **+ Poule** pour créer une poule vide, puis glissez les élèves depuis la zone "Non assignés".

### Génération automatique depuis le Diagnostic ⚡
Bouton **"⚡ Générer depuis Diagnostic"** :
1. Choisissez la **taille des poules** (3, 4 ou 5 joueurs)
2. L'application crée automatiquement les poules par niveau :
   - `N1-1`, `N1-2`... pour le Niveau 1
   - `N2-1`, `N2-2`... pour le Niveau 2
   - `N3-1`, `N3-2`... pour le Niveau 3
   - `NC` pour les non-classés
3. Répartition **équilibrée** : ex. 7 élèves en taille 4 → 1 poule de 4 + 1 poule de 3

### Résultats & Classement
Cliquez **📋 Résultats & Classement** sur une poule (≥ 2 joueurs) :
- Saisie des scores (sets gagnés par joueur)
- Classement automatique par **points** (V=3pts, nul=1pt), puis victoires, puis sets marqués
- Affichage 🥇🥈🥉

---

## 📋 Onglet Feuilles Match

### Multi-poules
Plusieurs feuilles de match simultanées via les **onglets en haut** (Poule 1, Poule 2...).  
Bouton **＋** pour ajouter une nouvelle feuille.

### Configuration
- **Taille de poule** : 3, 4 ou 5 joueurs
- **Saisie des joueurs** : tapez les noms (autocomplétion depuis la liste élèves)
- **Numéro de terrain** : libre

### Démarrer une feuille
Cliquez **▶️ Démarrer** → les matchs sont générés automatiquement :
- Poule de 3 ou 5 → **2 sets × 15 points**
- Poule de 4 → **2 sets × 11 points**
- Chaque paire joue 2 sets, avec arbitre désigné

### Saisie des points (tactile & souris)

| Geste | Action |
|---|---|
| 👆 **Toucher** (ou clic gauche) | ✓ Point normal (vert) |
| 👆 **Maintenir ~0,5s** (ou clic droit) | ✕ Point direct (rouge) |
| Taper sur un point existant | 🗑️ Supprimer le point |

> Une **vibration légère** (30ms) confirme le point direct sur tablette.

### Classement automatique
En bas de la feuille : tableau de classement mis à jour en temps réel (victoires + points marqués).

---

## 🔴 Onglet Zones Dangereuses

Outil d'**observation BAC** pour le critère zones dangereuses/zone centrale.

### Multi-poules
Plusieurs poules d'observation simultanées via les **onglets + bouton ＋**.

### Configuration
- Taille : **3 ou 4 joueurs**
- Saisie des noms de joueurs
- Cliquez **▶️ Démarrer**

### Saisie des zones
Pour chaque set et chaque joueur :

| Colonne | Description |
|---|---|
| 🟡 Zone centrale | Points marqués dans la zone centrale |
| 🔴 Zones dangereuses | Points marqués dans les zones dangereuses |
| **% ZD** | Pourcentage de points en zone dangereuse |

Boutons **＋** et **−** pour incrémenter/décrémenter rapidement.

### Bilan automatique
Tableau récapitulatif par joueur avec totaux et % ZD global.

---

## 🏆 Onglet ATP Tournoi

Reproduction fidèle de la feuille **"résultats"** du fichier Excel ATP.

### Double tableau
Bouton **＋** pour créer un second tableau (ex: **Tableau A** + **Tableau B** pour une classe de 35 élèves).

### Classement dynamique

| Colonne | Signification |
|---|---|
| **Rang** | Position actuelle |
| **Cl.S** | Classement séance précédente |
| **Pts** | Points totaux accumulés |
| **Pts/Séance** | Points de la séance en cours |
| **Matchs** | Nombre de matchs joués |
| **Arb.** | Nombre de matchs arbitrés |
| **GA** | Goal average (pts marqués − pts encaissés) |

### Barème des points

| Situation | Points |
|---|---|
| Gagner vs adversaire 5+ rangs au-dessus | **6 pts** |
| Gagner vs adversaire 2 à 4 rangs au-dessus | **5 pts** |
| Gagner vs adversaire 1 rang au-dessus | **4 pts** |
| Gagner vs adversaire même rang / 1 en dessous | **4 pts** |
| Gagner vs adversaire 2 à 4 rangs en dessous | **3 pts** |
| Gagner vs adversaire 5+ rangs en dessous | **2 pts** |
| **Perdant** (consolation) | **1 pt** |
| **Arbitre** | **1 pt** |

### Saisir un match
1. Entrez **Joueur 1** (avec son score)
2. Entrez **Joueur 2** (avec son score)
3. Entrez l'**Arbitre** (optionnel)
4. Cliquez **✅ Valider** → points calculés et classement mis à jour

### Gestion des séances
- **📅 Nouvelle séance** : archive le rang précédent, remet les pts/séance à 0
- **🔄 Reset scores (garder joueurs)** : remet tous les scores à zéro en conservant la liste des joueurs — idéal en début de séquence
- **🗑️ Vider tout** : supprime joueurs et matchs

### Historique
- 5 derniers matchs affichés en temps réel
- Bouton **📋 Voir tout** → ouvre une page complète dans un nouvel onglet
- **⬇️ Export CSV** → fichier téléchargeable avec date, joueurs, scores, points attribués

---

## 🎓 Onglet Barème GT (BAC Général & Technologique)

Calcul automatique de la note EPS BAC GT selon le référentiel de l'Académie de Créteil.

### Structure de notation

```
AFL1  = 12 pts  (évalué le jour de l'épreuve)
  └─ AFL1-a : Actions techniques     8 pts
  └─ AFL1-b : Choix tactiques        4 pts

AFL2 + AFL3 = 8 pts  (évalués au fil de la séquence)
  └─ Répartition au choix de l'élève : 4-4 / 2-6 / 6-2
```

### Ajouter des élèves
- **Dropdown** → sélectionner un élève de la liste importée
- Bouton **Tous** → ajoute tous les élèves d'un coup

### Saisie AFL1
Pour chaque composante (AFL1-a et AFL1-b) :
1. Sélectionner le **degré** (D1 à D4)
2. Sélectionner le **gain des sets** (−50% / 50% / +50%)
3. Le score est calculé automatiquement

### Saisie AFL2 + AFL3
1. Choisir la **répartition** (2-6 / 4-4 / 6-2)
2. Pour chaque AFL : sélectionner le **degré** (D1 à D4) et le niveau (Min / Moy / Max)

### Résultat
La **note /20** s'affiche instantanément avec un code couleur :
🟢 A (≥16) · 🟡 B (≥12) · 🔵 C (≥8) · 🔴 D (<8)

---

## 🎓 Onglet Barème Pro (BAC Professionnel)

Calcul automatique selon le référentiel BAC Pro de l'Académie de Créteil.

### Structure de notation

```
AFLP1  = 7 pts   (tactique/stratégie)
AFLP2  = 5 pts   (technique)
2×AFLP = 8 pts   (2 AFLP au choix parmi AFLP 3, 4, 5, 6)
  └─ Répartition : 2-6 / 4-4 / 6-2
```

### AFLP1 — Tactique (7 pts)
Quatre degrés nommés d'après les profils de joueurs :

| Degré | Nom | Points |
|---|---|---|
| D1 | Renvoyeur central | 0.5 → 1.5 pts |
| D2 | Repousseur régulier | 2 → 3 pts |
| D3 | Cibleur / Frappeur efficace | 3.5 → 5 pts |
| D4 | Tacticien cibleur & frappeur | 5.5 → 7 pts |

Sélectionner aussi le **gain des sets** (−50% / 50% / +50%).

### AFLP2 — Technique (5 pts)

| Niveau | Points |
|---|---|
| 🥉 Bronze | 1 pt |
| 🥈 Argent | 2.5 pts |
| 🥇 Or | 3.5 pts |
| 💎 Platine | 5 pts |

### 2 AFLP au choix — Annexe 1 (AFLP3)

Chaque AFLP peut être évalué avec le **barème standard** (Degré 1→4) ou en utilisant l'**Annexe 1** pour l'AFLP3.

#### Calcul AFLP3 via Annexe 1

**Étape 1 — Sélectionner la taille de la poule** (3, 4 ou 5 joueurs)

| Taille | Nombre de rencontres évaluées |
|---|---|
| Poule de 3 | 4 rencontres (double tour) |
| Poule de 4 | 3 rencontres |
| Poule de 5 | 4 rencontres |

**Étape 2 — Pour chaque rencontre, cliquer le type d'évolution :**

| Type | Description | Points |
|---|---|---|
| 🟢 | Gagne 2 sets, écart ↑ au 2ème | **4 pts** |
| 🟩 | Gagne 2 sets, écart ≤ au 1er | **3 pts** |
| 🔄 | Perd S1, gagne S2 (remontée) | **2 pts** |
| 📉 | Gagne S1, perd S2 (baisse) | **1.5 pt** |
| 🟡 | Perd 2 sets, réduit écart ≥ 2pts | **1 pt** |
| 🔴 | Perd 2 sets, sans progression | **0.5 pt** |

**Étape 3 — Résultat automatique :**

*Poule de 4 :*
| Gain total | Score AFLP3 | Niveau |
|---|---|---|
| 11–12 pts | 4 | Très Bonne Maîtrise |
| 9–10 pts | 3.5 | Très Bonne Maîtrise |
| 8–8.5 pts | 3 | Maîtrise Satisfaisante |
| 6.5–7.5 pts | 2.5 | Maîtrise Satisfaisante |
| 5–6 pts | 2 | Maîtrise Fragile |
| 3.5–4.5 pts | 1.5 | Maîtrise Fragile |
| 2–3 pts | 1 | Maîtrise Insuffisante |
| 1.5 pt | 0.5 | Maîtrise Insuffisante |
| < 1.5 pt | 0 | Maîtrise Insuffisante |

*Poule de 3 et 5 :*
| Gain total | Score AFLP3 | Niveau |
|---|---|---|
| 15–16 pts | 4 | Très Bonne Maîtrise |
| 13.5–14.5 pts | 3.5 | Très Bonne Maîtrise |
| 10.5–13 pts | 3 | Maîtrise Satisfaisante |
| 8.5–10 pts | 2.5 | Maîtrise Satisfaisante |
| 6.5–8 pts | 2 | Maîtrise Fragile |
| 4.5–6 pts | 1.5 | Maîtrise Fragile |
| 2.5–4 pts | 1 | Maîtrise Insuffisante |
| 2 pts | 0.5 | Maîtrise Insuffisante |
| < 2 pts | 0 | Maîtrise Insuffisante |

> 💡 Cochez "Utiliser AFLP3 (Annexe 1)" sur AFLP A ou AFLP B pour activer ce calcul sur la part correspondante.

---

## 💾 Sauvegarde & Persistence

Les données sont sauvegardées **automatiquement** dans le `localStorage` du navigateur à chaque action.

| Ce qui est sauvegardé |
|---|
| Liste des élèves |
| Niveaux diagnostiques |
| Positions montante |
| Composition des poules + résultats |
| Toutes les feuilles de match + scores |
| Toutes les poules d'observation |
| Tableaux ATP (joueurs, matchs, historique) |
| Notes BAC GT et Pro |

> ⚠️ **Important** : le `localStorage` est lié au navigateur et à l'origine du fichier. Si vous ouvrez le fichier depuis un autre ordinateur ou un autre navigateur, les données ne seront pas présentes. Utilisez l'**Export Excel** pour transférer les données entre sessions.

### Réinitialisation complète
Bouton **🗑️** en haut à droite de l'application (avec confirmation).

---

## 🔄 Workflow recommandé par séance

```
AVANT LA SÉANCE
─────────────────────────────────────────────────
1. Ouvrir BadmintonPro_EPS_v2.html dans Chrome
2. Entrer le mot de passe
3. (1ère fois) Onglet Élèves → Importer le fichier classe
4. (séances suivantes) Importer eleves_niveaux_badminton.xlsx
   → niveaux diagnostiques restaurés automatiquement

PENDANT LA SÉANCE
─────────────────────────────────────────────────
5. Onglet Diagnostic → ajuster les niveaux si besoin
6. Onglet Poules → "⚡ Générer depuis Diagnostic" si nouvelle constitution
7. Onglet Feuilles Match → une feuille par poule active
8. Saisir les scores en temps réel (tablette)
9. Onglet Zones Dangereuses → observation pour BAC si évaluation
10. Onglet ATP Tournoi → enregistrer les matchs
    → "📅 Nouvelle séance" en fin de cours

APRÈS LA SÉANCE
─────────────────────────────────────────────────
11. Onglet Barème GT/Pro → finaliser les notes si jour d'épreuve
12. Onglet Élèves → "⬇️ Export Excel + Niveaux" pour sauvegarder
```

---

## 🛠️ Informations techniques

| Élément | Détail |
|---|---|
| **Technologie** | HTML5 / CSS3 / JavaScript ES2020 (vanilla) |
| **Dépendances** | SheetJS 0.18.5 (lecture/écriture Excel) |
| **Polices** | Barlow Condensed + DM Sans (Google Fonts) |
| **Stockage** | localStorage (navigateur) |
| **Sécurité mot de passe** | SHA-256 via Web Crypto API (SubtleCrypto) |
| **Compatibilité** | Chrome 90+, Firefox 88+, Safari 14+, Edge 90+ |
| **Taille fichier** | ~100 Ko |
| **Réseau requis** | 1ère ouverture uniquement (polices + SheetJS CDN) |

### Modifier le mot de passe
Le mot de passe est stocké sous forme de hash SHA-256 fragmenté dans la fonction `checkPwd()`. Pour le changer, calculez le hash SHA-256 du nouveau mot de passe et remplacez le tableau `_s` :
```bash
echo -n "nouveau_mot_de_passe" | sha256sum
```
Puis découpez le résultat en 8 segments de 8 caractères.

---

## 📌 Référentiels d'évaluation utilisés

- **Référentiel BAC Général et Technologique** — Académie de Créteil — EPS — Champ d'apprentissage n°4 — Badminton
- **Référentiel BAC Professionnel et BMA** — Académie de Créteil — EPS — Badminton  
- Grilles AFLP1, AFLP2, AFLP3 (Annexe 1), AFLP4, AFLP5, AFLP6
- Barème ATP avec système de points basé sur l'écart de classement

---

## 👤 Auteur & Contexte

Outil développé pour les besoins pédagogiques du **Lycée Polyvalent Charlotte DELBO**, Dammartin-en-Goële.

Conçu pour une utilisation **terrain** : rapidité de saisie, ergonomie tablette, fonctionnement hors-connexion.

---

*BadmintonPro EPS v2 — 2025*
