pipeline {
    agent any
    
    environment {
        NODE_ENV = 'production'
        FIREBASE_TOKEN = credentials('FIREBASE_TOKEN')  // Referencia a la variable global
    }
    
    stages {
        stage('Checkout') {
            steps {
                script {
                    try {
                        // Clona el repositorio desde GitHub
                        git url: 'https://github.com/braninostroza/angular-jv-app-firebase.git', branch: 'main'
                        echo 'Repositorio clonado exitosamente'
                    } catch (Exception e) {
                        error "Error durante la clonación del repositorio: ${e.message}"
                    }
                }
            }
        }
        
        stage('Install Dependencies') {
            steps {
                script {
                    try {
                        // Instala dependencias de Node.js y Angular
                        sh 'npm ci'  // npm ci es más eficiente para CI/CD
                        echo 'Dependencias instaladas correctamente'
                    } catch (Exception e) {
                        error "Error durante la instalación de dependencias: ${e.message}"
                    }
                }
            }
        }
        
        stage('Build Angular') {
            steps {
                script {
                    try {
                        // Compila el proyecto Angular en modo producción
                        sh 'ng build --configuration production'
                        echo 'Proyecto Angular compilado con éxito'
                    } catch (Exception e) {
                        error "Error durante la construcción del proyecto Angular: ${e.message}"
                    }
               
