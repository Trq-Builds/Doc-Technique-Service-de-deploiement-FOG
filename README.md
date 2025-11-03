# ` 👻 `︲Documentation TP : Installer et configurer un service de déploiement FOG

---

Ce dépôt présente un guide complet pour l’installation, la configuration et l’utilisation du service de déploiement **FOG** sous **Debian 13**.
Tu y apprendras à **installer le serveur**, **capturer une image master**, et **déployer celle-ci** sur plusieurs machines clientes.

---

> [!IMPORTANT]
> Les vidéos et captures d’écran seront ajoutées progressivement.
> Chaque section sera accompagnée de commandes, de précisions techniques et de recommandations
> Les vidéos sont hébergées sur `dona.one`.
> Autrement utilisez `Mega.nz` pour visionner les vidéos.

---

## `📑`︲Sommaire (cliquez pour accéder directement à la section souhaitée)

1. [`📘`︲Introduction.](#introduction)

   * [`❔`︲Contexte et objectifs du TP.](#contexte-et-objectifs)
   * [`🧰`︲Présentation des outils et prérequis.](#presentation-outils-prerequis)

2. [`🛠️`︲Installation de Debian 13 sans interface graphique.](#installation-debian)

   * [`💿`︲Création et configuration de la VM.](#creation-vm)
   * [`⚙️`︲Installation du système Debian 13.](#installation-systeme)
   * [`🔐`︲Création des comptes et configuration SSH.](#configuration-ssh)

3. [`🌍`︲Installation du service FOG.](#installation-fog)

   * [`⬇️`︲Téléchargement et préparation de l’archive.](#telechargement-fog)
   * [`🧩`︲Procédure d’installation du serveur FOG.](#installation-serveur-fog)
   * [`🔧`︲Configuration initiale et tests.](#configuration-initiale-fog)

4. [`⚙️`︲Configuration du service FOG.](#configuration-service-fog)

   * [`🌐`︲Paramètres réseau et DHCP.](#parametres-reseau)
   * [`⏱️`︲Modification du menu PXE et du timeout.](#modification-pxe)
   * [`👥`︲Augmentation du nombre de clients simultanés.](#clients-simultanes)

5. [`🖼️`︲Création d’une image et d’un groupe de déploiement.](#creation-image-groupe)

   * [`📷`︲Création d’une image “S126-master”.](#creation-image)
   * [`👥`︲Création d’un groupe de déploiement et assignation.](#creation-groupe)

6. [`💻`︲Préparation de la machine master.](#preparation-master)

   * [`🪛`︲Installation du logiciel personnalisé.](#logiciel-master)
   * [`📋`︲Recensement de la machine sur le serveur FOG.](#recensement-machine)
   * [`🏷️`︲Renommage et validation.](#renommage-machine)

7. [`📦`︲Capture de l’image master.](#capture-image)

   * [`🧭`︲Préparation avant capture.](#preparation-capture)
   * [`📸`︲Lancer la capture et suivi du processus.](#lancer-capture)

8. [`🚀`︲Déploiement de l’image par multicast.](#deploiement-image)

   * [`🌐`︲Préparation de l’environnement réseau.](#preparation-deploiement)
   * [`⚡`︲Lancement du déploiement multicast.](#lancement-deploiement)
   * [`✅`︲Vérifications post-déploiement.](#verification-deploiement)

9. [`🧩`︲Fonctionnalités avancées (optionnel).](#fonctionnalites-avancees)

   * [`🔁`︲Mise à jour du serveur FOG.](#maj-fog)
   * [`🔒`︲Intégration au domaine Active Directory.](#integration-ad)
   * [`📡`︲Utilisation d’un second réseau dédié au déploiement.](#reseau-dedie)

10. [`📚`︲Conclusion et bonnes pratiques.](#conclusion)

11. [`🧰`︲Outils et ressources utilisées.](#outils-ressources)

---

<a id="introduction"></a>
## `📘`︲Introduction.

---

<a id="contexte-et-objectifs"></a>
## `❔`︲Contexte et objectifs du TP.
> [!NOTE]
> Tu vas apprendre à installer et configurer un serveur FOG, à capturer et déployer des images système sur un parc de machines, à gérer les configurations réseau (DHCP, multicast), et à automatiser le déploiement d’environnements Windows ou Linux dans un contexte professionnel.
> L’objectif est de te permettre de maîtriser les bases de la gestion de parc informatique et de la clonage d’images via FOG, un outil libre et puissant, afin de déployer rapidement et efficacement des postes de travail dans un environnement de type SISR.

---

<a id="presentation-outils-prerequis"></a>
## `🧰`︲Présentation des outils et prérequis.
> [!IMPORTANT]
> Présentation des outils et prérequis :
> - **Serveur :** Debian 13 **sans interface graphique** [`🌐`](https://www.debian.org/)
> - **Client :** Windows 11. [`🌐`](https://www.microsoft.com/fr-fr/software-download/windows11)
> - **Outils :** FOG (Free Open-Source Ghost) `V.1.5.10.1721` [`🌐`](https://fogproject.org/)
---

<a id="installation-debian"></a>
## `🛠️`︲Installation de Debian 13 sans interface graphique.

---

<a id="creation-vm"></a>
## `💿`︲Création et configuration de la VM.

### `⚙️`︲Configuration de la VM.

* **Hostname :** `srv-fog`.
* **Adressage IP :** dynamique (DHCP) récupérer une adresse sur le réseau local physique.
* **Interface graphique :** **aucune** (installation en mode serveur / ligne de commande).
* **Service à installer :** `ssh` (SSH activé et démarré).
* **Mémoire :** 1024 Mo.
* **Disque :** 100 Go (allocation dynamique).

### `🚧`︲Couples d’identifiants (__NON NÉGOCIABLE__)

```
ID : root    | MDP : btssio
ID : btssio  | MDP : btssio
```

* Créer les deux comptes avec ces mots de passe lors de l’installation / configuration.
* Vérifier les droits sudo si nécessaire pour l’utilisateur `btssio`.

---

> [!TIP]
> [🎥︲Vidéo explicative – Cliquez-ici (Dona.One)]()

> [!TIP]
> [🎥︲Vidéo explicative – Cliquez-ici (Mega.nz)]()

---

## Étapes recommandées (résumé rapide)

1. Préparer la VM dans l’hyperviseur (taille RAM : 1024 Mo, disque : 100 Go — dynamique).
2. Démarrer l’installateur Debian 13 en utilisant le **miroir local**.
3. Choisir l’installation **sans environnement de bureau** (server/console only).
4. Configurer le réseau en **DHCP** pour l’interface principale.
5. Définir le `hostname` : `srv-fog`.
6. Créer/valider les comptes et mots de passe fournis (root + btssio).
7. Installer et activer le service `ssh` (ex. `apt install openssh-server` puis `systemctl enable --now ssh`).
8. Finaliser et redémarrer la VM, puis vérifier la connectivité SSH et l’adresse IP reçue via DHCP (`ip a` / `ip addr show`).







