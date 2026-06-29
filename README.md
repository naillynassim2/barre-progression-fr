# Bot Barre de Progression FR

Un bot qui poste sur X (Twitter) le pourcentage de l'année écoulée, en français,
inspiré de @ProgressBar202_. Tourne automatiquement via GitHub Actions — pas
besoin de serveur perso.

## Ce que fait le bot

1. Calcule quel pourcentage de l'année courante est déjà passé.
2. Génère une image avec une barre de progression et le texte
   `"2026 est complétée à 23 %"`.
3. Poste un tweet avec cette image, mais seulement si le pourcentage a
   changé depuis le dernier post (pour éviter le spam).
4. Mémorise le dernier pourcentage posté dans `etat.json`.

## Étape 1 — Créer un compte développeur X

1. Va sur https://developer.x.com et connecte-toi avec ton compte X.
2. Crée un **Projet** puis une **App** à l'intérieur.
3. Dans les paramètres de l'app, mets les permissions sur
   **"Read and Write"** (sinon tu ne pourras pas poster).
4. Génère les 4 clés suivantes (section "Keys and tokens") :
   - `API Key`
   - `API Key Secret`
   - `Access Token`
   - `Access Token Secret`

⚠️ **Note sur les coûts (2026)** : X facture désormais à l'usage
(~0,015 $ par tweet sans lien). Pour ce bot qui poste quelques fois par jour
maximum, ça revient à quelques centimes par mois — mais ce n'est plus
gratuit comme avant. Vérifie les tarifs actuels dans ton compte
développeur avant de lancer le bot en continu.

## Étape 2 — Mettre le projet sur GitHub

1. Crée un nouveau dépôt GitHub (public ou privé, peu importe).
2. Mets-y tous les fichiers de ce projet (`progress_bar_fr.py`,
   `requirements.txt`, `etat.json`, et le dossier `.github/workflows/`).

## Étape 3 — Ajouter tes clés en secrets

Dans ton dépôt GitHub : **Settings → Secrets and variables → Actions →
New repository secret**, et ajoute ces 4 secrets avec les valeurs récupérées
à l'étape 1 :

- `X_API_KEY`
- `X_API_SECRET`
- `X_ACCESS_TOKEN`
- `X_ACCESS_SECRET`

## Étape 4 — C'est lancé !

Le workflow `.github/workflows/post.yml` tourne automatiquement toutes les
3 heures (réglable dans le fichier, ligne `cron:`). Tu peux aussi le
déclencher manuellement depuis l'onglet **Actions** de ton dépôt
(bouton "Run workflow").

## Personnalisation

- **Couleur de la barre** : modifie `COULEUR_BARRE` dans `progress_bar_fr.py`.
- **Texte du tweet** : modifie la ligne `texte = f"🇫🇷 L'année..."` dans la
  fonction `main()`.
- **Fréquence de vérification** : modifie la ligne `cron:` dans
  `post.yml` (la syntaxe cron classique ; ex. `"0 * * * *"` = toutes les heures).
- **Thème différent** (ex. % de la semaine, % du mois) : adapte la fonction
  `calculer_pourcentage_annee` pour changer la période de référence.

## Tester en local avant de déployer

```bash
pip install -r requirements.txt
export X_API_KEY="..."
export X_API_SECRET="..."
export X_ACCESS_TOKEN="..."
export X_ACCESS_SECRET="..."
python progress_bar_fr.py
```
