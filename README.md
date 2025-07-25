Procédure Linux
# **​​​​INTÉGRATION D’UN SERVEUR LINUX DEBIAN DANS UN DOMAINE ACTIVE DIRECTORY WINDOWS SERVER**

Avant de commencer, voici les informations nécessaires pour comprendre les étapes.

Ici dans cet exemple :

Serveur maître (Windows Server)     SANG        10.0.2.247     sang.emilezola.local

Serveur esclave (Linux)             HARPON      10.0.2.248     harpon.emilezola.local

Notre objectif est d'intégrer HARPON dans l'AD de SANG.

## **Etape 1 : Installer les paquets nécessaires**

   ```bash
   sudo apt update
   sudo apt install realmd sssd sssd-tools libnss-sss adcli samba-common samba-common-bin krb5-user packagekit
   ```
**Configuration de Kerberos** :

   Lors de l'installation de krb5-user, une fenêtre bleue va s'ouvrir, il faut indiquer le nom de domaine complet : emilezola.local

   Ensuite, il faut indiquer le nom du serveur principal (Windows Server dans l'exercice du cube 2) comme nom de contrôleur de domaine lors de l'installation de     Kerberos.
   Donc ici, ce sera : SANG

## **Etape 2 : Configuration du fichier /etc/hosts**

```bash
sudo nano /etc/hosts
```
Supprime le contenu et ajoute : 
```lua
10.0.2.247    sang.emilezola.local
10.0.2.248    harpon.emilezola.local
```

2. **Configuration du fichier /etc/hosts** :







3. **Vérifier le statut du service SSH** :

   ```bash
   sudo systemctl status ssh
   ```

   Le service doit être actif et en cours d'exécution.

## **Générer une paire de clés SSH sur le client Windows**

1. **Ouvrir powerShell** :

   - Appuyez sur **Windows + X** et sélectionnez **Windows PowerShell**.

2. **Générer la paire de clés** :

   ```powershell
   ssh-keygen -t rsa -b 2048
   ```

   - Acceptez l'emplacement par défaut (`C:\Users\VotreNom\.ssh\id_rsa`).
   - Choisissez une passphrase ou appuyez sur **Entrée** pour ne pas en mettre.

## **Configurer l'authentification par clé SSH sur les serveurs**

1. **Copier la clé publique sur Server1** :

   - Si `ssh-copy-id` n'est pas disponible sur Windows, vous pouvez copier manuellement la clé publique.

   - Affichez le contenu de la clé publique :

     ```powershell
     type $env:USERPROFILE\.ssh\id_rsa.pub
     ```

   - Connectez-vous au serveur :

     ```powershell
     ssh user@192.168.200.254
     ```

   - Sur le serveur, créez le dossier `.ssh` et le fichier `authorized_keys` :

     ```bash
     mkdir -p ~/.ssh
     nano ~/.ssh/authorized_keys
     ```

   - Collez le contenu de votre clé publique dans ce fichier, enregistrez et quittez.

2. **Répéter les étapes pour Server2** (IP : `192.168.200.2`)

## **Se connecter en SSH depuis le client Windows**

- Vous pouvez maintenant vous connecter sans mot de passe :

  ```powershell
  ssh user@192.168.200.254
  ssh user@192.168.200.2
  ```

---
