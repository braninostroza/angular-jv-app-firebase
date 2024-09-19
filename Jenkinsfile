pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                // Clona el repositorio desde GitHub
                git url: 'https://github.com/braninostroza/angular-jv-app-firebase.git', branch: 'main'
            }
        }

        /*stage('Install Dependencies') {
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
        }*/

        stage('Build Angular') {
            steps {
                script {
                    try {
                        // Compila el proyecto Angular
                        sh 'npm run build'
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
                        // Despliega a Firebase usando la variable de entorno
                        sh 'firebase deploy --token ${FIREBASE_DEPLOY_TOKEN}'
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
