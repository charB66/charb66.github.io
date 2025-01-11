---
layout: post
title:  "Vos notes Obsidian, partout, tout le temps"
description: "Et si vos notes Obsidian étaient synchronisées partout ?"
date:   2025-01-10
categories: ["notes", "productivité"]
tag: ["obsidian"]
---

<p align="center">
  <img src="/images/blog/articles/obsidian-notes-sync-mobile/notes-obsidian.jpeg" alt="Banniere article"/>
</p>

## ⚡️ TL;DR

Dans l'IT et particulièrement en cybersécurité il est important de synchroniser ses notes sur tous nos supports, ordinateur, téléphone portable, etc. Il est tout à fait possible de répondre à ce besoin en utilisant `Obsidian`, son plugin communautaire `Git` et un repository sur `GitHub`.

> Si vous utilisez déjà un repository `Github` pour synchroniser vos notes, vous pouvez suivre les étapes ci-dessous, vous gagnerez du temps. 😎

**Prérequis** :
- Un `vault Obsidian` existant.
- Le plugin communautaire `Git` fonctionnel dans son `vault` (s'il ne vous manque que ce point, sautez au chapitre `2.7 Configuration de Git dans Obsidian !`)

**Étapes** :
1. Créez un `token (classic)` dans les [réglages développeur](https://github.com/settings/tokens) avec le périmètre `repo Full control of private repositories`.
2. Clonez le repository sur votre ordinateur avec le protocole `HTTPS` ; lors de l'authentification, utilisez votre nom d'utilisateur et le `token` en guise de mot de passe.
3. Compressez en .zip le dossier du repository.
4. Créez un `vault` sur l'application mobile `Obsidian` en le nommant (et en respectant la casse) exactement comme le dossier du repository.
5. Téléchargez l'archive .zip sur téléphone portable.
6. Extrayez l'archive .zip et déplacez le dossier extrait dans le dossier de l'application mobile `Obsidian` (en écrasant le dossier déjà présent). Sur iPhone : `Files` > `On My iPhone` > `Obsidian`.
7. Dans l'application mobile `Obsidian`, allez dans `Settings` > `Community plugins` > `Settings` du plugin `Git` > défilez vers le bas jusqu'à la section `Authentication/Commit Author`.
8. Renseignez votre nom d'utilisateur dans `Username` et votre `token` dans `Password/Personal access token`.

## 📜 Table des matières

1. 🤔 POURQUOI ?
2. 🏗️ MISE EN PLACE  
  2.1 Création du repository sur GitHub  
  2.2 Création et configuration du token  
  2.3 Clone du repository  
  2.4 Configuration des informations d'utilisateur  
  2.5 Configuration du .gitignore  
  2.6 Premier commit & push  
  2.7 Configuration de Git dans Obsidian  
  2.8 Test de bon fonctionnent  
  2.9 Compression et téléchargement du vault  
  2.10 Création du vault  
  2.11 Configuration du vault 
  2.12 Test de bon fonctionnement multiplateforme    
3. 🏆 BÉNÉFICES
4. 🔄 RETOUR D'EXPÉRIENCE
5. [🗝️ EN RÉSUMÉ](#en-resume)
## 1. 🤔 POURQUOI ?

> Il m'est arrivé maintes fois de me perdre dans mes notes stockées sur une multitude de supports. Le plus fréquent est lorsque je suis en pentest. Le dernier en date, je suis tombé sur une techno rarement utilisée, je me souvenais avoir mis de côté un article ou un outil à son sujet... mais... où ? 🫠. À ce moment précis je me suis dit qu'il fallait que je stocke et gère mes notes dans un seul outil !

Dans les métiers de l'IT et d'autant plus en cybersécurité, la masse d'informations à lire, analyser et stocker est énorme ; bien trop dense pour ne pas avoir une judicieuse gestion de ses notes. Et cela commence par l'outil et le stockage de celles-ci.

Outre le fait d'utiliser - le bon outil de prise de note - il est primordial de se simplifier la tâche. Selon-moi il y a 2 critères vitaux :  
- La prise de note doit être simple.
- La prise de note doit être rapide.

Un point est commun aux critères ci-dessus : **le stockage**, et qui dit stockage, dit **synchronisation**.

Je détaille dans cet article comment **synchroniser** sur son ordinateur et son téléphone portable ses notes prises avec `Obsidian`.
## 2 🏗️ MISE EN PLACE

> Je veillerai à être aussi complet que possible dans mes explications, tout en évitant les informations superflues.

### 2.1 Création du repository sur GitHub

Commençons par la création du repository sur `GitHub`. Le premier point sur lequel il est intéressant de vous pencher est sa visibilité : privé ou publique ?  
Supposons que ce repository soit pour une prise de note personnelle, il se peut qu'il contienne des données à caractère personnel (`DCP`) ; respectons donc le `Zero Trust` en créant un repository privé.

![](/images/blog/articles/obsidian-notes-sync-mobile/obsidian-sync-1.png)  

### 2.2 Création et configuration du token

L'application `Obsidian` mobile ne gère pas le protocole SSH, la solution de contournement la plus propre est d'utiliser le protocole `HTTPS` et un `token GitHub`.  
La gestion des `tokens` se fait dans la section `Developer settings` des comptes `GitHub`. Parce que je suis sympa, voici [le lien](https://github.com/settings/tokens) d'accès (je vous en prie les feignants 🙃).  
Ensuite, cliquez sur `Generate new token` > `Generate new token (classic)` puis renseignez :
- Le nom du token dans le champ `Note`.
- Une date d'expiration.
	- Personnellement j'ai choisi `No expiration` car ce token ne servira que pour Obsidian et ne sera jamais partagé. Pour plus de sécurité il serait judicieux de définir une expiration au bout d'un an. En revanche cela implique d'en générer un nouveau puis de le mettre à jour dans l'application mobile `Obsidian` (oui, cette fois-ci c'est moi le feignant 😪).
- Le périmètre : quel sera le rôle de votre `token` ? Gérer le repository des notes via l'application `Obsidian`.
	- `repo Full control of private repositories` est parfait pour cela.

![](/images/blog/articles/obsidian-notes-sync-mobile/obsidian-sync-2.png)  

Enfin, défilez en bas de page puis cliquez sur `Generate token`.

>⚠️ Le token ne sera plus visible une fois la page courante fermée, il est impératif de le sauvegarder et de préférence dans un gestionnaire de mot de passe (KeePass, Bitwarden, Vaultwarden, Proton Pass, Enpass...).

### 2.3 Clone du repository

Dorénavant, tout est prêt ! 👏  
C'est le moment de cloner le repository sur votre ordinateur. Rendez-vous sur la page du repository tout juste créé, sélectionnez le protocole `HTTPS` puis copiez l'url.
![](/images/blog/articles/obsidian-notes-sync-mobile/obsidian-sync-3.png)  
Clonez le repository en ligne de commande :
```bash
git clone https://github.com/charB66/notes_blog_demo.git
```

Une authentification est nécessaire, il vous sera demandé de renseigner votre nom d'utilisateur `GitHub` et votre mot de passe. Le mot de passe à utiliser est votre `token GitHub` que vous venez de générer. 

Un message avertissant que votre repository est vide devrait apparaître, c'est normal, vous pouvez l'ignorer.  
![](/images/blog/articles/obsidian-notes-sync-mobile/obsidian-sync-4.png)  

Avant de continuer, il faut s'assurer que le repository a bien été cloné avec le protocole `HTTPS`, pour cela il faut regarder  l'url distante configurée dans le `.git`.  

Depuis le dossier du repository :
```bash
grep url .git/config
```
Cette commande cherche les occurrences du mot `url` dans le fichier `.git/config`, qui est le fichier de configuration du repository actuel.  
Celle-ci devrait retourner :  
```bash
url = https://github.com/charB66/notes_blog_demo.git
```
Le scheme `https` en début d'url doit être présent comme ci-dessus.  

Il est aussi possible de vérifier la bonne configuration avec la commande `git remote` :
```bash
git remote -v  
origin  https://github.com/charB66/notes_blog_demo.git (fetch)  
origin  https://github.com/charB66/notes_blog_demo.git (push)
```

### 2.4 Configuration des informations d'utilisateur

Pour fonctionner correctement quelques informations de l'utilisateur `Git` doivent être configurées.  

Configuration de votre nom d'utilisateur :
```bash
git config user.name "<pseudo>"
```

Configuration de votre email :
```bash
git config user.email "<email>"
```
### 2.5 Configuration du .gitignore

Afin d'éviter de passer votre temps à gérer les conflits `git` (et je sais de quoi je parle), il est fortement conseillé de configurer le `.gitignore`. Ce fichier permet d'ajouter des exceptions qui ne seront pas synchronisées dans le repository.  
Dans le contexte de repository utilisé avec `Obsidian`, il s'agit du `workspace` de ce dernier qui pose "problème". Les fichiers `.obsidian/workspace.json` et `.obsidian/workspace-mobile.json` sauvegardent l'espace de travail afin de retrouver - entre autre - les mêmes onglets ouverts à chaque lancement.  
Bref, ce n'est pas indispensable et ces fichiers génèrent beaucoup de conflits `Git`.  

En étant dans le dossier du repository, exécutez simplement cette commande :
```bash
echo ".obsidian/workspace.json\nworkspace-mobile.json" > .gitignore
```

Et voici de nombreux cheveux blancs évités (trop tard me concernant). 🙃

### 2.6 Premier commit & push

Avant d'activer et utiliser le plugin `Obsidian`, il est préférable de faire les premiers essais en ligne de commande.  

Vous devez réaliser le premier commit et le premier push sur le repository distant (`GitHub`).  

Depuis le dossier du repository :
```bash
git add .
git commit -m "Premier commit !"
git push
```
### 2.7 Configuration de Git dans Obsidian

Dans `Obsidian`, ouvrez un dossier en tant que `vault`.  
![](/images/blog/articles/obsidian-notes-sync-mobile/obsidian-sync-5.png)  

Sélectionnez le dossier du repository puis créez une note en guise de test.  
![](/images/blog/articles/obsidian-notes-sync-mobile/obsidian-sync-6.png)  

Le repository est configuré et utilisé, mais pour le moment `Obsidian` ne gère pas lui même les commits, push, pull... il convient donc d'ajouter le plugin communautaire `Git` dans `Obsidian`.  

Rendez-vous dans les réglages et activez les `Community plugins`.  
![](/images/blog/articles/obsidian-notes-sync-mobile/obsidian-sync-7.png)  
![](/images/blog/articles/obsidian-notes-sync-mobile/obsidian-sync-8.png)  

Cliquez sur `Browse` puis recherchez le plugin `Git`.  
![](/images/blog/articles/obsidian-notes-sync-mobile/obsidian-sync-9.png)  

Sélectionnez le plugin et cliquez sur `Install`, attendez l'installation puis cliquez sur `Enable`. Une fois l'activation terminée, cliquez sur `Options`. Je conseille d'activer la synchronisation automatique, cela simplifie l'usage, en particulier dans l'application mobile.  
![](/images/blog/articles/obsidian-notes-sync-mobile/obsidian-sync-10.png)  
  
### 2.8 Test de bon fonctionnent

Pour s'assurer du bon fonctionnement il est judicieux de modifier une note puis de pousser la mise à jour sur `GitHub`.  

Une petite astuce pratique pour suivre le statut du repository est d’utiliser le panneau latéral `Source control` (sur la droite ci-dessous).  
![](/images/blog/articles/obsidian-notes-sync-mobile/obsidian-sync-11.png)  

Pour l'afficher, faites le raccourci clavier `CTRL` + `P` puis saisir `control view` :  
![](/images/blog/articles/obsidian-notes-sync-mobile/obsidian-sync-12.png)  

Ce panneau permet de suivre les fichiers staged, pushed et pulled. Mais surtout via les commande situées dans la partie supérieure, de manuellement push, pull etc.  

Enfin, modifiez une note puis poussez manuellement la modification.  
![](/images/blog/articles/obsidian-notes-sync-mobile/obsidian-sync-13.png)  

Pensez à également vérifier sur `GitHub` que la note a bien été mise à jour.  
![](/images/blog/articles/obsidian-notes-sync-mobile/obsidian-sync-14.png)  

### 2.9 Compression et téléchargement du vault

Enfin la concrétisation ; place à vos notes sur votre téléphone portable ! 📲   

Depuis le dossier de votre `vault Obsidian` :
```bash
zip -r notes.zip <votre_dossier>
```
Cette commande crée une archive .zip, téléchargez là sur votre téléphone portable. Vous pouvez vous l'envoyer par courriel, ou même vous faire un python web server (`python3 -m http.server`). 🥷  

> Certaines des instructions suivantes sont effectuées depuis un iPhone ; je vous laisse le soin de les adapter pour Android. 😉

### 2.10 Création du vault

En premier lieu, il convient de créer le `vault Obsidian` dans l'application mobile.  
Celui-ci doit avoir exactement le même nom que le dossier du repository de vos notes sur votre ordinateur. Si votre dossier se nomme `notes_perso`, votre `vault Obsidian` doit se nommer `notes_perso`.

![](/images/blog/articles/obsidian-notes-sync-mobile/obsidian-sync-15.png)  
Appuyez sur `Create new vault` puis, dans l'écran suivant saisissez le nom et laissez décochée la case `Store in iCloud`.  

Décompressez l'archive .zip.  
![](/images/blog/articles/obsidian-notes-sync-mobile/obsidian-sync-16.png)  

Déplacer le dossier du repository dans le dossier de l'application mobile `Obsidian` (`Files` > `On My iPhone` > `Obsidian`). Si le nom est correct, un message vous avertissant qu'un dossier du même nom existe déjà ; choisissez "Replace".   
![](/images/blog/articles/obsidian-notes-sync-mobile/obsidian-sync-17.png)  

### 2.11 Configuration du vault 

Rendez-vous dans les réglages : `Settings` > `Community plugins`.  
![](/images/blog/articles/obsidian-notes-sync-mobile/obsidian-sync-18.png)  

Défilez en bas de l'écran et appuyez sur `Turn on community plugins`.  
Appuyez sur `Browse` puis recherchez `Git`. Sélectionnez le premier de la liste, appuyez sur `Install` > `Enable` puis `Options`.
![](/images/blog/articles/obsidian-notes-sync-mobile/obsidian-sync-19.png)  

Un nouvel écran apparaît vous demandant votre `username`, renseignez votre nom d'utilisateur `GitHub`. Une fois validé un autre écran apparaît vous demandant votre `Password/Personal access token`, renseignez votre `token (classic)` créé précédemment.  
![](/images/blog/articles/obsidian-notes-sync-mobile/obsidian-sync-20.png)  

### 2.12 Test de bon fonctionnement multiplateforme

Il est temps de tester le bon fonctionnement de la synchronisation entre l'application mobile et celle de votre ordinateur.  

Modifier une note dans l'application mobile puis pousser les changements sur votre repository `GitHub`.  
Pour cela je vous conseille de passer par le panneau `source control view`. Afin d'afficher la `Command palette` sur l'application vous devez swiper de haut en bas (pour rappel `CTRL` + `P` sur le client lourd de votre ordinateur).  
![](/images/blog/articles/obsidian-notes-sync-mobile/obsidian-sync-21.png)  
![](/images/blog/articles/obsidian-notes-sync-mobile/obsidian-sync-22.png)  

1. `Backup` : permet de sauvegarder et pousser vos modifications sur votre repository distant (`git add` ; `git commit` ; `git push`).
2. `git push` : permet de pousser vos modifications sur votre repository distant.
3. `git pull` : permet de mettre à jour votre repository local en récupérant les derniers changements depuis votre repository distant.


Et voici, vous avez vos notes automatiquement synchronisées sur vos différents supports. 👏

## 3. 🏆 BÉNÉFICES

### Gain de temps

La synchronisation automatique des notes évite la multiplication des applications et/ou supports de notes. En n'ayant pas à rechercher **sur quel support** vous avez stocké votre note (sur mon téléphone ou mon ordinateur ?) ni **dans quelle application** (Obsidian ? Notion ? Mon application notes ? Un papier ?), l'accès à l'information est fluide et rapide. Cette fluidité est importante car il est vital de pouvoir se concentrer sur le contenu de la note et non la "logistique" autour. Il n'est déjà évident de prendre une note, il faut analyser l'information, la synthétiser... alors si à cela, vous ajoutez du "bruit" à votre pensée, vous vous éloignez de l'essence même du principe de prise de note.

### Gain de productivité

Pour les mêmes raisons listées dans le gain de temps, la productivité de vos prise de notes se voit grandement amplifiée. Celle-ci sont plus rapides à rédiger, à retrouver, à modifier. Puis, une nouvelle fois, ne plus réfléchir à la gestion des notes favorise la clarté de penser et donc la pertinence du contenu.

### Sécurité et fiabilité

La synchronisation via un repository `GitHub` est un excellent choix pour plusieurs raisons. Vous retrouvez les mêmes notes, à jour, partout. Vos notes sont versionnées avec de nombreux commits, ce qui vous permets à tout moment de revenir en arrière en cas de mauvaise modification, suppression par erreur...

### Disponibilité constante

L'avantage d'utiliser `GitHub` pour la synchronisation est que **vos notes sont disponibles partout, tout le temps**. Vous y avez accès sur tous vos supports, fixes, mobiles... et si vous n'avez pas accès à ceux-ci - par exemple en déplacement - vous pouvez les retrouver depuis n'importe quel navigateur web, directement sur le site de `GitHub` !

## 4. 🔄 RETOUR D'EXPÉRIENCE

J'ai trouvé peu de ressources concernant les possibilités de synchronisation des notes sans passer par l'option payante d'`Obsidian`. Avec un peu de recherche, j'ai trouvé quelques informations sur des forums, des threads `Reddit`, le plus pertinent est ce [post](https://forum.obsidian.md/t/obsidian-git-sync-on-your-ios-without-any-extra-app/60639) sur le forum officiel `Obsidian`, mais cela reste succinct. Voici l'origine de ma volonté de vulgariser la procédure.  

Après 1 mois d'utilisation je suis conquis. Je prends plus de notes qu'auparavant, je m'y retrouve beaucoup plus facilement. Toutefois, je vous conseille d'augmenter à 5 ou 10 mins l'interval de synchronisation automatique, sans quoi, vous aurez trop souvent les notifications du plugin `Git` lors de la lecture de vos notes, et cela n'est vraiment pas pratique. Pour ma part, j'ai réglé à 10 mins, puis, je push et pull manuellement via le panneau `source control view`.  

Je tiens à préciser que la facilité de prise de note peut mener à une surcharge informationnelle. Vous pouvez accumuler trop de données sans les structurer ni les prioriser. Une accumulation excessive des notes peut engendrer un encombrement cognitif, réduisant votre capacité à hiérarchiser et exploiter efficacement les données. Cela tombe bien, un article sur la méthodologie de prise de notes est en cours de rédaction. 😉

## 5. 🗝️ EN RÉSUMÉ {#en-resume}

Le procédé n'est pas facile au premier abord et il convient d'être à l'aise avec la manipulation de votre téléphone portable et de `Git`. Une fois ce "gap" technique passé, les avantages sont notables et valent le coup d'investir des efforts à l'acquisition des connaissances techniques.  

N'hésitez pas à me faire vos retours (en commentaire ou via mes contacts sur ma page [about](#about)) sur la mise en place et l'utilisation de cette méthode !