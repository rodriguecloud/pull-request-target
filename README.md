# 🎯 CTF Lab : Vulnérabilité `pull_request_target`

> ⚠️ **AVERTISSEMENT DE SÉCURITÉ**
> Ce dépôt est **volontairement vulnérable** à des fins éducatives. Ne reproduisez jamais cette configuration sur un dépôt de production et n'y insérez **aucun secret réel**. Ce lab a été créé pour comprendre et prévenir les failles d'intégration continue (CI/CD).

## 📖 À propos de ce lab

Ce dépôt illustre une faille de sécurité critique courante dans GitHub Actions : la compromission d'un workflow via l'événement `pull_request_target`. 

L'événement `pull_request_target` exécute les workflows dans le contexte du dépôt de base (la cible de la Pull Request), ce qui donne au workflow un accès en lecture/écriture au dépôt et l'accès à ses secrets. La vulnérabilité survient lorsque ce workflow avec privilèges clone et exécute du code non approuvé provenant du fork de l'attaquant.

**Votre mission :** Agir en tant qu'attaquant externe pour forker ce dépôt, modifier le code, et voler le secret `FLAG_SECRET` caché dans l'environnement du dépôt principal.

---

## 🏴‍☠️ Comment jouer (Guide de l'Attaquant)

Pour réussir ce CTF, vous devez utiliser un compte GitHub qui n'a pas les droits d'écriture sur ce dépôt.

### Étape 1 : Forker le dépôt
1. Cliquez sur le bouton **Fork** en haut à droite de cette page pour copier ce dépôt sur votre propre compte GitHub.

### Étape 2 : Préparer la charge utile (Payload)
1. Dans votre fork, ouvrez le fichier `build.sh`.
2. Éditez le fichier pour injecter un script malveillant. Puisque GitHub masque automatiquement les secrets en clair dans les logs, vous devrez ruser (par exemple, en encodant le secret en base64).
   
   *Exemple d'injection malveillante :*
   ```bash
   #!/bin/bash
   echo "Exfiltration du secret en cours..."
   echo $MY_SECRET | base64
