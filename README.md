# ` 👻 `︲Documentation TP : Installer et configurer un service de déploiement FOG.

<p align="center">
<img src="https://github.com/user-attachments/assets/2582985d-c4a0-48a7-86ff-64daa2e0044c" width="100"/>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/FOG-imaging-orange?style=for-the-badge">
  <img src="https://img.shields.io/badge/Linux-server-333333?logo=linux&logoColor=white&style=for-the-badge">
  <img src="https://img.shields.io/badge/PXE-boot-blue?style=for-the-badge">
  <img src="https://img.shields.io/badge/iPXE-networkboot-8A2BE2?style=for-the-badge">
  <img src="https://img.shields.io/badge/Markdown-made%20with-2B7489?logo=markdown&style=for-the-badge">
</p>

---

Ce dépôt présente un guide complet pour le déploiement de FOG (Free Open-Source Ghost) sous Debian 13. Il couvre l'installation, la configuration et l'utilisation du service. Tu y apprendras à installer le serveur FOG, à capturer une image master, et à la déployer rapidement sur l'ensemble de tes machines clientes !

---

## `📑`︲Sommaire (cliquez pour accéder directement à la section souhaitée)

1. [`📘`︲Introduction.](#introduction)

   * [`❔`︲Contexte et objectifs du TP.](#contexte-et-objectifs)
   * [`🧰`︲Présentation des outils et prérequis.](#presentation-outils-prerequis)

   ---

2. [`🛠️`︲Installation de Debian 13 sans interface graphique.](#installation-debian)

   * [`💿`︲Création et configuration de la VM.](#creation-vm)
   * [`⚙️`︲Installation du système Debian 13.](#installation-systeme)
   * [`🔐`︲Création des comptes et configuration SSH.](#configuration-ssh)

   ---

3. [`🌍`︲Installation du service FOG.](#installation-fog)

   * [`⬇️`︲Téléchargement et préparation de l’archive.](#telechargement-fog)
   * [`🧩`︲Procédure d’installation du serveur FOG.](#installation-serveur-fog)
   * [`🔧`︲Configuration initiale et tests.](#configuration-initiale-fog)

   ---

4. [`⚙️`︲Configuration du service FOG.](#configuration-service-fog)

   * [`🌐`︲Paramètres réseau et DHCP.](#parametres-reseau)
   * [`⏱️`︲Modification du menu PXE et du timeout.](#modification-pxe)
   * [`👥`︲Augmentation du nombre de clients simultanés.](#clients-simultanes)
   
   ---

5. [`🖼️`︲Création d’une image et d’un groupe de déploiement.](#creation-image-groupe)

   * [`📷`︲Création d’une image “S126-Master”.](#creation-image)
   * [`👥`︲Création d’un groupe de déploiement et assignation.](#creation-groupe)

   ---

6. [`💻`︲Préparation de la machine master.](#preparation-master)

   * [`🧰`︲Installation du logiciel personnalisé.](#logiciel-master)
   * [`📋`︲Recensement de la machine sur le serveur FOG.](#recensement-machine)
   * [`🏷️`︲Renommage et validation.](#renommage-machine)

   ---

7. [`📦`︲Capture de l’image Master.](#capture-image)

   * [`🧭`︲Préparation avant capture.](#preparation-capture)
   * [`📸`︲Lancer la capture et suivi du processus.](#lancer-capture)

   ---

8. [`🚀`︲Déploiement de l’image.](#deploiement-image)

   * [`🌐`︲Préparation de l’environnement réseau.](#preparation-deploiement)
   * [`⚡`︲Lancement du déploiement.](#lancement-deploiement)
   * [`✅`︲Vérifications post-déploiement.](#verification-deploiement)

   ---
   
9. [`🧰`︲Outils et ressources utilisées.](#outils-ressources)
10. [`📚`︲Technique et méthode.](#technique-methode)

---

<img width="1128" height="287" alt="image" src="https://github.com/user-attachments/assets/60d844c6-6575-487d-b95c-f1b2b854f067" />

---

> [!TIP]
> **Pour naviguer rapidement dans le dépôt, vous pouvez également utiliser le sommaire généré automatiquement par GitHub, situé en haut à droite de ce cadre !**

---

<a id="introduction"></a>
# `📘`︲Introduction.

---

<a id="contexte-et-objectifs"></a>
### `❔`︲Contexte et objectifs du TP.
> [!NOTE]
> Tu vas apprendre à installer et configurer un serveur FOG, à capturer et déployer des images système sur un parc de machines, à gérer les configurations réseau (DHCP, etc...), et à automatiser le déploiement d’environnements Windows ou Linux dans un contexte professionnel.
> L’objectif est de te permettre de maîtriser les bases de la gestion de parc informatique et de la clonage d’images via FOG, un outil libre et puissant, afin de déployer rapidement et efficacement des postes de travail dans un environnement de type SISR.

---

<a id="presentation-outils-prerequis"></a>
### ` 🧰 `︲Présentation des outils et prérequis.
> [!IMPORTANT]
> **Présentation des outils et prérequis :**
> - ` 🐧 `︲**Serveur :** *Debian 13* **sans interface graphique** ︲[`🌐`](https://www.debian.org/)
> 
> - ` 🟦 `︲**Windows 11**  ︲[`🌐`](https://www.microsoft.com/fr-fr/software-download/windows11)
> 
> - ` 🧰 `︲**Outil :** *FOG (Free Open-Source Ghost)* `V.1.5.10.1721` ︲[`🌐`](https://fogproject.org/)
> 
> - ` 📦 `︲**VMWare :** ︲[`🌐`](https://www.vmware.com/)
> 
> - ` ⚡ `︲**PuTTY :** ︲[`🌐`](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)
> 
> - ` 👤 `︲**Interface Chaise-Clavier fonctionnelle.** 🫵
> 
> - ` ☕ `︲**Un peu de patience !**

---

<a id="installation-debian"></a>
# ` 🛠️ `︲Installation de Debian 13 sans interface graphique.

---

<a id="creation-vm"></a>
## ` 💿 `︲Création et Configuration de la VM.

> [!IMPORTANT]
> * **Les captures d’écran seront ajoutées progressivement !**
> * **Si une image est peu lisible dans le menu, il suffit de cliquer dessus. L'image s'ouvrira dans un nouvel onglet, vous permettant ainsi de la consulter en taille réelle et d'utiliser la fonction zoom !**

> [!TIP]
> - **Pour afficher les captures d’écran, clique sur le menu déroulant avec l’émoji : `  📸  `.**
> - **Le menu s’ouvrira et affichera la ou les captures d’écran !**

---

` ⚙️ `︲**Configuration de la VM.**

* ` ❓ ` ︲**Hostname :** `srv-fog`.

* ` 📡 ` ︲**Adressage IP :** dynamique (`DHCP`) récupérer une adresse sur le réseau local physique.

* ` 🖼️ ` ︲**Interface graphique :** **__aucune__** (__installation en mode serveur / ligne de commande__).

* ` 🧩 ` ︲**Service à installer :** `ssh` **(SSH activé et démarré)**.

* ` 📏 ` ︲**Mémoire :** `1024 Mo`.

* ` 💾 ` ︲**Disque :** `100 Go`. (*allocation dynamique*).

* ` ❤️ ` ︲**Cœurs :** `1`. (*Optionel*)

<details>
  <summary><strong>📸︲Capture d'écran (Config VMWare).</strong></summary>
  
  <img width="887" height="880" alt="image" src="https://github.com/user-attachments/assets/8671256d-3603-45e0-af58-d9cf730d5d52" />
  
</details>

---

` 🚧 `︲**Couples d’identifiants (__NON NÉGOCIABLE__).**

```
ID : root    | MDP : btssio
ID : btssio  | MDP : btssio
```

* Créer les deux comptes avec ces mots de passe lors de l’installation / configuration.
* Vérifier les droits sudo si nécessaire pour l’utilisateur `btssio`.

---

<a id="installation-systeme"></a>
## ` ⚙️ `︲Installation du système Debian 13.

---

> [!NOTE]
> Cette partie couvre **l’installation de Debian 13 sans interface graphique**.
> Objectif : obtenir un serveur minimaliste, stable et prêt pour héberger FOG !

---

1️⃣︲**Lancement de l’installation depuis l’ISO Debian 13.**

* Sélectionner `Install` (pas “Graphical install”).
* Choisir la **langue :** `Français`
* Choisir le **pays :** `France`
* Disposition clavier : **Français (`AZERTY`)**

<details>
  <summary>📸︲Installation et sélection langue et clavier.</summary>

  <img width="641" height="482" alt="image" src="https://github.com/user-attachments/assets/f7a1f54b-9540-48ce-b3a9-bf605673302b" />
  
  --- 
  
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

--- 

<img width="802" height="595" alt="image" src="https://github.com/user-attachments/assets/2266ccbd-f9a3-4ab6-8cd0-d2c079c9fecf" />

---

<img width="801" height="596" alt="image" src="https://github.com/user-attachments/assets/40ccea59-94b4-4ba1-88f4-25eeb0370e81" />

---

<img width="796" height="592" alt="image" src="https://github.com/user-attachments/assets/4c376e36-fd1f-439c-a746-8a3c903564aa" />

---

<img width="797" height="591" alt="image" src="https://github.com/user-attachments/assets/b0d8cc27-4859-451b-92a6-fd5cf0d9bc81" />

</details>

---

3️⃣︲**Partitionnement du disque.**

* Disque virtuel : `100Go` **dynamiquement alloué**
* Schéma recommandé :

  * `/` → `60Go`
  * `swap` → `2Go`
  * `/var` → reste du disque
* Type : **Guidé – utiliser tout le disque**, séparé selon les besoins.

<details>
  <summary>📸︲Partitionnement automatique.</summary>
<img width="802" height="594" alt="image" src="https://github.com/user-attachments/assets/ed6046dd-c312-47fb-bce0-79ef312d88bb" />
  
---
  
<img width="797" height="597" alt="image" src="https://github.com/user-attachments/assets/d401aafe-522b-4d87-9a58-c75c25c8f6ba" />

---

<img width="794" height="589" alt="image" src="https://github.com/user-attachments/assets/65948516-8dcc-40c3-883d-1375b6a9f65e" />

---

<img width="803" height="588" alt="image" src="https://github.com/user-attachments/assets/46bb2f9c-b96e-4986-b22c-91a0dae464a1" />

---

<img width="801" height="596" alt="image" src="https://github.com/user-attachments/assets/07d8ee6b-4e9b-406f-baf1-948ba7fe5bad" />

</details>

---

4️⃣︲**Sélection des paquets à installer.**

* Ne **pas** installer d’environnement graphique.
* **Cocher uniquement :**

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
## ` 🔐 `︲Création des comptes et configuration SSH.

---

> [!NOTE]
> Cette section configure les **utilisateurs**, le **SSH** et la **sécurisation basique du serveur**.
> Indispensable avant de passer à l’installation de FOG !

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
* En mode **ROOT** : 

  ```bash
   apt install openssh-server -y
  ```
* Démarrer et activer le service :

  ```bash
  $ systemctl enable ssh --now
  $ systemctl status ssh
  ```

<details>
  <summary>📸︲Vérification du service SSH.</summary>
  
<img width="857" height="814" alt="image" src="https://github.com/user-attachments/assets/22247b4e-f8f5-41a4-8da9-cb546dd40862" />

</details>

---

3️⃣︲**Autoriser la connexion root (optionnel).**

* Éditer le fichier de configuration :

  ```bash
  $ nano /etc/ssh/sshd_config
  ```
* Modifier / vérifier ces lignes :

  ```
  PermitRootLogin yes
  PasswordAuthentication yes
  ```
* Redémarrer SSH :

  ```bash
  $ systemctl restart ssh
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
  <summary>📸︲Connexion SSH réussie (Putty).</summary>
  
<img width="1481" height="914" alt="image" src="https://github.com/user-attachments/assets/d71b108c-696f-42d7-a863-e7ebfb7f4b94" />

</details>

---

> [!WARNING]
> **Prends un instantané de la VM après avoir validé le SSH, afin de pouvoir y revenir avant d’installer le service FOG !**

---

<a id="installation-fog"></a>
# ` 🌍 `︲Installation du service FOG.

---

> [!NOTE]  
> Cette partie détaille l’installation complète du **service FOG (Free Open-Source Ghost)** sur **Debian 13**.  
> Objectif : **mettre en place un serveur de déploiement d’images prêt à capturer et diffuser des systèmes Windows.**

---

<a id="telechargement-fog"></a>
### ` ⬇️ `︲Téléchargement et préparation de l’archive.

---

1️⃣︲**Installation des dépendances requises.**

Avant d’installer FOG, on met à jour le système et on installe les outils nécessaires :

```bash
$ apt update && $ apt install -y unzip git curl wget lsb-release net-tools
````

> [!TIP]
> **Ces paquets garantissent le bon fonctionnement du script d’installation FOG (gestion réseau, extraction, téléchargements, etc...)**

---

2️⃣︲**Téléchargement du projet FOG.**

On récupère la version `1.5.10.1721` du projet depuis GitHub :

```bash
cd /opt/
$ wget https://api.github.com/repos/FOGProject/fogproject/zipball/1.5.10.1721 -O fogproject.zip
$ unzip fogproject.zip
```

> [!TIP]
> Si le dossier extrait possède un nom long (`FOGProject-fogproject-*`), renomme-le simplement :
>
> ```bash
> $ mv FOGProject-fogproject-* fogproject
> ```

<details>
  <summary>📸︲Téléchargement et extraction.</summary>
  
<img width="825" height="701" alt="image" src="https://github.com/user-attachments/assets/a69c78e0-e50d-4d50-917a-f7414c4f9110" />

--- 

<img width="985" height="861" alt="image" src="https://github.com/user-attachments/assets/82888635-ad99-4343-8d46-c77c2f8da117" />

</details>

---

3️⃣︲**Accès au répertoire d’installation.**

Déplace-toi dans le dossier contenant le script principal :

```bash
cd /opt/fogproject/bin/
```

<details>
  <summary>📸︲Navigation vers le dossier `bin/`.</summary>
  
  <img width="985" height="861" alt="image" src="https://github.com/user-attachments/assets/9d51d583-3959-4a61-8136-ee61bb5722bc" />
  
</details>

---

<a id="installation-serveur-fog"></a>
### ` 🧩 `︲Procédure d’installation du serveur FOG.

---

> [!NOTE]
> L’installation s’effectue via un script automatisé qui configure tous les services requis :
> **Apache**, **PHP**, **MariaDB**, **TFTP**, **DHCP**, et le **daemon FOG**.

---

1️⃣︲**Lancement du script d’installation.**

```bash
$ ./installfog.sh
```

Lors du processus, plusieurs questions seront posées : (**__Les questions dans le tableau ne sont pas dans l'ordre__.**)

| Question                      | Réponse attendue              |
| ----------------------------- | ----------------------------- |
| Version Linux ?               | `2` (Debian Based Linux       |
| Type d’installation ?         | `N` (Normal Server)           |
| Changer l'interface réseau par défaut ? | `N`                 |
| Setup Router Adress ?         | `Y`                           |
| DHCP handle DNS               | `Y`                           | 
| Interface réseau détectée ?   | `Entrée` (valider par défaut) |
| FOG DHCP Service ?| `Y`       | `Y`                           |
| Installer les langues supplementaires | `N`                   |
| Sécuriser MariaDB ?           | `N`                           |
| Activer HTTPS ?               | `N`                           |
| Change Host-Name ?            | `N`                           |
| Télémetrie ?                  | `N`                           |

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
  <summary>📸︲Copie d'écran.</summary>
  
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
  <summary>📸︲Installation du script FOG.</summary>
  
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
**“Install/Update Now”** pour lancer la création de la base de données FOG !

<details>
  <summary>📸︲Page web d’installation.</summary>
  
<img width="1774" height="1048" alt="image" src="https://github.com/user-attachments/assets/cf84e032-fa31-42b7-9320-6f205b2913e8" />

</details>

---

> [!WARNING]
> **Une fois l’installation web terminée, crée un snapshot de ta VM afin de pouvoir revenir à cet état avant les prochaines configurations !**

---

<a id="configuration-initiale-fog"></a>
### ` 🔧 `︲Configuration initiale et tests.

---

> [!NOTE]
> Avant d’aller plus loin, on vérifie le bon fonctionnement global de FOG (services, réseau, TFTP, etc...)

---

1️⃣︲**Connexion au tableau de bord FOG.**

Rends-toi à nouveau sur :

```
http://<ip_du_serveur_fog>/fog/management
```

Par défaut :

* **Utilisateur :** `fog`
* **Mot de passe :** `password` *(à modifier plus tard ici test en Labo donc ce n'est pas grave)*
  
<details>
  <summary>📸︲Copie d'écran (Login).</summary>
  
  <img width="1783" height="982" alt="image" src="https://github.com/user-attachments/assets/a9b7d3db-2983-4546-863d-8da59ebbef29" />
  
</details>

<details>
  <summary>📸︲Tableau de bord FOG.</summary>
  
<img width="1787" height="1057" alt="image" src="https://github.com/user-attachments/assets/49bf6987-7a78-4eb2-ae31-30417bc12f55" />

</details>

---

2️⃣︲**Vérifier le service DHCP.**

**FOG gère son propre service DHCP.**
- **Vérifie qu’il est actif :**
```bash
$ systemctl status isc-dhcp-server
```

> [!TIP]
> Passe ton **réseau en “interne”** pour éviter les conflits avec le DHCP !

---

3️⃣︲**Tester le service TFTP.**

Pour t’assurer que le boot PXE fonctionnera correctement :

```bash
tftp <ip_du_serveur_fog>
tftp> get default.ipxe
```

Le fichier `default.ipxe` doit se télécharger sans erreur.
Sinon, vérifie le service TFTP :

```bash
$ systemctl status tftpd-hpa
```

<details>
  <summary>📸︲Test du service TFTP.</summary>
  
<img width="1089" height="701" alt="image" src="https://github.com/user-attachments/assets/0467a90e-1723-40fd-bb13-5ea6d8e21b27" />

</details>

---

3️⃣︲**Modifier le mot de passe par défaut (recommandé mais ici test en Labo).**

Une fois tout opérationnel :
`User Management → fog → Edit → Change Password`

> [!WARNING]
> **Ne laisse jamais le mot de passe `password` par défaut, ici c'est pour un simple TP en réseau local pour gagner du temps.**

---

> [!TIP]
> À ce stade, ton serveur FOG est **installé, opérationnel et prêt pour la configuration avancée !**.
> Tu peux maintenant passer à la section suivante pour affiner les paramètres (menu PXE, délais, nombre de clients simultanés, etc...)

---

<a id="configuration-service-fog"></a>
# ` ⚙️ `︲Configuration du service FOG.

---

> [!NOTE]  
> Cette partie te guide dans la **configuration avancée du service FOG**, afin d’adapter son comportement à ton environnement réseau de TP.  
> Objectif : ajuster le DHCP, modifier le délai du menu PXE et optimiser le nombre de connexions simultanées.

---

<a id="parametres-reseau"></a>
### ` 🌐 `︲Paramètres réseau et DHCP.

---

1️⃣︲**Isoler le réseau du serveur FOG**

Dans VMWare :  
- Passe la carte réseau du **serveur FOG** en mode **“Réseau interne”**.  
- Fais de même pour les **machines clientes** à déployer.
  
<details>
  <summary>📸︲Config VMWare.</summary>
  
  <img width="1197" height="933" alt="image" src="https://github.com/user-attachments/assets/0dd3b069-8b9e-4e01-accc-16ebde632b9c" />
    
</details>


> [!TIP]  
> Cela permet d’éviter les conflits DHCP avec le réseau de la salle et de s’assurer que **seul FOG attribue les adresses IP**.

---

2️⃣︲**Vérifier la configuration DHCP de FOG**

**FOG peut faire office de serveur DHCP.** 
Pour vérifier et ajuster la configuration :

```bash
$ nano /etc/dhcp/dhcpd.conf
````

Les lignes essentielles doivent ressembler à ceci :

```bash
subnet 192.168.100.0 netmask 255.255.255.0 {
    range dynamic-bootp 192.168.100.50 192.168.100.150;
    option routers 192.168.100.1;
    filename "undionly.kpxe";
    next-server 192.168.100.10;
}
```

> [!WARNING]
> **Ne modifie pas le fichier si tu n’es pas sûr de ton plan d’adressage.**
> **Une mauvaise configuration DHCP peut empêcher tout boot PXE.**

<details>
  <summary>📸︲Exemple de configuration DHCP.</summary>
  
<img width="1085" height="701" alt="image" src="https://github.com/user-attachments/assets/9356d95a-27ec-4928-af09-5f08839d44d9" />

</details>

---

3️⃣︲**Redémarrer les services FOG et DHCP**

```bash
$ systemctl restart isc-dhcp-server
$ systemctl restart FOGMulticastManager
$ systemctl restart FOGImageReplicator
$ systemctl restart FOGScheduler
```

<details>
  <summary>📸︲Redémarrage des services.</summary>
  
<img width="1085" height="701" alt="image" src="https://github.com/user-attachments/assets/94c235dd-1aaf-476e-bbbe-ab9b13cb8edd" />

</details>

---

> [!TIP]
> À ce stade, ton réseau interne FOG est isolé et opérationnel.
> Les clients peuvent désormais recevoir une adresse IP depuis le serveur FOG au boot PXE !

---

<a id="modification-pxe"></a>
### ` ⏱️ `︲Modification du menu PXE et du timeout.

---

> [!NOTE]
> Par défaut, le menu PXE du serveur FOG s’affiche 3 secondes.
> On va augmenter ce délai pour faciliter les manipulations et diagnostics.

---

1️⃣︲**Accéder à l’interface web FOG**

`Dashboard → FOG Configuration → PXE Boot Menu`

<details>
<summary>📸︲Copie d"écran.</summary>
  
<img width="1777" height="999" alt="image" src="https://github.com/user-attachments/assets/f9cf1be6-1945-48f9-badf-44986edfb79c" />

</details>

---

2️⃣︲**Modifier le délai d’affichage du menu**

Cherche le paramètre :

```
FOG_PXE_BOOT_MENU_TIMEOUT
```

Modifie la valeur :

```
10
```

Puis clique sur **“Save Changes”** pour enregistrer.

<details>
  <summary>📸︲Modification du délai PXE</summary>
  
<img width="1151" height="383" alt="image" src="https://github.com/user-attachments/assets/51dc3624-a08d-4b52-87c7-22342cc861dc" />

</details>

---

3️⃣︲**Vérifier le menu au démarrage d’un client**

Lance une VM cliente configurée pour booter en réseau (PXE).
Le menu FOG devrait maintenant s’afficher **pendant 10 secondes**.

<details>
  <summary>📸︲Affichage du menu PXE.</summary>
  
  <img width="803" height="604" alt="image" src="https://github.com/user-attachments/assets/904507c2-8a98-439d-b4c8-e60c15ca2381" />
  
</details>

---

> [!TIP]
> **Augmenter le délai du menu PXE évite de rater l’entrée en mode “Quick Registration” ou “Deploy Image” lors des tests en TP.**

---

<a id="clients-simultanes"></a>
### ` 👥 `︲Augmentation du nombre de clients simultanés.

---

> [!NOTE]
> Par défaut, FOG limite le nombre de connexions PXE simultanées à 10.
> On va augmenter cette valeur pour permettre un déploiement massif sur plusieurs postes à la fois.

---

1️⃣︲**Accéder aux paramètres avancés**

Depuis l’interface web :
`FOG Configuration → FOG Settings → TFTP Server`

---

2️⃣︲**Modifier le paramètre de sessions simultanées**

---

La modification du nombre de connexions simultanées dans FOG se fait via l’interface Web d’administration !

**Modifier le nombre de clients (Max Clients).**

1. Ouvrir **Storage Management**.
2. Sélectionner **DefaultMember** (stockage créé par défaut).
3. Ajuster la valeur **Max Clients** (limite par défaut : 10).

Ce paramétrage permet d’adapter FOG aux besoins, par exemple pour couvrir plusieurs salles de TP de 18 postes lors d’un déploiement.

**Modifier le délai d’affichage du menu PXE.**

1. Aller dans **FOG Configuration**.
2. Ouvrir **PXE Boot Menu**.
3. Modifier **Menu Timeout (in seconds)** puis valider avec **Save PXE MENU**.


> [!TIP]
> Pour un test en salle complète (18 à 20 machines), 30 connexions assurent un flux stable.

<details>
  <summary>📸︲Paramètre modifié.</summary>
  
<img width="1777" height="999" alt="image" src="https://github.com/user-attachments/assets/6188d04a-2b91-468c-af9b-24393eeccdf4" />

</details>

---

3️⃣︲**Redémarrer le service TFTP pour appliquer la modification**

```bash
$ systemctl restart tftpd-hpa
```

---

4️⃣︲**Vérification finale**

Teste un boot PXE simultané sur plusieurs machines virtuelles.
Toutes doivent atteindre l’écran de menu FOG sans erreur réseau.

> [!TIP]
> Si certains clients restent bloqués, augmente légèrement la limite (jusqu’à 50 connexions selon la puissance de ta machine hôte).

---

> [!TIP]
> Ton serveur FOG est maintenant **configuré, optimisé et prêt à gérer plusieurs déploiements en parallèle !**.

---

<a id="creation-image-groupe"></a>
# ` 🖼️ `︲Création d’une image et d’un groupe de déploiement.

---

> [!NOTE]  
> Cette partie consiste à préparer l’infrastructure FOG pour accueillir et gérer la future image master Windows 11 de la salle 126.  
> Objectif : créer une **image FOG** et un **groupe de déploiement** pour faciliter les opérations à grande échelle.

---

<a id="creation-image"></a>
### ` 📷 `︲Création d’une image “S126-master”.

---

1️⃣︲**Accéder au menu Images depuis l’interface FOG**

Interface Web → `Image Management` → `Create New Image`

---

2️⃣︲**Paramétrer l’image**

**Renseigne les champs suivants :**

| Paramètre | Valeur |
|----------|--------|
| **Image Name** | `S126-master` |
| **Image Description** | *Image master Windows 11 – Salle 126* |
| **Storage Group** | `default` |
| **Image Type** | `Single Disk - Resizable` |
| **OS** | `Windows 10/11` |
| **Image Path** | `/images/S126-master` |

> [!IMPORTANT]  
> **L’option Windows 11 n’est pas disponible, mais sélectionner Windows 10 fonctionnera quand même !**

> [!TIP]  
> Le type *Resizable* permet à FOG d’adapter la partition lors du déploiement, idéal pour les environnements homogènes.

<details>
  <summary>📸︲Création de l’image dans FOG.</summary>
  
<img width="1294" height="738" alt="image" src="https://github.com/user-attachments/assets/3bfe28f9-58ac-4e4a-a23d-418be5255508" />

</details>

---

3️⃣︲**Sauvegarder l’image**

Clique sur **“Add”** pour valider la création.

Tu verras apparaître ton image dans :
`Image Management → All Images`

<details>
  <summary>📸︲Image enregistrée.</summary>
  
<img width="1299" height="350" alt="image" src="https://github.com/user-attachments/assets/34fa040f-dc2e-4cbb-a666-9853ccfad912" />

</details>

---

<a id="creation-groupe"></a>
### ` 👥 `︲Création d’un groupe de déploiement.

---

1️⃣︲**Aller dans Group Management**

Interface Web → `Group Management` → `Create New Group`

---

2️⃣︲**Configurer le groupe**

| Paramètre | Valeur |
|----------|--------|
| **Group Name** | `Salle126` |
| **Group Description** | *Groupe des clients destinés à recevoir l’image S126-master* |

Clique ensuite sur **“Add”**.

<details>
  <summary>📸︲Création du groupe Salle 126.</summary>
  
<img width="1290" height="482" alt="image" src="https://github.com/user-attachments/assets/a59bc055-d069-42c4-a6e7-b99d4970fb7f" />

</details>

---

3️⃣︲**Associer le groupe à l’image**

Dans le groupe fraîchement créé :

`Group Management → Salle126 → Image`

Sélectionne :
```

S126-master

```
Puis clique sur **“Update”**.

> [!TIP]  
>  Cette étape est cruciale : elle permet de déployer automatiquement l’image sur **toutes les machines recensées dans le groupe** sans reconfigurer chaque client.

<details>
  <summary>📸︲Assignation de l’image au groupe.</summary>
  
<img width="1777" height="999" alt="image" src="https://github.com/user-attachments/assets/99cf6486-9613-493f-be73-316e1428f330" />

</details>

---

> [!TIP]  
> Tu as maintenant un **groupe de déploiement opérationnel**, prêt à recevoir des clients et à lancer un déploiement massif.

---

<a id="preparation-master"></a>
# ` 💻 `︲Préparation et recensement de la machine master.

---

> [!NOTE]  
> Ici, tu vas préparer la machine qui servira d’**image master** : installation d’un logiciel, recensement via FOG, et renommage dans l’interface.  
> **Cette étape est indispensable avant la capture de l’image.**

---

<a id="logiciel-master"></a>
### ` 🧰 `︲Installation du logiciel de personnalisation (au choix).

---

1️⃣︲**Créer la VM Windows 11 “Master”**

- Utilise une configuration proche des PC du centre de formation.  
- Désactive l’EFI dans VMWare (important pour le recensement FOG).  
- Une fois Windows 11 installé : mets à jour, configure et prépare l’environnement.

---

2️⃣︲**Installer un logiciel parmi la liste imposée**

Tu dois installer **un des logiciels suivants** sur la machine master :

- ` 📁 `︲**FileZilla Client** ︲[`🌐`](https://filezilla-project.org/)
- ` 💻 `︲**LibreOffice** ︲[`🌐`](https://fr.libreoffice.org/)
- ` 👨‍💻 `︲**Visual Studio Code** ︲[`🌐`](https://code.visualstudio.com/)
- ` 📦 `︲**WinRAR** ︲[`🌐`](https://www.win-rar.com/)

> [!TIP]  
> Choisis un logiciel visible et simple à repérer : idéal pour vérifier, après déploiement, que l’image a bien été appliquée.

> [!TIP]  
> **Dans mon cas, j’ai installé les quatre logiciels d'exemple ainsi que réalisé quelques modifications sur Windows pour vérifier si FOG est capable de les récupérer.**


<details>
  <summary>📸︲Bureau de la VM de Test (Master).</summary>
  
<img width="1039" height="778" alt="image" src="https://github.com/user-attachments/assets/c3a847d3-90b1-4f70-b2cf-341a347576b5" />

<img width="1035" height="766" alt="image" src="https://github.com/user-attachments/assets/c24ca75c-a490-4de0-8937-9875f2f97c98" />

</details>

---

3️⃣︲**Finaliser la machine avant recensement (Optionel)**

- ` 🧹 `︲**Nettoyer les fichiers inutiles.**
- ` ✅ `︲**Vérifier que Windows est activé.**
- ` 🔁 `︲**Vérifier les mises à jour.**
- ` 🔁 `︲**Redémarrer une dernière fois avant de passer au boot PXE.**

---

<a id="recensement-machine"></a>
### `📋`︲Recensement de la machine master dans FOG.

---

> [!NOTE]  
> Le recensement permet à FOG d’enregistrer la machine dans sa base afin de pouvoir la capturer.

---

1️⃣︲**Configurer le boot réseau de la VM master**

Dans VMWare :  
- Ne pas oublier de mettre les deux machines en 'LAN Segment' (réseau interne) afin que la machine Master puisse démarrer en PXE.

---

2️⃣︲**Démarrer la machine Master en PXE**

Au démarrage, la VM doit afficher le menu FOG.  
Sélectionne :

```

Quick Registration and Inventory

```

<details>
  <summary>📸︲Menu PXE  FOG</summary>
  
<img width="803" height="604" alt="image" src="https://github.com/user-attachments/assets/be68581a-fe4c-48c8-acc6-e2046e3e9d4a" />

</details>

---

3️⃣︲**Remplir les informations demandées**

- **Host name** : `master-s126` (exemple recommandé)  
- **Image association** : ne rien toucher pour le moment  
- **Autres champs** : valider par défaut

---

4️⃣︲**Redémarrer la machine**

Après l’enregistrement, laisse la machine redémarrer normalement.

> [!TIP]  
> Tu peux maintenant vérifier dans l’interface FOG :  
> `Host Management → All Hosts`  
> Ta machine doit apparaître dans la liste.

<details>
  <summary>📸︲Machine master recensée dans FOG</summary>
  
<img width="1756" height="927" alt="image" src="https://github.com/user-attachments/assets/425637ff-dd3e-4c31-a045-2c2a434f6ba2" />

</details>

---

<a id="renommage-machine"></a>
### `🏷️`︲Renommage et validations sur le serveur FOG.

---

1️⃣︲**Ouvrir la fiche de la machine enregistrée**

Interface Web → `Host Management → master-s126`

---

2️⃣︲**Vérifier / modifier si nécessaire**

- ` 🚦 `︲Hostname  
- ` ❓ `︲MAC address  
- ` ℹ️ `︲Description  
- ` ❔ `︲Groupe d’appartenance (**La machine master NE DOIT PAS aller dans un groupe client** !)

> [!WARNING]  
> Le master NE doit **jamais** être dans le groupe “Salle126”.  
> (Sinon FOG pourrait tenter de lui déployer sa propre image...)

---

3️⃣︲**Associer l’image à capturer (facultatif à ce stade)**

Tu peux associer l’image `S126-master`, mais ce n’est pas obligatoire avant la capture.

<details>
  <summary>📸︲Paramètres du host master dans FOG</summary>
  
<img width="1777" height="999" alt="image" src="https://github.com/user-attachments/assets/6fe59d1b-a890-44c7-9b2f-f873ff50ba30" />

</details>


> [!TIP]  
> Ton master est maintenant **préparé, recensé et prêt à être capturé !**


---

<a id="capture-image"></a>
# ` 📦 `︲Capture de l’image master.

---

> [!NOTE]  
> Cette étape consiste à **capturer l’image Windows 11 master** depuis la machine que tu viens de préparer et recenser.  
> **L’objectif : créer un fichier d’image utilisable pour les déploiements en masse.**

---

<a id="preparation-capture"></a>
### ` 🧭 `︲Préparation avant capture.

---

1️⃣︲**Vérifier les prérequis sur la machine master**

**Avant toute capture :**

- Un logiciel choisi au choix (VSCode, WinRAR, LibreOffice, FileZilla...) doit être installé.
- Le système doit être propre (pas de fichiers perso, pas de téléchargements inutiles).
- Les mises à jour Windows doivent être terminées.
- Le nom d’hôte doit être correct (`master-s126`).
- L’EFI doit être **désactivé** dans VMWare.
- Le master doit être **éteint** avant capture.

> [!TIP]  
> Laisse la machine dans un état **exactement prêt à être cloné**, comme en production.

---

> [!IMPORTANT]  
> **Sysprep est un outil Windows qui permet de préparer une installation pour le déploiement sur plusieurs machines en supprimant les informations spécifiques au PC (comme le SID, les pilotes ou la configuration matérielle). Son intérêt principal est de créer une image système généralisée, réutilisable et adaptable à différents environnements.**

--- 

` 🎯 `**︲ Intérêt de Sysprep :**

- **Généralisation** : supprime les identifiants uniques (SID) et rend l’image indépendante du matériel.
- **Gain de temps** : permet de déployer rapidement des systèmes préconfigurés sur plusieurs postes.
- **Uniformité** : assure une configuration standardisée sur l’ensemble du parc informatique.
- **Flexibilité** : compatible avec des scénarios de déploiement automatisés via fichiers de réponses (unattend.xml).
- **Maintenance facilitée** : mise à jour d’une seule image maître au lieu de chaque poste individuellement.

` 📌 `**︲Exemple concret :**
- **Une entreprise installe Windows 11 sur une machine, ajoute Office, configure les paramètres réseau et applique les mises à jour. Avec Sysprep, elle généralise cette installation et capture une image. Cette image est ensuite déployée sur 100 ordinateurs, chacun recevant un SID unique et une configuration adaptée lors du premier démarrage.**

---

` 📐 `**︲Shéma `ASCII` récapitulatif :**
 
```
⭐︲[Machine de référence]
     │
     │ Installation + configuration
     ▼
▶️︲[Exécution Sysprep]
     │
     │ /generalize /oobe /shutdown
     ▼
🖼️︲[Capture de l’image]
     │
     │ Outil de déploiement (MDT, DISM…)
     ▼
🚀︲[Déploiement sur parc]
     │
     │ Premier démarrage → SID unique + OOBE
     ▼
✅︲[Machines prêtes et personnalisées]
```

---

` 🌐 `**︲Sources :**
- Vue d’ensemble de Sysprep ︲ Microsoft Learn [`🌐`](https://learn.microsoft.com/fr-fr/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview?view=windows-11)  
- Processus Sysprep ︲ Microsoft Learn [`🌐`](https://learn.microsoft.com/fr-fr/windows-hardware/manufacture/desktop/sysprep-process-overview?view=windows-11)  
- Utiliser Sysprep (généralisation) ︲ Microsoft Learn [`🌐`](https://learn.microsoft.com/fr-fr/windows-hardware/manufacture/desktop/sysprep--generalize--a-windows-installation?view=windows-11)  
---

Chemin et commande Sysprep (MASTER)

```
C:\Windows\System32\sysprep
```

```
sysprep /generalize /oobe /shutdown
```

<details>
  <summary>📸︲Copie d'écran Sysprep.</summary>
  
<img width="1052" height="788" alt="image" src="https://github.com/user-attachments/assets/1ab1f2ed-12a8-45c8-bc20-d813767d0d1f" />

</details>

---

2️⃣︲**Associer l’image à capturer dans FOG**

Interface Web →  
`Host Management → master-s126 → Image`

Sélectionne :  
```

S126-master

```

Puis clique sur **Update**.

<details>
  <summary>📸︲Association de l’image au master.</summary>
  
<img width="1770" height="977" alt="image" src="https://github.com/user-attachments/assets/48d0d2c8-8b56-44b5-ac05-89a1cfb98b11" />

</details>

---

3️⃣︲**Passer le host en mode capture**

Dans FOG, va dans :  
`Host Management → master-s126 → Basic Tasks`

Choisis :  
```

Capture

```

Puis clique sur :  
➡️ **“Schedule Task”**

<details>
  <summary>📸︲Planification de la tâche de capture.</summary>
  
<img width="1777" height="999" alt="image" src="https://github.com/user-attachments/assets/c36d7a2f-2254-49ce-a583-0586296c58f0" />

</details>

---

<a id="lancer-capture"></a>
### `📸`︲Lancer la capture et suivi du processus.

---

1️⃣︲**Démarrer la VM master en PXE**

- Démarre la machine master  
- Elle boote en PXE automatiquement  
- Le menu FOG détecte la tâche en attente et lance la capture

---

2️⃣︲**Déroulement de la capture**

Tu vas voir plusieurs étapes :

- Chargement du noyau FOG  
- Analyse de la ou des partitions  
- Upload des blocs du disque (`Partclone`)  
- Compression (selon ton image type)  
- Envoi vers `/images/S126-master` sur le serveur

<details>
  <summary>📸︲Écran Partclone pendant la capture.</summary>
  
<img width="541" height="378" alt="image" src="https://github.com/user-attachments/assets/520f37d6-37b9-4eed-87e7-247a168e2ce3" />

</details>

---

3️⃣︲**Vérification de la fin de capture**

La capture est terminée lorsque la VM affiche :
```

Image upload complete

```
… puis un redémarrage automatique.

Sur ton serveur Debian, dans `/images/`, tu dois voir le dossier :

`/images/S126-master/`

<details>
  <summary>📸︲Fichiers d’image sur le serveur.</summary>
  
<img width="1777" height="999" alt="image" src="https://github.com/user-attachments/assets/ca4c670c-c1ea-43ca-8f10-b8ede6971c63" />

</details>

---

4️⃣︲**Vérification dans l'interface FOG**

Interface Web → `Image Management → All Images → S126-master`

Vérifie :

- Taille de l’image  
- Date de dernière capture  
- Nombre de partitions détectées  

> [!TIP]  
> **Si la taille est trop petite (moins de ~2 Go), c’est que la capture n’a pas réussi.**

---

> [!TIP]  
> Ton image master Windows 11 est maintenant parfaitement capturée et prête à être déployée en masse.


---

<a id="deploiement-image"></a>
# ` 🚀 `︲Déploiement de l’image master.

---

> [!NOTE]  
> Dans cette partie, tu vas déployer ton image **S126-master** vers toutes les machines clientes du groupe `Salle126`.  

---

<a id="preparation-deploiement"></a>
### ` 🌐 `︲Préparation de l’environnement réseau.

---

1️⃣︲**Isoler le réseau du plot de TP**

Pour éviter toute interférence :

- Débranche le câble réseau du plot de travail  
- Assure-toi que **seul le serveur FOG** fournit DHCP  
- Passe les machines clientes et le serveur en **réseau interne** (VMWare)

---

2️⃣︲**Configurer correctement les VM clientes**

Pour chaque machine cliente :

- Désactiver **EFI**  
- Activer le boot **PXE (Network Boot)**  
- Vérifier que la carte réseau pointe vers le **même réseau interne** que le serveur FOG  
- Vérifier le groupe : les clients doivent être dans le **groupe Salle126**

> [!TIP]  
> Assure-toi que toutes les VM ont bien une MAC différente !

---

3️⃣︲**Vérifier que chaque client apparaît dans FOG**

Interface Web →  
`Host Management → All Hosts`

Les machines destinées au déploiement doivent :

- Être recensées  
- Être propres  
- Avoir le bon hostname (ex : `client126-01`, `client126-02`, etc.)  
- Être dans le groupe `Salle126`

<details>
  <summary>📸︲Liste des hosts clients.</summary>
  
<img width="1777" height="1002" alt="image" src="https://github.com/user-attachments/assets/6c57f2c4-b573-4bae-bcec-6aff09bcc1e1" />

</details>

---

<a id="lancement-deploiement"></a>
### ` ⚡ `︲Lancement du déploiement.

---

1️⃣︲**Créer la tâche de déploiement**

Dans l’interface FOG :

`Group Management → Salle126 → Basic Tasks → Deploy`

Sélectionne l’image :

```

S126-master

```

Clique sur :  
➡️ **“`Deploy`”**  
➡️ **“`Task`”**

<details>
  <summary>📸︲Deploy / Création de la tâche.</summary>
<img width="1242" height="306" alt="image" src="https://github.com/user-attachments/assets/7d501fa9-b79e-4ef6-8d90-207a9b6bc2b6" />
<img width="1222" height="688" alt="image" src="https://github.com/user-attachments/assets/1fc608a4-9403-4d94-ba5f-b71fe0248e73" />
<img width="1241" height="452" alt="image" src="https://github.com/user-attachments/assets/fd9c859b-c2e0-4c41-ab14-816fb92d4101" />
</details>

---

2️⃣︲**Démarrer toutes les machines clientes**

Lance simultanément toutes les VM clientes.

Elles devraient :

1. Obtenir une adresse IP par FOG  
2. Charger le boot PXE  
3. Détecter la tâche  
4. Se synchroniser  
5. Lancer Partclone toutes en même temps  
6. Déployer la même image en parallèle

---

3️⃣︲**Déroulement du déploiement**

Les étapes typiques :

- Attente de synchronisation “Waiting for clients…”  
- Démarrage de la session  
- Récupération des blocs depuis le serveur  
- Décompression et écriture sur le disque  
- Finalisation et reboot automatique

<details>
  <summary>📸︲Déploiement en cours (Partclone).</summary>
<img width="1618" height="938" alt="image" src="https://github.com/user-attachments/assets/71fb3e35-a28d-475e-80e4-d8be2f7eec18" />
</details>

---

4️⃣︲**Fin du déploiement**

Chaque client affichera :  

```

Image deployed successfully

```

Puis redémarrera automatiquement !

---

<a id="verification-deploiement"></a>
### ` ✅ `︲Vérifications post-déploiement.

---

1️⃣︲**Premier démarrage Windows**

À la fin du déploiement, laisse chaque VM démarrer normalement.

2️⃣︲**Vérifier que le logiciel installé sur la machine master est présent**

Exemple :  
- ` 👨‍💻 `︲**VSCode.**  
- ` 📦 `︲**WinRAR.**  
- ` 💻 `︲**LibreOffice.**  
- ` 📂 `︲**FileZilla.**

**C’est l’indicateur principal que le déploiement a fonctionné !**

---

3️⃣︲**Vérifier les informations système**

- ` ❔ `︲**Nom de la machine.**
- ` ☑️ `︲**Comportement général.**
- ` ❌ `︲**Absence d’erreurs Windows.** 
- ` ⚡ `︲**Boot rapide et propre.**
- ` 🌐 `︲**Connexions réseau fonctionnelles.**

---

4️⃣︲**Vérifier dans l’interface FOG que la tâche est marquée comme complète**

Interface Web → `Task Management → Active Tasks`

> [!TIP]  
> Une tâche peut mettre un peu de temps à disparaître même après sa fin : FOG attend la confirmation de tous les clients !


> [!TIP]  
> À ce stade, ton serveur FOG a **réussi un déploiement**, et tu as un workflow complet de capture/déploiement fonctionnel !

---

<a id="outils-ressources"></a>
# ` 🧰 `︲Outils et ressources utilisées.

---

> [!TIP]
> Vous trouverez ici la liste de tous les outils, ressources et services utilisés pour la création de cette documentation.
> Les liens correspondants sont accessibles en cliquant sur l’emoji  : `  🌐  ` .

---

* `🗃️ ︲ 🌐` **︲Documents/Liens d’annexes fournis dans le TP :**
  * ` 📂 ` ︲`1.2 - Installer et configurer un service de déploiement FOG - TP`.`pdf`
  
  * ` 🌐 ` ︲`Idum.fr`︲[`🌐`](https://idum.fr/spip.php?article301)
  
  * ` 🌐 ` ︲`Installation de FOG 1.4.4 sur Debian 9`︲[`🌐`](https://idum.fr/spip.php?article301](https://memo-linux.com/installation-de-fog-1-4-4-sur-debian-9/))
  
  * ` 🌐 ` ︲`www.ceos3c.com`︲[`🌐`](https://www.ceos3c.com/linux/)
  
--- 

* ` 🤖 ` **︲`GPT 5.1`** ︲  [`🌐`](https://chatgpt.com/)
  
* ` ❓ ` **︲`Markdownguide.org`**   ︲[`🌐`](https://www.markdownguide.org/)
  
* ` 🤖 ` **︲`NoteBookLM`**   ︲[`🌐`](https://notebooklm.google.com/)
  
* ` ✂️ ` **︲`Screenpresso`**   ︲[`🌐`](https://www.screenpresso.com/fr/)
  
* ` 😀 ` **︲`Smiley.cool`**   ︲[`🌐`](https://smiley.cool/emoji-list.php)
  
* ` ❓ ` **︲`Syntaxe de base pour l’écriture et la mise en forme`**  ︲ [`🌐`](https://docs.github.com/fr/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)

--- 

> * ` ⏺️ `︲Nagi Player︲ [`🌐`](https://github.com/Anthonyy232/Nagi)
> 
> * ` 🎶 `︲No More Tears to Cry - Bullet for My Valentine ︲ [`🌐`](https://youtu.be/awX198gz4xk?si=Hbsem9RVtT40v1vd)
> 
> * ` ☕ `︲De la patience.

---

<a id="technique-methode"></a>
# ` 📚 `︲Technique et méthode.

---

> [!NOTE]
> J’ai noté ici tous les ajustements et optimisations que j’ai appliqués par rapport à [la première documentation que j’avais faite.](https://github.com/Trq-Builds/Doc-Technique-Administrer-Un-Controleur-De-Domaine)

--- 

 `1️⃣`︲**Structure générale : x10 plus claire.**

**Avant :**

* Découpage un peu approximatif...
* Sections mélangées (objectifs, étapes, notes, captures)...
* Manque de hiérarchie visuelle...

**Maintenant :**

* Un découpage propre, logique, séquencé.
* Une architecture cohérente du début à la fin.
* Titres uniformisés, ancres, transitions propres.
* Un vrai sommaire professionnel.

`➡️`︲**Lisibilité améliorée.**

---

 `2️⃣`︲**Utilisation intelligente et constante des blocs “NOTE / TIP / WARNING”.**

**Avant :**

* Très peu ou mal utilisés...
* Parfois trop denses ou mal espacés...

**Maintenant :**

* Blocs parfaitement intégrés.
* Utilisés pour *expliquer*, *clarifier*, *avertir*.
* Respect du rôle précis de chaque type.

`➡️`︲**Pédagogie.**

---

 `3️⃣`︲**Sectionnement narratif propre (flow continu)**

**Avant :**

* Changements de sujet parfois brutaux...
* Connexions entre étapes peu explicites...

**Maintenant :**

* Chaque section **introduit** la suivante.
* Le cheminement est fluide et logique.
* Aucun “saut” ou flou.

`➡️`︲**Cohérence.**

---

 `4️⃣`︲**Intégration PRO des captures (via `<details>`).**

**Avant :**

* Captures collées dans le texte...
* Peu de contextualisation...
* Mauvaise lisibilité dans les longs blocs...

**Maintenant :**

* Captures rangées dans des `<details>`.
* Titres explicites.
* Texte lisible sans pollution visuelle.
* L’utilisateur décide quand ouvrir.

`➡️`︲**UX documentaire.**

---

 `5️⃣`︲**Style visuel maîtrisé (emojis, uniformité, mise en forme).**

**Avant :**

* Emojis un peu aléatoires...
* Style plus scolaire que pro...
* Manque d’identité visuelle...

**Maintenant :**

* Identité graphique uniforme.
* Emojis structurants, pas décoratifs.
* Mise en forme stable (titres, listes, sections).

`➡️`︲**Esthétique.**

---

 `6️⃣`︲**Niveau technique clair, plus mature, mieux vulgarisé.**

**Avant :**

* Explications parfois courtes...
* Quelques zones floues...
* Manque d’approfondissement sur certains concepts...

**Maintenant :**

* J'explique *pourquoi* je fais les choses.
* Contexte réseau (DHCP / PXE / NAT / Host-only) mieux détaillé.
* FOG expliqué comme un vrai service d’infra, pas juste un script.
* Ajustements réseau parfaitement formulés.

`➡️`︲**Maîtrise technique.**

---

 `7️⃣`︲**Qualité d’écriture + confiance dans le ton.**

**Avant :**

* Style parfois hésitant...
* Ton un peu scolaire / neutre...

**Maintenant :**

* Style affirmé.
* Ton clair, posé, professionnel.
* Écriture propre, stable, lisible.
* Beaucoup plus fluide.

`➡️`︲**Rédaction.**

---

 `8️⃣`︲**Organisation des ressources & transparence.**

**Avant :**

* Aucune section dédiée...
* Ressources éparpillées...

**Maintenant :**

* Section “Outils et ressources” complète, claire, assumée.
* **Transparence totale sur mes moyens de production.**
* Sources externes correctement créditées.
* Outils techniques + outils de création.

`➡️`︲**Qualité documentaire.**

---
*En somme c'est* : 

* **Plus Clair.**
* **Plus Structuré.**
* **Plus Professionnel.**
* **Plus Technique.**
* **Plus Lisible.**
* **Plus Propre.**

---

> - *Être méticuleux jusque dans les moindres détails, c’est le secret.*
> - *Crossin’ your t’s, dottin’ your i’s. That’s the way to be.*

---
