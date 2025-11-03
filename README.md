# ` ☁️ `︲Documentation TP : Installer et configurer un service de déploiement FOG

---

Ce dépôt présente un guide complet pour l’installation, la configuration et l’utilisation du service de déploiement **FOG** sous **Debian 13**.
Tu y apprendras à **installer le serveur**, **capturer une image master**, et **déployer celle-ci** sur plusieurs machines clientes.

---

> [!IMPORTANT]
> Les vidéos et captures d’écran seront ajoutées progressivement.
> Chaque section sera accompagnée de commandes, de précisions techniques et de recommandations
> Les vidéos sont hébergées sur `dona.one`.
> Autrement utilisez Mega.nz pour visionner les vidéos.

---

## `📑`︲Sommaire (cliquez pour accéder directement à la section souhaitée)

1. [`📘`︲Introduction](#introduction)

   * [`❔`︲Contexte et objectifs du TP](#contexte-et-objectifs)
   * [`🧰`︲Présentation des outils et prérequis](#presentation-outils-prerequis)

2. [`🛠️`︲Installation de Debian 13 sans interface graphique](#installation-debian)

   * [`💿`︲Création et configuration de la VM](#creation-vm)
   * [`⚙️`︲Installation du système Debian 13](#installation-systeme)
   * [`🔐`︲Création des comptes et configuration SSH](#configuration-ssh)

3. [`🌍`︲Installation du service FOG](#installation-fog)

   * [`⬇️`︲Téléchargement et préparation de l’archive](#telechargement-fog)
   * [`🧩`︲Procédure d’installation du serveur FOG](#installation-serveur-fog)
   * [`🔧`︲Configuration initiale et tests](#configuration-initiale-fog)

4. [`⚙️`︲Configuration du service FOG](#configuration-service-fog)

   * [`🌐`︲Paramètres réseau et DHCP](#parametres-reseau)
   * [`⏱️`︲Modification du menu PXE et du timeout](#modification-pxe)
   * [`👥`︲Augmentation du nombre de clients simultanés](#clients-simultanes)

5. [`🖼️`︲Création d’une image et d’un groupe de déploiement](#creation-image-groupe)

   * [`📷`︲Création d’une image “S126-master”](#creation-image)
   * [`👥`︲Création d’un groupe de déploiement et assignation](#creation-groupe)

6. [`💻`︲Préparation de la machine master](#preparation-master)

   * [`🪛`︲Installation du logiciel personnalisé](#logiciel-master)
   * [`📋`︲Recensement de la machine sur le serveur FOG](#recensement-machine)
   * [`🏷️`︲Renommage et validation](#renommage-machine)

7. [`📦`︲Capture de l’image master](#capture-image)

   * [`🧭`︲Préparation avant capture](#preparation-capture)
   * [`📸`︲Lancer la capture et suivi du processus](#lancer-capture)

8. [`🚀`︲Déploiement de l’image par multicast](#deploiement-image)

   * [`🌐`︲Préparation de l’environnement réseau](#preparation-deploiement)
   * [`⚡`︲Lancement du déploiement multicast](#lancement-deploiement)
   * [`✅`︲Vérifications post-déploiement](#verification-deploiement)

9. [`🧩`︲Fonctionnalités avancées (optionnel)](#fonctionnalites-avancees)

   * [`🔁`︲Mise à jour du serveur FOG](#maj-fog)
   * [`🔒`︲Intégration au domaine Active Directory](#integration-ad)
   * [`📡`︲Utilisation d’un second réseau dédié au déploiement](#reseau-dedie)

10. [`📚`︲Conclusion et bonnes pratiques](#conclusion)

11. [`🧰`︲Outils et ressources utilisées](#outils-ressources)

---

<a id="introduction"></a>
## `📘`︲Introduction

---

<a id="contexte-et-objectifs"></a>
> [!NOTE]
> Tu vas apprendre à configurer un domaine, comprendre le rôle d’un contrôleur de domaine, gérer efficacement les utilisateurs et les groupes, appliquer des stratégies de groupe (GPO) et automatiser certaines tâches courantes grâce à PowerShell. > L’objectif est de te permettre de mettre en place un environnement réseau fonctionnel et de maîtriser les bases essentielles de l’administration système dans un contexte professionnel.

---

<a id="presentation-outils-prerequis"></a>
> [!IMPORTANT]
> Présentation des outils et prérequis :
> - **Serveur :** Debian 13 **sans interface graphique**
> - **Client :** Windows 11.
> - **Outils :** FOG (Free Open-Source Ghost) `V.1.5.10.1721` [`🌐`](https://fogproject.org/)

---





