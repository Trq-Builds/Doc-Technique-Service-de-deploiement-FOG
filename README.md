# ` 👻 `︲Documentation TP : Installer et configurer un service de déploiement FOG.

---

Ce dépôt présente un guide complet pour l’installation, la configuration et l’utilisation du service de déploiement **FOG** (Free Open-Source Ghost) sous **Debian 13**.
Tu y apprendras à **installer le serveur**, **capturer une image master**, et **déployer celle-ci** sur plusieurs machines clientes.

---

> [!IMPORTANT]
> Les vidéos et captures d’écran seront ajoutées progressivement.
> Les vidéos sont hébergées sur `dona.one`.
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

### `⚙️`︲Configuration de la VM.

* `❓` ︲**Hostname :** `srv-fog`.
* `🪪` ︲**Adressage IP :** dynamique (DHCP) récupérer une adresse sur le réseau local physique.
* `🖼️` ︲**Interface graphique :** **__aucune__** (__installation en mode serveur / ligne de commande__).
* `🧩` ︲**Service à installer :** `ssh` (SSH activé et démarré).
* `📏` ︲**Mémoire :** 1024 Mo.
* `💾` ︲**Disque :** 100 Go (allocation dynamique).
* `❤️` ︲**Cœurs :** 1. (Optionel)

<details>
  <summary><strong>🖼️︲Captures d'écran (Config VMWare)</strong></summary>
    <img width="887" height="880" alt="image" src="https://github.com/user-attachments/assets/8671256d-3603-45e0-af58-d9cf730d5d52" />
</details>


### `🚧`︲Couples d’identifiants (__NON NÉGOCIABLE__)

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
> Cette partie couvre **l’installation complète de Debian 13 sans interface graphique**.
> Objectif : obtenir un serveur minimaliste, stable et prêt pour héberger FOG.

---

1️⃣︲**Lancement de l’installation depuis l’ISO Debian 13**

* Sélectionner **Installation** (pas “Graphical install”).
* Choisir la **langue :** `Français`
* Choisir le **pays :** `France`
* Disposition clavier : **Français (AZERTY)**

<details>
  <summary>📸︲Sélection langue et clavier</summary>
<img width="800" height="593" alt="image" src="https://github.com/user-attachments/assets/035692f5-ee65-4936-9856-041100e57798" />
</details>

---

2️⃣︲**Configuration réseau**

* Nom de la machine : `srv-fog`
* Méthode d’adressage : **DHCP (automatique)**
* Domaine : *(laisser vide ou local)*

<details>
  <summary>📸︲Identifiants</summary>

<img width="799" height="594" alt="image" src="https://github.com/user-attachments/assets/e74ab1d5-33ba-4686-b6c8-c50b86963e06" />


</details>

---

3️⃣︲**Partitionnement du disque**

* Disque virtuel : **100 Go dynamiquement alloué**
* Schéma recommandé :

  * `/` → 60 Go
  * `swap` → 2 Go
  * `/var` → reste du disque
* Type : **Guidé – utiliser tout le disque**, séparé selon les besoins.

<details>
  <summary>📸︲Partitionnement automatique</summary>

*(Tes captures ici)*

</details>

---

4️⃣︲**Sélection des paquets à installer**

* Ne **pas** installer d’environnement graphique.
* Cocher uniquement :

  * `serveur SSH`
  * `utilitaires système standard`

---

5️⃣︲**Installation du chargeur de démarrage (GRUB)**

* Installer sur le disque principal `/dev/sda`.
* Une fois l’installation terminée : **retirer l’ISO et redémarrer.**

<details>
  <summary>📸︲Fin d’installation et redémarrage</summary>

*(Tes captures ici)*

</details>

---

> [!TIP]
> 💡 *Prends un snapshot de ta VM à ce stade (avant configuration SSH).*
> Cela te permettra de revenir rapidement si la configuration réseau ou FOG plante plus tard.


  ---

  ---

  ---

  

> [!TIP]
> [🎥︲Vidéo explicative – Cliquez-ici (Dona.One)]()

> [!TIP]
> [🎥︲Vidéo explicative – Cliquez-ici (Mega.nz)]()

<details>
  <summary><strong>🖼️︲Captures d'écran</strong></summary>
</details>

---

<a id="configuration-ssh"></a>
## `🔐`︲Création des comptes et configuration SSH.



---

> [!NOTE]
> Cette section configure les **utilisateurs**, le **SSH** et la **sécurisation basique du serveur**.
> Indispensable avant de passer à l’installation de FOG.

---

1️⃣︲**Création des utilisateurs**

* Utilisateur root : `root / btssio`
* Utilisateur standard : `btssio / btssio`
* Vérifie que les deux existent avec :

  ```bash
  cat /etc/passwd | grep btssio
  ```

---

2️⃣︲**Activation du SSH**

* S’assurer que le paquet est installé :

  ```bash
  sudo apt install openssh-server -y
  ```
* Démarrer et activer le service :

  ```bash
  sudo systemctl enable ssh --now
  sudo systemctl status ssh
  ```

<details>
  <summary>📸︲Vérification du service SSH</summary>

*(Insère la capture montrant le service SSH actif)*

</details>

---

3️⃣︲**Autoriser la connexion root (optionnel)**

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
> ⚠️ **Ne laisse pas le root activé en environnement réel**.
> Ici c’est uniquement pour le TP ou les tests internes.

---

4️⃣︲**Test de connexion distante**
Depuis la machine hôte :

```bash
ssh btssio@<ip_du_serveur>
```

ou

```bash
ssh root@<ip_du_serveur>
```

<details>
  <summary>📸︲Connexion SSH réussie</summary>

*(Capture de la première connexion)*

</details>

---

> [!TIP]
> 💾 *Prends un instantané de la VM “srv-fog” après validation du SSH.*
> Tu pourras y revenir avant de lancer l’installation du service FOG.

---

---


--- 

> [!TIP]
> [🎥︲Vidéo explicative – Cliquez-ici (Dona.One)]()

> [!TIP]
> [🎥︲Vidéo explicative – Cliquez-ici (Mega.nz)]()


<details>
  <summary><strong>🖼️︲Captures d'écran</strong></summary>
</details>

---

<a id="installation-fog"></a>
# `🌍`︲Installation du service FOG.

---

<a id="telechargement-fog"></a>
### `⬇️`︲Téléchargement et préparation de l’archive.

---

<a id="installation-serveur-fog"></a>
### `🧩`︲Procédure d’installation du serveur FOG.


---

<a id="configuration-initiale-fog"></a>
### `🔧`︲Configuration initiale et tests.

---

<a id="configuration-service-fog"></a>
# `⚙️`︲Configuration du service FOG.












