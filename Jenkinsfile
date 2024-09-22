pipeline {
    agent any
    
    environment {
        NODE_ENV = 'production'
        GOOGLE_APPLICATION_CREDENTIALS = credentials('Deploy-Angular')
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
        
        stage('Build Angular') {
            steps {
                script {
                    try {
                        // Compila el proyecto Angular
                        bat 'npm run build'
                    } catch (Exception e) {
                        error "Error durante la construcción del proyecto Angular: ${e.message}"
                    }
                }
            }
        }
        
        stage('Deploy to Firebase') {
            steps {
                script {
                    bat 'firebase deploy' // Desplegar a Firebase sin usar el token
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
