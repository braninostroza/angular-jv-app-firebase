pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                // Clona el repositorio desde GitHub
                git url: 'https://github.com/Benjamin1106/Proyeco-Portafolio.git', branch: 'main'
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    try {
                        // Instala dependencias de Node.js y Angular
                        sh 'npm install'
                    } catch (Exception e) {
                        error "Error durante la instalación de dependencias"
                    }
                }
            }
        }

        stage('Build Angular') {
            steps {
                script {
                    try {
                        // Compila el proyecto Angular
                        sh 'npm run build --prod'
                    } catch (Exception e) {
                        error "Error durante la construcción del proyecto Angular"
                    }
                }
            }
        }

        stage('Deploy to Firebase') {
            steps {
                script {
                    try {
                        // Despliega a Firebase usando el token de entorno
                        sh 'firebase deploy --only hosting --token $FIREBASE_DEPLOY_TOKEN'
                    } catch (Exception e) {
                        error "Error durante el despliegue a Firebase"
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
