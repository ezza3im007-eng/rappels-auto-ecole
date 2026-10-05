# Rappels automatiques — Auto-École HN

Envoie **tout seul** un e-mail et un message WhatsApp à chaque élève :

| Séance | Quand |
|---|---|
| Conduite / Code | la veille à 18h, puis 2 h avant |
| Examen | 3 jours avant (18h), la veille (18h), 2 h avant (ou 7h le jour J si pas d'heure) |

Les données viennent de l'appli : planning (type Conduite / Code / Examen), **e-mail** et **téléphone** de chaque élève.
Chaque rappel n'est envoyé qu'une seule fois (journal dans la collection `rappels_envoyes`).

## Installation (une seule fois, ~30 min)

### 1. GitHub (gratuit) — c'est lui qui lance le programme toutes les 30 min
1. Créez un compte sur github.com, puis un dépôt **privé** nommé `rappels-auto-ecole`.
2. Envoyez-y tout le contenu de ce dossier (y compris le dossier caché `.github`).

### 2. Accès à votre base Firebase
Console Firebase › ⚙ Paramètres du projet › **Comptes de service** › « Générer une nouvelle clé privée ».
Ouvrez le fichier .json téléchargé : copiez **tout son contenu**.
GitHub › votre dépôt › Settings › Secrets and variables › Actions › New secret :
`FIREBASE_SERVICE_ACCOUNT` = le contenu du fichier. Ne partagez jamais ce fichier.

### 3. E-mail (Gmail, gratuit)
Compte Google › Sécurité › activez la validation en 2 étapes › **Mots de passe d'application** › créez-en un.
Secrets GitHub : `SMTP_USER` = votre adresse Gmail, `SMTP_PASS` = le mot de passe d'application (16 lettres).

### 4. WhatsApp (API officielle de Meta)
1. developers.facebook.com › créer une application de type **Entreprise** › ajouter le produit **WhatsApp**.
2. Ajoutez/validez le numéro d'envoi. Notez le **Phone number ID**.
3. Créez un **jeton permanent** (Paramètres de l'entreprise › Utilisateurs système › Générer un jeton, permission `whatsapp_business_messaging`).
4. WhatsApp Manager › Modèles de messages › créez le modèle :
   - Nom : `rappel_seance` — Langue : **Français** — Catégorie : **Utilitaire**
   - Corps : `Bonjour {{1}}, rappel Auto-École HN : {{2}} {{3}}. {{4}}`
   - Exemples : Sami / votre leçon de conduite / demain à 10:00 / Merci d'être à l'heure.
   Attendez l'approbation (de quelques minutes à quelques heures).
5. Secrets GitHub : `WA_TOKEN` = le jeton, `WA_PHONE_ID` = le Phone number ID.

### 5. Essai
GitHub › onglet **Actions** › « Rappels automatiques » › **Run workflow** (case « Mode test » cochée) :
le journal affiche ce qui *serait* envoyé, sans rien envoyer. Si c'est bon, le programme tourne ensuite seul.
Pour un essai réel, décochez « Mode test ».

## À savoir
- **Coût** : GitHub, Gmail et Firebase (lecture) restent gratuits à cette échelle. WhatsApp facture chaque message « modèle » : consultez la grille tarifaire de Meta pour votre pays.
- **Consentement** : Meta exige que l'élève ait accepté de recevoir des messages WhatsApp (à faire signer à l'inscription).
- **Retards** : GitHub peut décaler un déclenchement de quelques minutes. Les rappels en retard de moins de 3 h partent quand même ; au-delà, ils sont ignorés.
- **Un élève sans e-mail ni téléphone** reçoit seulement les notifications de l'appli. Numéros tunisiens à 8 chiffres acceptés (préfixe +216 ajouté).
- Un dépôt public sans activité pendant 60 jours voit ses tâches planifiées désactivées : relancez-les dans l'onglet Actions.
- Test local : `npm install && npm test`.
