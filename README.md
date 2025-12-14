# TP : Pipeline CI/CD avec Jenkins, Spring Boot et Docker

## Description du projet

Application Spring Boot simple pour démontrer un pipeline CI/CD complet avec Jenkins, Maven, Docker et GitHub.

## Prérequis

- Windows 10 / 11
- Java JDK 17 ou 21 installé
- Git installé
- Docker Desktop installé et démarré
- Compte GitHub
- Accès Internet
- Cloudflare Tunnel (exposition Internet)

## Structure du projet

```
tp30/
├── src/
│   ├── main/
│   │   ├── java/com/example/pos/
│   │   │   ├── PosApplication.java
│   │   │   └── controller/
│   │   │       └── HealthController.java
│   │   └── resources/
│   │       └── application.properties
│   └── test/
│       └── java/com/example/pos/
│           └── PosApplicationTests.java
├── pom.xml
├── Dockerfile
├── Jenkinsfile
└── README.md
```

## Installation et configuration

### Partie 1 : Installation de Jenkins

1. Télécharger Jenkins (Windows installer) depuis https://www.jenkins.io/download/
2. Lancer l'installation et suivre l'assistant
3. Ouvrir Jenkins dans le navigateur : `http://localhost:8080`
4. Récupérer le mot de passe initial depuis :
   ```
   C:\Program Files\Jenkins\secrets\initialAdminPassword
   ```
5. Installer les plugins recommandés et créer un utilisateur admin

### Partie 2 : Configuration des outils Jenkins

1. Aller dans **Manage Jenkins** → **Tools**
2. Descendre jusqu'à **Maven installations**
3. Cliquer sur **Add Maven**
4. Nom : `maven`
5. Cocher **Install automatically**
6. Choisir une version (ex : 3.9.6)
7. Enregistrer

### Partie 3 : Test local du projet

1. Compiler le projet :
   ```bash
   mvn clean install
   ```

2. Lancer l'application :
   ```bash
   mvn spring-boot:run
   ```

3. Tester l'API :
   - Health check : http://localhost:8282/api/health
   - Hello : http://localhost:8282/api/hello

### Partie 4 : Publication sur GitHub

Le projet est configuré pour utiliser le dépôt GitHub :
- **URL** : `https://github.com/jdabachine3378-svg/jenkins-demo.git`
- **Branche** : `master`

Si vous souhaitez utiliser votre propre dépôt :

1. Créer un dépôt GitHub personnel (ex : `first_test_jenkins`)

2. Initialiser Git et pousser le code :
   ```bash
   git init
   git add .
   git commit -m "Initial commit"
   git branch -M master
   git remote add origin https://github.com/USERNAME/first_test_jenkins.git
   git push -u origin master
   ```

   ⚠️ **Important** : Si vous changez de dépôt, mettre à jour l'URL dans le `Jenkinsfile`.

### Partie 5 : Création du job Pipeline Jenkins

1. Dans Jenkins, cliquer sur **New Item**
2. Nom : `my_project`
3. Type : **Pipeline**
4. Valider

5. Configuration :
   - Cocher **GitHub project** et renseigner l'URL : `https://github.com/jdabachine3378-svg/jenkins-demo.git`
   - Dans **Build Triggers**, cocher : **GitHub hook trigger for GITScm polling**
   - Dans **Pipeline**, sélectionner **Pipeline script** et coller le contenu du `Jenkinsfile`
   - Le `Jenkinsfile` est déjà configuré avec la branche `master` et l'URL du dépôt

6. Enregistrer le job

7. Tester manuellement : Cliquer sur **Build Now**

### Partie 6 : Configuration de ngrok et Webhook GitHub

1. **Installation de ngrok** :
   - Télécharger ngrok depuis https://ngrok.com/download
   - Ajouter le token :
     ```bash
     ngrok config add-authtoken VOTRE_TOKEN
     ```

2. **Exposer Jenkins** :
   ```bash
   ngrok http 8080
   ```
   - Copier l'URL HTTPS fournie (ex : `https://xxxx.ngrok-free.app`)

3. **Configuration du Webhook GitHub** :
   - Aller dans votre dépôt GitHub → **Settings** → **Webhooks** → **Add webhook**
   - **Payload URL** : `https://xxxx.ngrok-free.app/github-webhook/`
   - **Content type** : `application/json`
   - **Event** : `Push`
   - Ajouter le webhook

4. **Test du déclenchement automatique** :
   ```bash
   git add .
   git commit -m "test webhook"
   git push
   ```
   - Vérifier que Jenkins déclenche automatiquement le pipeline

## Endpoints de l'application

Une fois l'application déployée via Docker, elle sera accessible sur le port 8585 :

- Health check : http://localhost:8585/api/health
- Hello : http://localhost:8585/api/hello

## Pipeline CI/CD

Le pipeline Jenkins exécute les étapes suivantes :

1. **Git Clone** : Récupération du code depuis GitHub
2. **Build** : Compilation et tests avec Maven
3. **Create Docker Image** : Création de l'image Docker
4. **Run** : Lancement du conteneur Docker

## Notes importantes

- ⚠️ Jenkins doit pouvoir exécuter Docker. Si Jenkins tourne comme service Windows, vérifier les droits d'accès.
- ⚠️ Remplacer `USERNAME` dans le Jenkinsfile par votre nom d'utilisateur GitHub
- ⚠️ Remplacer `myuser` dans le Dockerfile/Jenkinsfile par votre nom d'utilisateur Docker Hub (optionnel)
## capteur d'ecron de tp :
<img width="1919" height="971" alt="image" src="https://github.com/user-attachments/assets/2309fa44-92eb-402e-a406-c758f166fd92" />
<img width="1916" height="969" alt="image" src="https://github.com/user-attachments/assets/0f2f2c22-ecbd-48f7-921d-5ae34b5ee163" />
<img width="1911" height="957" alt="image" src="https://github.com/user-attachments/assets/331d51cd-d2d9-479b-b9dd-0dba6914a85b" />
<img width="1755" height="1242" alt="image" src="https://github.com/user-attachments/assets/04a0519a-3e42-4112-a24f-a7722cf74ff1" />






## Auteur
jamila dabachine 
TP réalisé dans le cadre de l'apprentissage du CI/CD avec Jenkins.

