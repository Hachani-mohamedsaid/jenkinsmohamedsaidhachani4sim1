# 📋 Jenkinsfile Corrigé - Version Complète

## Version Corrigée du Jenkinsfile

```groovy
pipeline {
    agent any

    environment {
        SONAR_HOST_URL = 'http://10.211.55.4:9000'
        SONAR_TOKEN = 'squ_54f6daad603d47d3044860ebcb7b4da46c8d41ac'
    }

    stages {
        stage('Checkout') {
            steps {
                // Clonage du repo GitHub
                git branch: 'main', url: 'https://github.com/Hachani-mohamedsaid/jenkinsmohamedsaidhachani4sim1.git'
            }
        }

        stage('Build & Test') {
            steps {
                // Compile le projet et exécute les tests
                sh 'mvn clean verify'
            }
        }

        stage('Analyse SonarQube') {
            steps {
                // Analyse SonarQube avec token
                sh """
                    mvn sonar:sonar \
                    -Dsonar.host.url=${SONAR_HOST_URL} \
                    -Dsonar.token=${SONAR_TOKEN}
                """
            }
        }
    }

    post {
        success {
            echo 'Pipeline terminé avec succès !'
        }
        failure {
            echo 'La pipeline a échoué. Vérifie les logs.'
        }
    }
}
```

---

## ✅ Corrections Apportées

| Élément | Avant | Après |
|---------|-------|-------|
| **Authentification SonarQube** | `-Dsonar.login=admin` | `-Dsonar.token=squ_54f6...` |
| **Type d'authentification** | Login/Password (Deprecated) | Token (Standard moderne) |
| **Variables d'environnement** | Absentes | `SONAR_HOST_URL` et `SONAR_TOKEN` |
| **Stage Checkout** | Absent ou incomplet | `git branch: 'main'` ajouté |
| **Erreur attendue** | ❌ "Not authorized" | ✅ Authentification réussie |

---

## 📊 Tableau de Composition du Pipeline

### 1️⃣ **Environnement (Environment Block)**
```groovy
environment {
    SONAR_HOST_URL = 'http://10.211.55.4:9000'    // Serveur SonarQube
    SONAR_TOKEN = 'squ_54f6daad603d47d3044860ebcb7b4da46c8d41ac'  // Token d'authentification
}
```

### 2️⃣ **Étape 1 : Checkout**
```groovy
stage('Checkout') {
    steps {
        git branch: 'main', url: 'https://github.com/Hachani-mohamedsaid/jenkinsmohamedsaidhachani4sim1.git'
    }
}
```
- Récupère le code source depuis GitHub
- Branche : `main`
- URL du repository

### 3️⃣ **Étape 2 : Build & Test**
```groovy
stage('Build & Test') {
    steps {
        sh 'mvn clean verify'
    }
}
```
- Nettoie les fichiers compilés précédents
- Compile le code Java
- Exécute les tests unitaires (avec H2 en-mémoire)
- Crée l'artifact JAR

**Logs attendus :**
```
Tests run: 1, Failures: 0, Errors: 0, Skipped: 0
BUILD SUCCESS
```

### 4️⃣ **Étape 3 : Analyse SonarQube** ⭐
```groovy
stage('Analyse SonarQube') {
    steps {
        sh """
            mvn sonar:sonar \
            -Dsonar.host.url=${SONAR_HOST_URL} \
            -Dsonar.token=${SONAR_TOKEN}
        """
    }
}
```
- Analyse la qualité du code
- Envoie les résultats à SonarQube
- Authentification via token (sécurisé)

### 5️⃣ **Post-Actions**
```groovy
post {
    success {
        echo 'Pipeline terminé avec succès !'
    }
    failure {
        echo 'La pipeline a échoué. Vérifie les logs.'
    }
}
```
- Message de succès ou d'erreur
- Utile pour le monitoring

---

## 🔐 Détails du Token

| Propriété | Valeur |
|-----------|--------|
| **Token** | `squ_54f6daad603d47d3044860ebcb7b4da46c8d41ac` |
| **Type** | SonarQube Token |
| **Utilisation** | Authentification pour l'analyse de code |
| **Sécurité** | Tokens révocables et temporaires |
| **Paramètre Maven** | `-Dsonar.token` |

