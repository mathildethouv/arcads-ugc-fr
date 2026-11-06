# Arcads UGC en français · skill Claude

Ce skill fait faire à Claude des vidéos UGC « talking head » ultra réalistes, **en français**, avec Arcads (Seedance 2.5) : une personne qui parle face à son téléphone, comme une vraie créatrice.

Tu donnes une vidéo de référence (la façon de parler qui te plaît), une personne (une photo ou une description), ton produit et ton idée. Claude analyse le rythme de la référence, crée l'image de départ, écrit le script en français au bon rythme, puis lance la vidéo dans Arcads. Il s'arrête pour ton accord sur l'image et sur le script.

C'est la méthode Talking Head d'Arcads, adaptée pour que la personne parle un français naturel, avec l'accent de France.

## Ce qu'il te faut
- Claude (l'appli, claude.ai ou Claude Code).
- Le connecteur **Arcads** connecté dans Claude, et un compte Arcads avec des crédits.

## Installer le skill
**Dans l'appli Claude ou sur claude.ai**
1. Télécharge [`arcads-talking-head-fr.zip`](arcads-talking-head-fr.zip).
2. Dans Paramètres › Capacités, active « Créer et modifier des fichiers ».
3. Va dans Personnaliser › Compétences, clique sur **+**, puis **Importer une compétence**, et choisis le `.zip`.
4. Connecte le connecteur Arcads : Paramètres › Connecteurs › Ajouter un connecteur personnalisé, avec l'adresse `https://mcp.arcads.ai`.

**Dans Claude Code** (l'onglet Code de l'appli Claude, avec un abonnement Pro ou plus)

Colle simplement :
> Installe la compétence de ce dépôt GitHub : https://github.com/mathildethouv/arcads-ugc-fr

Ou, dans un terminal :
```bash
git clone https://github.com/mathildethouv/arcads-ugc-fr.git
cp -r arcads-ugc-fr/arcads-talking-head-fr ~/.claude/skills/
```

## L'utiliser
Écris par exemple à Claude :
> Fais une vidéo talking head en français à partir de cette vidéo de référence, pour présenter mon sérum, ton enthousiaste.

---
Méthode d'origine : Arcads (Talking Head). Version française : [@mathilde_ia](https://www.instagram.com/mathilde_ia/).
