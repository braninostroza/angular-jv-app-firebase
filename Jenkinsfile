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
                    git url: 'https://github.com/braninostroza/angular-jv-app-firebase.git', branch: 'main'
                    echo '[SUCCESS] - El repositorio ha sido clonado correctamente'
                }
            }
        }
        
        stage('Install Dependencies') {
            steps {
                script {
                    bat 'npm ci'
                    bat 'npm install -g @angular/cli'  // Instala Angular CLI globalmente
                    echo '[SUCCESS] - Las dependencias se han instalado correctamente'
                }
            }
        }
        
        stage('Build Angular') {
            steps {
                script {
                    bat 'npx ng build --configuration production'
                }
            }
        }
        
        stage('Deploy to Firebase') {
            steps {
                script {
                    bat 'firebase deploy --token $FIREBASE_TOKEN'
                    echo '[SUCCESS] - Despliegue a Firebase realizado con éxito'
                }
            }
        }
    }
    
    post {
        always {
            script {
                if (currentBuild.currentResult == 'SUCCESS') {
                    echo 'El pipeline se completó correctamente'
                } else {
                    echo '[FAILURE] - El pipeline falló. Revisa los logs para más detalles.'
                }
            }
        }
    }
}