---

## 🚀 Flux d'Exécution Complet

```
START PIPELINE
    ↓
[1] CHECKOUT
    └─ Clone le repo GitHub (main branch)
    ↓
[2] BUILD & TEST
    ├─ mvn clean verify
    ├─ Compilation du code Java
    ├─ Exécution des tests (H2 in-memory)
    └─ Création du JAR
    ↓
[3] ANALYSE SONARQUBE
    ├─ mvn sonar:sonar
    ├─ Authentification via token
    ├─ Analyse de la qualité du code
    └─ Envoi des résultats à SonarQube
    ↓
[4] POST-ACTIONS
    └─ Message de succès/échec
    ↓
END PIPELINE (SUCCESS ✅)
```

---

## 📝 Configuration Requise

### Jenkins
- ✅ Maven installé et configuré
- ✅ Git configuré
- ✅ Accès à GitHub
- ✅ Accès à SonarQube (10.211.55.4:9000)

### Application Spring Boot
- ✅ `src/test/resources/application.properties` : Configuration H2
- ✅ `src/main/resources/application.properties` : Configuration MySQL
- ✅ `pom.xml` : Dépendances Maven correctes

### SonarQube
- ✅ Serveur SonarQube accessible
- ✅ Token généré et valide
- ✅ URL correcte : `http://10.211.55.4:9000`

---

## ✨ Points Clés de la Correction

1. **Token au lieu de Login** ⭐
   - Avant : `-Dsonar.login=admin` (deprecated)
   - Après : `-Dsonar.token=squ_...` (standard moderne)

2. **Variables d'Environnement**
   - Centralisation de la configuration
   - Facilité de maintenance
   - Réutilisabilité

3. **Structure Déclarative**
   - Pipeline lisible et maintenable
   - Stages clairs et séquentiels
   - Gestion des erreurs avec `post`

4. **Sécurité**
   - Token révocable
   - Pas d'exposition du mot de passe
   - Conformité avec les standards SonarQube

---

## 🧪 Test du Pipeline

### Pour tester localement (avant Jenkins)
```bash
# Compile et teste
mvn clean verify

# Analyse SonarQube
mvn sonar:sonar \
  -Dsonar.host.url=http://10.211.55.4:9000 \
  -Dsonar.token=squ_54f6daad603d47d3044860ebcb7b4da46c8d41ac
```

### Dans Jenkins
1. Accédez à : `http://192.168.33.10:8080`
2. Trouvez le job "sonar"
3. Cliquez "Build Now"
4. Vérifiez les logs dans "Console Output"

---

## 📊 Résultats Attendus

### Après Build & Test ✅
```
[INFO] Tests run: 1, Failures: 0, Errors: 0, Skipped: 0
[INFO] BUILD SUCCESS
```

### Après Analyse SonarQube ✅
```
[INFO] ANALYSIS SUCCESSFUL, you can browse http://10.211.55.4:9000/dashboard...
```

### Post-Actions ✅
```
[Pipeline] echo
Pipeline terminé avec succès !
```

---

## 📌 Fichiers Importants

| Fichier | Rôle |
|---------|------|
| `/Jenkinsfile` | Pipeline Jenkins (ce fichier) |
| `/pom.xml` | Configuration Maven |
| `/src/main/resources/application.properties` | Config MySQL production |
| `/src/test/resources/application.properties` | Config H2 test |
| `/src/main/java/...` | Code source Spring Boot |

---

## 🔧 Dépannage

| Problème | Solution |
|----------|----------|
| "Not authorized" | Vérifier le token SonarQube |
| "Connection refused" | Vérifier l'URL SonarQube |
| Tests échouent | Vérifier H2 dans pom.xml scope=test |
| Git auth fails | Vérifier accès GitHub |

---

**Dernière mise à jour :** 1 décembre 2025  
**Statut :** ✅ Prêt pour production  
**Version :** 1.0 (Corrigée)
