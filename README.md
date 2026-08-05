# Ma Maison Adaptée — site vitrine

Site d'une page pour **Dom&Vie Lyon Sud-Ouest**, avec back-office pour modifier
les textes et les photos sans toucher au code.

## Comment ça marche

- Le **contenu** vit dans un seul fichier : `src/_data/site.json`
- La **mise en page** vit dans `src/index.njk`
- Le back-office (Decap CMS) écrit dans `site.json`
- À chaque modification, Netlify reconstruit le site automatiquement

Vous ne touchez jamais au code : vous modifiez le contenu depuis `/admin`.

---

## Installation, une seule fois

### 1. Créer le dépôt

> Avec l'authentification GitHub, seules les personnes ayant un accès en
> écriture au dépôt peuvent modifier le site. Pour vous seul, c'est parfait.

1. Créez un compte sur [github.com](https://github.com) si nécessaire.
2. Créez un dépôt nommé `mamaisonadaptee` (privé ou public, au choix).
3. Envoyez-y le contenu de ce dossier.

### 2. Déployer sur Netlify

1. Créez un compte sur [netlify.com](https://netlify.com).
2. **Add new site → Import an existing project**, choisissez votre dépôt GitHub.
3. Netlify lit `netlify.toml` et configure tout seul la commande de build.
4. Le site est en ligne sur une adresse provisoire `xxx.netlify.app`.

### 3. Activer le back-office (authentification GitHub)

Vous vous connecterez au back-office avec votre compte GitHub.
Netlify sert d'intermédiaire OAuth (c'est un service distinct de Netlify
Identity, qui est déprécié).

**a) Créer une application OAuth chez GitHub**

1. GitHub → *Settings* → *Developer settings* → *OAuth Apps* → *New OAuth App*
2. Remplissez :
   - Application name : `Ma Maison Adaptee CMS`
   - Homepage URL : l'adresse de votre site
   - **Authorization callback URL** : `https://api.netlify.com/auth/done`
3. Validez, puis notez le **Client ID** et générez un **Client Secret**.

**b) Déclarer l'application chez Netlify**

1. Dans votre site Netlify : *Site configuration* → *Access control* → *OAuth*
2. *Install provider* → GitHub → collez le Client ID et le Client Secret.

**c) Renseigner le dépôt**

Dans `src/admin/config.yml`, remplacez `VOTRE-COMPTE/VOTRE-DEPOT` par le chemin
de votre dépôt (exemple : `gmaindret/mamaisonadaptee`).
Enregistrez, envoyez sur GitHub.

Rendez-vous ensuite sur `/admin` : un bouton **Login with GitHub** apparaît.

> **Si le fournisseur OAuth n'apparaît pas chez Netlify**, l'interface a pu
> changer. Utilisez alors [decapbridge.com](https://decapbridge.com) (gratuit) :
> la configuration de repli est déjà écrite, en commentaire, dans `config.yml`.
> Autre option : héberger vous-même un petit gestionnaire OAuth
> (Cloudflare Worker), documenté sur decapcms.org.

### 4. Brancher le nom de domaine

Dans Netlify : **Domain management → Add a domain** → `mamaisonadaptee.fr`.
Suivez les instructions DNS chez votre registrar. Le HTTPS s'active seul.

> Rappel contrat (art. 7.4.2) : le nom de domaine doit être **validé par le
> franchiseur** avant réservation.

---

## Utilisation au quotidien

### Modifier le contenu

Allez sur `https://mamaisonadaptee.fr/admin`, connectez-vous.
Vous pouvez modifier : téléphone, e-mail, textes, photos, réalisations,
avis clients, communes desservies.

Cliquez **Publish**. Le site est à jour en une à deux minutes.

### Les photos

Elles sont envoyées dans `src/images` depuis le back-office.
**Préparez-les avant** : largeur d'environ 1200 px, format JPEG, moins de 300 Ko.
Une photo de 5 Mo sortie de l'appareil ralentira le site.

### Les demandes de devis

Le formulaire est géré par Netlify (gratuit jusqu'à 100 envois par mois).
Les demandes arrivent dans **Netlify → Forms**. Configurez une notification
par e-mail pour être prévenu.

---

## Développer en local (facultatif)

```bash
npm install
npm start
```

Le site tourne sur `http://localhost:8080`.

---

## À ne pas modifier

- Le bloc `<style>` dans `index.njk` : couleurs et polices imposées par la
  **charte graphique Dom&Vie** (rose `#E6007E`, violet `#622977`, Montserrat).
- Le bloc `sprite` : logo officiel et icônes. Le logo ne doit jamais être
  redessiné, ni le texte séparé de la pastille.

## À compléter avant la mise en ligne

- [ ] Numéro de téléphone et e-mail réels
- [ ] SIREN
- [ ] Assureur et médiateur dans `mentions-legales.njk`
- [ ] Photos avant/après et réalisations
- [ ] Vrais avis Google (ne pas reprendre ceux d'autres agences)
- [ ] Validation du domaine par le franchiseur
