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
                        sh 'npm install'
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
                        // Compila el proyecto Angular
                        sh 'ng build --prod'
                    } catch (Exception e) {
                        error "Error durante la construcción del proyecto Angular: ${e.message}"
                    }
                }
            }
        }
        
        stage('Deploy to Firebase') {
            steps {
                script {
                    try {
                        // Despliega a Firebase usando la variable de entorno
                        sh 'firebase deploy --token $FIREBASE_TOKEN'
                    } catch (Exception e) {
                        error "Error durante el despliegue a Firebase: ${e.message}"
                    }
                }
            }
        }
    }
    
    post {
        success {
            echo 'Despliegue completado con éxito'
        }
        failure {
            echo 'El pipeline falló. Revisa los logs para más detalles.'
        }
    }
}
