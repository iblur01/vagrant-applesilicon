# Documentation : Utilisation de Vagrant avec UTM sur Mac Silicon (M1, M2, M3, M4) 🚀

Cette documentation explique comment configurer et utiliser **Vagrant** avec le fournisseur **UTM** pour gérer des machines virtuelles sur macOS (Apple Silicon). 

Ce README est basé sur le projet [vagrant_utm](https://github.com/naveenrajm7/vagrant_utm/tree/main), développé par **naveenrajm7**, et reflète les informations disponibles à la date de **décembre 2024**. Il fournit une documentation adaptée pour l’utilisation du plugin **vagrant_utm** en combinaison avec Vagrant et UTM sur macOS (Apple Silicon). Pour des mises à jour ou des détails supplémentaires, veuillez consulter directement le dépôt GitHub officiel. 

Ce document d’installation a été rédigé par Théo Delannoy (e4a, décembre 2024) dans le cadre de l’intégration de Vagrant et du plugin vagrant_utm avec UTM sur macOS Apple Silicon. 

---

## **Prérequis** 🛠️

Avant de commencer, il est important de s'assurer que vous maîtrisez et avez configuré correctement certains outils nécessaires au bon déroulement des étapes. Voici ce dont vous aurez besoin :

---

#### **1. Bases sur l’installation d’applications tierces sous macOS** 💡  
- Savoir installer des applications tierces depuis des fichiers `.dmg`, `.pkg` ou via des gestionnaires de paquets comme **Homebrew**.
- Exemple : Télécharger un fichier, l’ouvrir, glisser l’application dans le dossier **Applications**.

---

#### **2. Xcode et outils de développement macOS** 🛠️  
Certains outils de ligne de commande nécessitent les utilitaires de développement macOS :
- Installez les outils de ligne de commande Xcode si ce n’est pas déjà fait :
  ```bash
  xcode-select --install
  ```
- Cela installera les outils nécessaires comme `git`, `make`, et d'autres commandes.

---

#### **3. Homebrew (Gestionnaire de paquets)** 🍺  
**Homebrew** est un gestionnaire de paquets essentiel pour installer rapidement des outils et dépendances sur macOS. Si vous ne l’avez pas encore installé :
1. Exécutez cette commande dans le terminal :
   ```bash
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   ```
2. Vérifiez l'installation avec :
   ```bash
   brew --version
   ```

## **Utilitaires à installer** 🛠️

### **1. Installer UTM (versions compatibles uniquement)** ⚙️

- Téléchargez et installez UTM, version **4.5.1** ou **4.5.2** :  
  📥 [Lien de téléchargement UTM](https://github.com/utmapp/UTM/releases/tag/v4.5.2)  

  **Instructions** :  
  1. Rendez-vous dans la section **Assets** en bas de la page des releases.  
  2. Téléchargez le fichier `UTM.dmg`.  
  3. Ouvrez le fichier `.dmg` téléchargé.  
  4. Suivez l'installation classique sur macOS en glissant l'icône **UTM** dans le dossier **Applications**.  


### **2. Installer Vagrant (version obligatoire)** 🧰
- Téléchargez et installez **Vagrant 2.4.1** :  
  📥 [Lien de téléchargement Vagrant](https://releases.hashicorp.com/vagrant/2.4.1/vagrant_2.4.1_darwin_arm64.dmg)

### **3. Attention aux VMs indisponibles dans UTM** 
- **Important** : Si une machine virtuelle est **indisponible ou corrompue** dans UTM, cela peut provoquer des erreurs et faire planter UTM.
- Supprimez toute VM inaccessible avant de continuer. 🗑️

---

## **Installation du plugin UTM pour Vagrant** 🛠️

1. Installez le plugin UTM :  
   ```bash
   vagrant plugin install vagrant_utm
   ```
   ✅ Ce plugin permet d'ajouter UTM comme fournisseur pour Vagrant.

2. Vérifiez l'installation :  
   ```bash
   vagrant plugin list
   ```
   Vous devriez voir `vagrant_utm` dans la liste des plugins. 📃

---

## **Création d’un projet Vagrant** 📂

1. **Créez un dossier pour le projet :** 📁  
   ```bash
   mkdir vagrant-debian
   cd vagrant-debian
   ```

2. **Créez un fichier `Vagrantfile` :** ✏️  
   Ajoutez le contenu suivant dans un fichier nommé `Vagrantfile` :
   ```ruby
   Vagrant.configure("2") do |config|
     config.vm.provider :utm do |u|
       u.utm_file_url = "https://github.com/naveenrajm7/utm-box/releases/download/debian-11/debian_vagrant_utm.zip"
     end
   end
   ```

---

## **Démarrage de la machine virtuelle** 🚀

1. **Lancez la VM avec Vagrant :** 🖥️  
   ```bash
   vagrant up
   ```
   Pendant l’exécution, vous verrez un message similaire :  
   🛠️
   ```
   Bringing machine 'default' up with 'utm' provider...
   ==> default: Importing UTM virtual machine file ...
   default: IMPORTANT: Due to limited UTM API support, this plugin does not know when the download is finished.
   ```

2. **Aller dans UTM pour confirmer le téléchargement :** ✅  
   - Une fenêtre apparaîtra pour confirmer le téléchargement de la VM. Acceptez et suivez les instructions.  
   - **Vérifiez que la nouvelle VM est listée en dernière position dans l’interface UTM.** 📋

3. **Confirmez dans le terminal :**  
   Une fois le téléchargement terminé, tapez `y` pour continuer. 

4. **Connexion SSH à la VM :** 🔐  
   Une fois la machine démarrée, connectez-vous avec :  
   ```bash
   vagrant ssh
   ```

---

## **Attention aux autorisations macOS** 

Lors du premier lancement, macOS peut afficher des fenêtres demandant si le terminal peut **contrôler UTM**. Assurez-vous de donner les autorisations nécessaires :  
✅ Cliquez sur **Oui** ou **Autoriser** pour toutes les demandes.  

Si des erreurs persistent :  
1. Allez dans **Préférences Système > Sécurité et confidentialité > Confidentialité > Automatisation**.  
2. Donnez les permissions nécessaires au **Terminal**.

---

## **Résolution des problèmes** 🔍

### **Erreur fréquente :** ❌
```
There was an error while executing a command.
Command: ["osascript", "/Users/theo/.vagrant.d/gems/3.1.4/gems/vagrant_utm-0.0.1/lib/vagrant_utm/scripts/list_vm.js"]
Stderr: ... Error: La connexion est invalide. (-609)
```
- **Cause probable :** Une VM inaccessible ou corrompue dans UTM.  
- **Solution :**  
  1. Ouvrez UTM et supprimez toute VM qui ne fonctionne pas. 🗑️  
  2. Assurez-vous que la VM importée est bien la dernière dans la liste.

---

## **Compatibilité** 🔧

- **UTM :** Versions 4.5.1 ou 4.5.2 uniquement.  
- **Vagrant :** Version 2.4.1 obligatoire.  
- **macOS :** Compatible avec Apple Silicon (M1, M2, M3, M4).  
