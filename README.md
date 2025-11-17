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
  <summary><strong>📸︲Capture d'écran (Config VMWare).</strong></summary>
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

> [!WARNING]
> **Une fois l’installation web terminée, crée un snapshot de ta VM afin de pouvoir revenir à cet état avant les prochaines configurations.**

<details>
  <summary>📸︲Panel</summary>
<img width="1787" height="1057" alt="image" src="https://github.com/user-attachments/assets/49bf6987-7a78-4eb2-ae31-30417bc12f55" />
</details>

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
  
<details>
  <summary>📸︲Copie d'écran< (Login)</summary>
  <img width="1783" height="982" alt="image" src="https://github.com/user-attachments/assets/a9b7d3db-2983-4546-863d-8da59ebbef29" />
</details>

<details>
  <summary>📸︲Tableau de bord FOG</summary>
<img width="1787" height="1057" alt="image" src="https://github.com/user-attachments/assets/49bf6987-7a78-4eb2-ae31-30417bc12f55" />
</details>

---

2️⃣︲**Vérifier le service DHCP.**

FOG gère son propre service DHCP.
Vérifie qu’il est actif :

```bash
$ systemctl status isc-dhcp-server
```

> [!TIP]
> Passe ton **réseau en “interne”** pour éviter les conflits avec le DHCP !

---

︲**Tester le service TFTP.**

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
> Tu peux maintenant passer à la section suivante pour affiner les paramètres (menu PXE, délais, nombre de clients simultanés, etc.).

---

<a id="configuration-service-fog"></a>
# `⚙️`︲Configuration du service FOG.

---

> [!NOTE]  
> Cette partie te guide dans la **configuration avancée du service FOG**, afin d’adapter son comportement à ton environnement réseau de TP.  
> Objectif : ajuster le DHCP, modifier le délai du menu PXE et optimiser le nombre de connexions simultanées.

---

<a id="parametres-reseau"></a>
### `🌐`︲Paramètres réseau et DHCP

---

1️⃣︲**Isoler le réseau du serveur FOG**

Dans VMWare :  
- Passe la carte réseau du **serveur FOG** en mode **“Réseau interne”**.  
- Fais de même pour les **machines clientes** à déployer.
  
<details>
  <summary>📸︲Config VMWare</summary>
    <img width="1197" height="933" alt="image" src="https://github.com/user-attachments/assets/0dd3b069-8b9e-4e01-accc-16ebde632b9c" />
</details>


> [!TIP]  
> Cela permet d’éviter les conflits DHCP avec le réseau de la salle et de s’assurer que **seul FOG attribue les adresses IP**.

---

2️⃣︲**Vérifier la configuration DHCP de FOG**

FOG peut faire office de serveur DHCP.  
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
> Ne modifie pas le fichier si tu n’es pas sûr de ton plan d’adressage.
> Une mauvaise configuration DHCP peut empêcher tout boot PXE.

<details>
  <summary>📸︲Exemple de configuration DHCP</summary>
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
  <summary>📸︲Redémarrage des services</summary>
<img width="1085" height="701" alt="image" src="https://github.com/user-attachments/assets/94c235dd-1aaf-476e-bbbe-ab9b13cb8edd" />
</details>

---

> [!TIP]
> À ce stade, ton réseau interne FOG est isolé et opérationnel.
> Les clients peuvent désormais recevoir une adresse IP depuis le serveur FOG au boot PXE !

---

<a id="modification-pxe"></a>
### `⏱️`︲Modification du menu PXE et du timeout

---

> [!NOTE]
> Par défaut, le menu PXE du serveur FOG s’affiche 3 secondes.
> On va augmenter ce délai pour faciliter les manipulations et diagnostics.

---

1️⃣︲**Accéder à l’interface web FOG**

`Dashboard → FOG Configuration → PXE Boot Menu`

<details>
<summary>📸︲Copie d"écran</summary>
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
  <summary>📸︲Affichage du menu PXE</summary>
</details>

---

> [!TIP]
> Augmenter le délai du menu PXE évite de rater l’entrée en mode “Quick Registration” ou “Deploy Image” lors des tests en TP.

---

<a id="clients-simultanes"></a>
### `👥`︲Augmentation du nombre de clients simultanés

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

