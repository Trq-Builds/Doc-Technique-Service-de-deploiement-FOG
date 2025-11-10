# ` 👻 `︲Documentation TP : Installer et configurer un service de déploiement FOG.

---

Ce dépôt présente un guide complet pour l’installation, la configuration et l’utilisation du service de déploiement **FOG** (Free Open-Source Ghost) sous **Debian 13**.
Tu y apprendras à **installer le serveur**, **capturer une image master**, et **déployer celle-ci** sur plusieurs machines clientes.

---

> [!IMPORTANT]
> Les vidéos et captures d’écran seront ajoutées progressivement.
> ~~Les **vidéos** sont hébergées sur `dona.one`.~~
> Autrement utilisez `mega.nz` pour visionner les vidéos.

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

   * [`📷`︲Création d’une image “S126-Master”.](#creation-image)
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
# `📘`︲Introduction.

---

<a id="contexte-et-objectifs"></a>
### `❔`︲Contexte et objectifs du TP.
> [!NOTE]
> Tu vas apprendre à installer et configurer un serveur FOG, à capturer et déployer des images système sur un parc de machines, à gérer les configurations réseau (DHCP, multicast), et à automatiser le déploiement d’environnements Windows ou Linux dans un contexte professionnel.
> L’objectif est de te permettre de maîtriser les bases de la gestion de parc informatique et de la clonage d’images via FOG, un outil libre et puissant, afin de déployer rapidement et efficacement des postes de travail dans un environnement de type SISR.

---

<a id="presentation-outils-prerequis"></a>
### `🧰`︲Présentation des outils et prérequis.
> [!IMPORTANT]
> Présentation des outils et prérequis :
> - `🟦`︲**Serveur :** Debian 13 **sans interface graphique** ︲[`🌐`](https://www.debian.org/)
> - `🟦`︲**Client :** Windows 11. ︲[`🌐`](https://www.microsoft.com/fr-fr/software-download/windows11)
> - `🧰`︲**Outils :** FOG (Free Open-Source Ghost) `V.1.5.10.1721` ︲[`🌐`](https://fogproject.org/)
> - `📦`︲**VMWare :** ︲[`🌐`](https://www.vmware.com/)
---

<a id="installation-debian"></a>
# `🛠️`︲Installation de Debian 13 sans interface graphique.

---

<a id="creation-vm"></a>
## `💿`︲Création et Configuration de la VM.

> [!TIP]
> **Pour afficher les captures d’écran, clique sur le menu déroulant avec l’émoji `📸`. Le menu s’ouvrira et affichera la ou les captures d’écran.**

---

### `⚙️`︲Configuration de la VM.

* `❓` ︲**Hostname :** `srv-fog`.
* `📡` ︲**Adressage IP :** dynamique (DHCP) récupérer une adresse sur le réseau local physique.
* `🖼️` ︲**Interface graphique :** **__aucune__** (__installation en mode serveur / ligne de commande__).
* `🧩` ︲**Service à installer :** `ssh` (SSH activé et démarré).
* `📏` ︲**Mémoire :** `1024 Mo`.
* `💾` ︲**Disque :** `100 Go` (allocation dynamique).
* `❤️` ︲**Cœurs :** `1`. (Optionel)

<details>
  <summary><strong>🖼️︲Capture d'écran (Config VMWare).</strong></summary>
    <img width="887" height="880" alt="image" src="https://github.com/user-attachments/assets/8671256d-3603-45e0-af58-d9cf730d5d52" />
</details>


### `🚧`︲Couples d’identifiants (__NON NÉGOCIABLE__).

```
ID : root    | MDP : btssio
ID : btssio  | MDP : btssio
```

* Créer les deux comptes avec ces mots de passe lors de l’installation / configuration.
* Vérifier les droits sudo si nécessaire pour l’utilisateur `btssio`.

---

<a id="installation-systeme"></a>
## `⚙️`︲Installation du système Debian 13.

---

> [!NOTE]
> Cette partie couvre **l’installation de Debian 13 sans interface graphique**.
> Objectif : obtenir un serveur minimaliste, stable et prêt pour héberger FOG !

---

1️⃣︲**Lancement de l’installation depuis l’ISO Debian 13.**

* Sélectionner **Installation** (pas “Graphical install”).
* Choisir la **langue :** `Français`
* Choisir le **pays :** `France`
* Disposition clavier : **Français (AZERTY)**

<details>
  <summary>📸︲Installation et sélection langue et clavier.</summary>
  
<img width="641" height="482" alt="image" src="https://github.com/user-attachments/assets/f7a1f54b-9540-48ce-b3a9-bf605673302b" />
<img width="800" height="593" alt="image" src="https://github.com/user-attachments/assets/035692f5-ee65-4936-9856-041100e57798" />
</details>

---

2️⃣︲**Configuration réseau.**

* Nom de la machine : `srv-fog`
* Méthode d’adressage : **DHCP (automatique)**
* Domaine : *(laisser vide ou local)*

<details>
  <summary>📸︲Identifiants.</summary>

<img width="799" height="594" alt="image" src="https://github.com/user-attachments/assets/e74ab1d5-33ba-4686-b6c8-c50b86963e06" />
<img width="802" height="595" alt="image" src="https://github.com/user-attachments/assets/2266ccbd-f9a3-4ab6-8cd0-d2c079c9fecf" />
<img width="801" height="596" alt="image" src="https://github.com/user-attachments/assets/40ccea59-94b4-4ba1-88f4-25eeb0370e81" />
<img width="796" height="592" alt="image" src="https://github.com/user-attachments/assets/4c376e36-fd1f-439c-a746-8a3c903564aa" />
<img width="797" height="591" alt="image" src="https://github.com/user-attachments/assets/b0d8cc27-4859-451b-92a6-fd5cf0d9bc81" />
</details>

---

3️⃣︲**Partitionnement du disque.**

* Disque virtuel : **100 Go dynamiquement alloué**
* Schéma recommandé :

  * `/` → 60 Go
  * `swap` → 2 Go
  * `/var` → reste du disque
* Type : **Guidé – utiliser tout le disque**, séparé selon les besoins.

<details>
  <summary>📸︲Partitionnement automatique.</summary>
<img width="802" height="594" alt="image" src="https://github.com/user-attachments/assets/ed6046dd-c312-47fb-bce0-79ef312d88bb" />
<img width="797" height="597" alt="image" src="https://github.com/user-attachments/assets/d401aafe-522b-4d87-9a58-c75c25c8f6ba" />
<img width="794" height="589" alt="image" src="https://github.com/user-attachments/assets/65948516-8dcc-40c3-883d-1375b6a9f65e" />
<img width="803" height="588" alt="image" src="https://github.com/user-attachments/assets/46bb2f9c-b96e-4986-b22c-91a0dae464a1" />
<img width="801" height="596" alt="image" src="https://github.com/user-attachments/assets/07d8ee6b-4e9b-406f-baf1-948ba7fe5bad" />
</details>

---

4️⃣︲**Sélection des paquets à installer.**

* Ne **pas** installer d’environnement graphique.
* Cocher uniquement :

- [ ] environnement de bureau Debian
- [ ] ... GNOME
- [ ] ... Xfce
- [ ] ... bureau GNOME Flashback
- [ ] ... KDE Plasma
- [ ] ... Cinnamon
- [ ] ... MATE
- [ ] ... LXDE
- [ ] ... LXQt
- [ ] serveur web
- [x] serveur SSH
- [x] utilitaires usuels du système
- [ ] choix d’un assemblage (Blend) de Debian lors de l’installation

---

5️⃣︲**Installation du chargeur de démarrage (GRUB).**

* Installer sur le disque principal `/dev/sda`.
* Une fois l’installation terminée : **retirer l’ISO et redémarrer.**

<details>
  <summary>📸︲Installation Grub et redémarrage.</summary>
<img width="803" height="595" alt="image" src="https://github.com/user-attachments/assets/171b3e92-fc9a-4348-8f36-5ea8fcfd55be" />
<img width="802" height="596" alt="image" src="https://github.com/user-attachments/assets/868f4416-77c3-4f45-a338-913e8c6595b3" />
</details>

---
> [!WARNING]
> **Prends un snapshot de ta VM à ce stade (avant de configurer SSH). Cela te permettra de revenir rapidement en arrière en cas de problème avec la configuration réseau ou si FOG rencontre une erreur par la suite**

---

<a id="configuration-ssh"></a>
## `🔐`︲Création des comptes et configuration SSH.

---

> [!NOTE]
> Cette section configure les **utilisateurs**, le **SSH** et la **sécurisation basique du serveur**.
> Indispensable avant de passer à l’installation de FOG.

---

1️⃣︲**Création des utilisateurs.**

* Utilisateur root : `root / btssio`
* Utilisateur standard : `btssio / btssio`
* Vérifie que les deux existent avec :

  ```bash
  cat /etc/passwd | grep btssio
  ```

---

2️⃣︲**Activation du SSH.**

* S’assurer que le paquet est installé :
* En mode ROOT : 

  ```bash
   apt install openssh-server -y
  ```
* Démarrer et activer le service :

  ```bash
  sudo systemctl enable ssh --now
  sudo systemctl status ssh
  ```

<details>
  <summary>📸︲Vérification du service SSH.</summary>
<img width="857" height="814" alt="image" src="https://github.com/user-attachments/assets/22247b4e-f8f5-41a4-8da9-cb546dd40862" />
</details>

---

3️⃣︲**Autoriser la connexion root (optionnel).**

* Éditer le fichier de configuration :

  ```bash
  sudo nano /etc/ssh/sshd_config
  ```
* Modifier / vérifier ces lignes :

  ```
  PermitRootLogin yes
  PasswordAuthentication yes
  ```
* Redémarrer SSH :

  ```bash
  sudo systemctl restart ssh
  ```

> [!WARNING]
> **Ne laissez jamais le compte root activé en production : il ne doit être utilisé que pour les besoins du TP ou des tests internes.**

---

4️⃣︲**Test de connexion distante.**
Depuis la machine hôte :

```bash
ssh btssio@<ip_du_serveur>
```

ou

```bash
ssh root@<ip_du_serveur>
```

<details>
  <summary>📸︲Connexion SSH réussie. (Putty )</summary>
<img width="1481" height="914" alt="image" src="https://github.com/user-attachments/assets/d71b108c-696f-42d7-a863-e7ebfb7f4b94" />
</details>

---

> [!WARNING]
> **Prends un instantané de la VM après avoir validé le SSH, afin de pouvoir y revenir avant d’installer le service FOG.**

---

<a id="installation-fog"></a>
## `🌍`︲Installation du service FOG.

---

> [!NOTE]  
> Cette partie détaille l’installation complète du **service FOG (Free Open-Source Ghost)** sur **Debian 13**.  
> Objectif : mettre en place un serveur de déploiement d’images prêt à capturer et diffuser des systèmes Windows.

---

<a id="telechargement-fog"></a>
### `⬇️`︲Téléchargement et préparation de l’archive.

---

1️⃣︲**Installation des dépendances requises.**

Avant d’installer FOG, on met à jour le système et on installe les outils nécessaires :

```bash
sudo apt update && sudo apt install -y unzip git curl wget lsb-release net-tools
````

> [!TIP]
> 💡 Ces paquets garantissent le bon fonctionnement du script d’installation FOG (gestion réseau, extraction, téléchargements, etc.).

---

2️⃣︲**Téléchargement du projet FOG.**

On récupère la version 1.5.10.1721 du projet depuis GitHub :

```bash
cd /opt/
sudo wget https://api.github.com/repos/FOGProject/fogproject/zipball/1.5.10.1721 -O fogproject.zip
sudo unzip fogproject.zip
```

> [!TIP]
> Si le dossier extrait possède un nom long (`FOGProject-fogproject-*`), renomme-le simplement :
>
> ```bash
> sudo mv FOGProject-fogproject-* fogproject
> ```

<details>
  <summary>📸︲Téléchargement et extraction</summary>
<img width="825" height="701" alt="image" src="https://github.com/user-attachments/assets/a69c78e0-e50d-4d50-917a-f7414c4f9110" />
<img width="985" height="861" alt="image" src="https://github.com/user-attachments/assets/82888635-ad99-4343-8d46-c77c2f8da117" />
</details>

---

3️⃣︲**Accès au répertoire d’installation.**

Déplace-toi dans le dossier contenant le script principal :

```bash
cd /opt/fogproject/bin/
```

<details>
  <summary>📸︲Navigation vers le dossier `bin/`</summary>
  <img width="985" height="861" alt="image" src="https://github.com/user-attachments/assets/9d51d583-3959-4a61-8136-ee61bb5722bc" />
</details>

---

<a id="installation-serveur-fog"></a>
### `🧩`︲Procédure d’installation du serveur FOG.

---

> [!NOTE]
> L’installation s’effectue via un script automatisé qui configure tous les services requis :
> **Apache**, **PHP**, **MariaDB**, **TFTP**, **DHCP**, et le **daemon FOG**.

---

1️⃣︲**Lancement du script d’installation.**

```bash
sudo ./installfog.sh
```

Lors du processus, plusieurs questions seront posées :

| Question                      | Réponse attendue              |
| ----------------------------- | ----------------------------- |
| Version Linux ?               | `2` (Debian Based Linux       |
| Type d’installation ?         | `N` (Normal Server)           |
| Interface réseau détectée ?   | `Entrée` (valider par défaut) |
| Activer DHCP sur ce serveur ? | `Y`                           |
| Sécuriser MariaDB ?           | `N`                           |
| Activer HTTPS ?               | `N`                           |

---
Récapitulatif rapide des options : 

- [x] **Base Linux:** Debian  
- [x] **Detected Linux Distribution:** Debian GNU/Linux  
- [x] **Interface:** ens33  
- [x] **Server IP Address:** 192.168.23.135  
- [x] **Server Subnet Mask:** 255.255.255.0  
- [x] **Hostname:** srvfog  
- [x] **Installation Type:** Normal Server  
- [x] **Internationalization:** No  
- [x] **Image Storage Location:** /images  
- [x] **Using FOG DHCP:** Yes  
- [x] **DHCP Router Address:** 192.168.23.2  
- [x] **Send OS Name, OS Version, and FOG Version:** No
<details>
  <summary>📸︲Copie d'écran</summary>
<img width="612" height="833" alt="image" src="https://github.com/user-attachments/assets/57116a47-b443-43e4-9008-b2aff9e5cfb7" />
</details>




---

2️⃣︲**Validation du hostname et de l’adresse IP.**

Confirme simplement les valeurs par défaut si le nom `srv-fog` et l’adresse DHCP correspondent à ta configuration.

---

3️⃣︲**Installation automatique.**

Le script installe les dépendances et configure FOG.
Une fois terminé, un message t’indiquera d’accéder à l’interface web pour finaliser la configuration.

<details>
  <summary>📸︲Installation du script FOG</summary>
<img width="637" height="836" alt="image" src="https://github.com/user-attachments/assets/b2ac5481-4ddd-4058-99cf-6f3d1b64870a" />
</details>

---

4️⃣︲**Configuration web initiale.**

Depuis ton navigateur (machine hôte ou autre VM sur le même réseau) :

```
http://<ip_du_serveur_fog>/fog/management
```

> Exemple : `http://192.168.1.25/fog/management`

Sur la page d’accueil, clique sur :
➡️ **“Install/Update Now”** pour lancer la création de la base de données FOG.

<details>
  <summary>📸︲Page web d’installation</summary>
<img width="1774" height="1048" alt="image" src="https://github.com/user-attachments/assets/cf84e032-fa31-42b7-9320-6f205b2913e8" />
</details>

---

> [!TIP]
> 💾 Une fois l’installation web terminée, effectue un **snapshot de ta VM `srv-fog`**.
> Cela te permettra de revenir à cet état avant les configurations suivantes.

---

<a id="configuration-initiale-fog"></a>
### `🔧`︲Configuration initiale et tests

---

> [!NOTE]
> Avant d’aller plus loin, on vérifie le bon fonctionnement global de FOG (services, réseau, TFTP, etc.).

---

1️⃣︲**Connexion au tableau de bord FOG.**

Rends-toi à nouveau sur :

```
http://<ip_du_serveur_fog>/fog/management
```

Par défaut :

* **Utilisateur :** `fog`
* **Mot de passe :** `password` *(à modifier plus tard)*

---

2️⃣︲**Vérification des services actifs.**

Sur le tableau de bord (**Dashboard**), assure-toi que tous les services sont affichés en vert ✅ :

* FOG Scheduler
* FOG Multicast Manager
* FOG Image Replicator
* FOG Task Scheduler

<details>
  <summary>📸︲Tableau de bord FOG</summary>

*(Capture de la page Dashboard montrant les services actifs)*

</details>

---

3️⃣︲**Vérifier le service DHCP.**

FOG gère son propre service DHCP.
Vérifie qu’il est actif :

```bash
sudo systemctl status isc-dhcp-server
```

> [!TIP]
> Passe ton **réseau en “interne”** pour éviter les conflits avec le DHCP !

---

4️⃣︲**Tester le service TFTP.**

Pour t’assurer que le boot PXE fonctionnera correctement :

```bash
tftp <ip_du_serveur_fog>
tftp> get default.ipxe
```

Le fichier `default.ipxe` doit se télécharger sans erreur.
Sinon, vérifie le service TFTP :

```bash
sudo systemctl status tftpd-hpa
```

<details>
  <summary>📸︲Test du service TFTP</summary>

*(Capture du test `tftp` réussi)*

</details>

---

5️⃣︲**Modifier le mot de passe par défaut (recommandé).**

Une fois tout opérationnel :
`User Management → fog → Edit → Change Password`

> [!WARNING]
> **Ne laisse jamais le mot de passe `password` par défaut, même pour un simple TP en réseau local.**

---

> [!TIP]
> 🎯 À ce stade, ton serveur FOG est **installé, opérationnel et prêt pour la configuration avancée**.
> Tu peux maintenant passer à la section suivante :
> `⚙️︲Configuration du service FOG` pour affiner les paramètres (menu PXE, délais, nombre de clients simultanés, etc.).

---

<a id="configuration-service-fog"></a>
# `⚙️`︲Configuration du service FOG.













