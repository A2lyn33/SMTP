# 📧 Tutoriel : Mise en place d'un serveur SMTP et intégration avec Active Directory (AD)

## 📝 Table des matières

1. [Introduction](#introduction)
2. [Prérequis](#prérequis)
3. [Installation d'un serveur SMTP](#installation-dun-serveur-smtp)
   - [Sous Linux](#sous-linux)
   - [Sous Windows](#sous-windows)
4. [Configuration du serveur SMTP](#configuration-du-serveur-smtp)
5. [Ajout du serveur SMTP à un domaine Active Directory](#ajout-du-serveur-smtp-à-un-domaine-active-directory)
6. [Tests et vérifications](#tests-et-vérifications)

---

## 📚 Introduction

Ce tutoriel vous guide pour :
- Installer un serveur SMTP sur Linux ou Windows.
- Le configurer pour envoyer et recevoir des e-mails.
- L'intégrer à un domaine Active Directory.

---

## ⚙️ Prérequis

- **Accès administrateur** sur la machine où le serveur SMTP sera installé.
- Un **nom de domaine** (exemple : `example.com`).
- Un serveur Active Directory fonctionnel.
- Une connaissance de base des outils CLI ou GUI selon votre environnement.

---

## 📡 Installation d'un serveur SMTP

### 🐧 Sous Linux

1. **Installer Postfix** (serveur SMTP open source) :

   ```bash
   sudo apt update
   sudo apt install postfix -y
   ```

2. **Configurer Postfix** :
   - Pendant l'installation, sélectionnez `Site Internet`.
   - Entrez votre nom de domaine lorsqu'il est demandé (par exemple : `example.com`).

3. **Vérifier l'installation** :

   ```bash
   systemctl status postfix
   ```

### 🪟 Sous Windows

1. **Installer les fonctionnalités SMTP Server** :
   - Accédez à `Gestionnaire de serveur` > `Ajouter des rôles et des fonctionnalités`.
   - Sélectionnez **Serveur SMTP**.

2. **Configurer le service SMTP** :
   - Ouvrez le `Gestionnaire IIS`.
   - Configurez les options SMTP dans la section **Delivery**.

3. **Démarrer le service SMTP** :

   ```powershell
   Start-Service SMTPSVC
   ```

---

## ⚙️ Configuration du serveur SMTP

### 🛠️ Configurer Postfix sur Linux

1. **Modifier le fichier de configuration** :

   ```bash
   sudo nano /etc/postfix/main.cf
   ```

2. **Paramètres clés** :

   ```text
   myhostname = mail.example.com
   mydomain = example.com
   myorigin = $mydomain
   inet_interfaces = all
   mydestination = $myhostname, localhost.$mydomain, localhost, $mydomain
   relayhost =
   home_mailbox = Maildir/
   ```

3. **Redémarrer Postfix** :

   ```bash
   sudo systemctl restart postfix
   ```

### ⚙️ Configurer le serveur SMTP sous Windows

1. **Configurer les domaines acceptés** :
   - Accédez aux paramètres SMTP dans le Gestionnaire IIS.
   - Ajoutez le domaine dans les **propriétés de messagerie**.

2. **Configurer les restrictions d'accès** :
   - Limitez les connexions aux adresses IP de votre réseau local.

3. **Tester la configuration** :

   ```powershell
   Test-SmtpServer -Server "mail.example.com"
   ```

---

## 🔗 Ajout du serveur SMTP à un domaine Active Directory

1. **Créer un utilisateur dédié pour le SMTP** dans Active Directory :
   - Ouvrez `Utilisateurs et ordinateurs Active Directory`.
   - Créez un utilisateur (par exemple : `smtp-user`).

2. **Configurer l'authentification SMTP** :
   - Assurez-vous que votre serveur SMTP utilise l'utilisateur créé pour authentifier les connexions.

3. **Mettre à jour les enregistrements DNS** :
   - Ajoutez un enregistrement `MX` pointant vers votre serveur SMTP.
   - Exemple :

     ```text
     Type: MX
     Nom: @
     Priorité: 10
     Valeur: mail.example.com
     ```

4. **Configurer les autorisations** :
   - Dans la console SMTP (Windows) ou Postfix (Linux), assurez-vous que l'utilisateur AD a les autorisations nécessaires pour envoyer des e-mails.

---

## ✅ Tests et vérifications

1. **Tester l'envoi d'un e-mail** :
   - Sous Linux :

     ```bash
     echo "Test SMTP" | mail -s "Sujet" user@example.com
     ```
   - Sous Windows :

     Utilisez un client SMTP comme Outlook ou Thunderbird.

2. **Vérifier les logs** :
   - Linux : `/var/log/mail.log`
   - Windows : Journaux d'événements > Applications et Services.

3. **Valider la connectivité DNS** :

   ```bash
   nslookup -type=MX example.com
   ```

---

🎉 **Félicitations !** Vous avez mis en place et intégré un serveur SMTP à votre domaine Active Directory.