La modification du nombre de connexions simultanées dans FOG se fait via l’interface Web d’administration.

### Modifier le nombre de clients (Max Clients)

1. Ouvrir **Storage Management**.
2. Sélectionner **DefaultMember** (stockage créé par défaut).
3. Ajuster la valeur **Max Clients** (limite par défaut : 10).

Ce paramétrage permet d’adapter FOG aux besoins, par exemple pour couvrir plusieurs salles de TP de 18 postes lors d’un déploiement.

### Modifier le délai d’affichage du menu PXE

1. Aller dans **FOG Configuration**.
2. Ouvrir **PXE Boot Menu**.
3. Modifier **Menu Timeout (in seconds)** puis valider avec **Save PXE MENU**.

---

> [!TIP]
> Pour un test en salle complète (18 à 20 machines), 30 connexions assurent un flux stable.

<details>
  <summary>📸︲Paramètre modifié</summary>
<img width="1777" height="999" alt="image" src="https://github.com/user-attachments/assets/6188d04a-2b91-468c-af9b-24393eeccdf4" />
</details>

---

3️⃣︲**Redémarrer le service TFTP pour appliquer la modification**

```bash
sudo systemctl restart tftpd-hpa
```

---

4️⃣︲**Vérification finale**

Teste un boot PXE simultané sur plusieurs machines virtuelles.
Toutes doivent atteindre l’écran de menu FOG sans erreur réseau.

> [!TIP]
> Si certains clients restent bloqués, augmente légèrement la limite (jusqu’à 50 connexions selon la puissance de ta machine hôte).

---

> [!TIP]
> Ton serveur FOG est maintenant **configuré, optimisé et prêt à gérer plusieurs déploiements en parallèle**.

---

<a id="creation-image-groupe"></a>
## `🖼️`︲Création d’une image et d’un groupe de déploiement

---

> [!NOTE]  
> Cette partie consiste à préparer l’infrastructure FOG pour accueillir et gérer la future image master Windows 11 de la salle 126.  
> Objectif : créer une **image FOG** et un **groupe de déploiement** pour faciliter les opérations à grande échelle.

---

<a id="creation-image"></a>
### `📷`︲Création d’une image “S126-master”

---

1️⃣︲**Accéder au menu Images depuis l’interface FOG**

Interface Web → `Image Management` → `Create New Image`

---

2️⃣︲**Paramétrer l’image**

Renseigne les champs suivants :

| Paramètre | Valeur |
|----------|--------|
| **Image Name** | `S126-master` |
| **Image Description** | *Image master Windows 11 – Salle 126* |
| **Storage Group** | `default` |
| **Image Type** | `Single Disk - Resizable` |
| **OS** | `Windows 10/11` |
| **Image Path** | `/images/S126-master` |

> [!TIP]  
> 💡 Le type *Resizable* permet à FOG d’adapter la partition lors du déploiement, idéal pour les environnements homogènes.

<details>
  <summary>📸︲Création de l’image dans FOG</summary>

  *(Insère ici la capture de la page “Create New Image”)*

</details>

---

3️⃣︲**Sauvegarder l’image**

Clique sur **“Add”** pour valider la création.

Tu verras apparaître ton image dans :
`Image Management → All Images`

<details>
  <summary>📸︲Image enregistrée</summary>

  *(Capture de la liste des images avec “S126-master” dans la liste)*

</details>

---

<a id="creation-groupe"></a>
### `👥`︲Création d’un groupe de déploiement

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
  <summary>📸︲Création du groupe Salle126</summary>

  *(Capture de la page de création du groupe)*

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
> 🎯 Cette étape est cruciale : elle permet de déployer automatiquement l’image sur **toutes les machines recensées dans le groupe** sans reconfigurer chaque client.

<details>
  <summary>📸︲Assignation de l’image au groupe</summary>

  *(Capture du groupe Salle126 avec l’image assignée)*

</details>

---

> [!TIP]  
> Tu as maintenant un **groupe de déploiement opérationnel**, prêt à recevoir des clients et à lancer un déploiement massif.  
> Prochaine étape :  
> `💻︲Préparation et recensement de la machine master`.







