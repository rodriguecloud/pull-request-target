# 🎯 CTF : Vulnérabilité `pull_request_target` GitHub Actions

**Difficulté :** ⭐☆☆☆☆ (1/5)  
**Catégorie :** CI/CD Security / Sécurité Github Actions  

> ⚠️ **AVERTISSEMENT DE SÉCURITÉ**
> Ce dépôt est **volontairement vulnérable** à des fins éducatives. Ne reproduisez jamais cette configuration sur un dépôt de production et n'y insérez **aucun secret réel**. Ce lab a été créé pour s'entraîner, comprendre et prévenir les failles d'intégration continue.

## 📖 À propos de ce lab

Ce laboratoire pratique met en lumière une vulnérabilité critique et très courante dans les pipelines GitHub Actions : la mauvaise utilisation du déclencheur `pull_request_target`.

L'événement `pull_request_target` s'exécute dans le contexte et avec les privilèges du dépôt de base (la victime), lui donnant un accès complet aux secrets de l'environnement. La faille de sécurité survient lorsque le workflow utilise l'action `checkout` pour récupérer le code non vérifié du fork de l'attaquant, puis exécute un script issu de ce code corrompu.

---

## 🏆 Objectif et Condition de victoire

Un secret nommé `FLAG_SECRET` a été configuré dans les paramètres Actions de ce dépôt. 

**✅ Le lab est réussi si :** Vous parvenez à extraire et lire la valeur exacte de ce secret (qui est au format `CTF{...}`). 

Vous êtes libre de choisir la méthode d'exfiltration (via les logs de la console GitHub ou via une requête réseau sortante vers un webhook externe).

---

## 🏴‍☠️ Comment jouer (Instructions)

Pour réaliser ce CTF dans des conditions réelles, vous devez agir en tant qu'attaquant externe. Utilisez un compte GitHub qui ne possède **aucun droit d'écriture** sur ce dépôt.

### Étape 1 : Forker et analyser
1. Cliquez sur le bouton **Fork** en haut à droite pour copier ce dépôt sur votre compte.
2. Analysez le fichier `.github/workflows/ci.yml` pour comprendre comment le secret est injecté dans l'environnement d'exécution.

### Étape 2 : Préparer la charge utile (Payload)
1. Dans votre fork, modifiez le fichier `build.sh`.
2. Injectez-y vos commandes malveillantes pour lire la variable d'environnement contenant le secret. *(Astuce : GitHub masque les secrets en clair dans les logs, il faudra ruser, par exemple avec un encodage base64 ou un envoi via `curl`).*
3. Committez vos changements sur votre branche.

### Étape 3 : Déclencher l'attaque
1. Ouvrez une **Pull Request** depuis votre fork en direction de ce dépôt principal.
2. L'ouverture de la PR déclenchera automatiquement le workflow vulnérable sur le serveur cible.

### Étape 4 : Récupérer le Flag
1. Rendez-vous dans l'onglet **Actions** de ce dépôt principal.
2. Inspectez les logs de l'exécution de votre Pull Request, ou vérifiez la réception de vos requêtes HTTP externes si vous avez utilisé la méthode réseau.
3. Décodez le secret. Si vous lisez le flag complet, c'est gagné ! 🎉

Bon hacking éthique ! 👾
