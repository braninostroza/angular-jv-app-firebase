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
                        // Clona el repositorio desde GitHub (añadir credenciales si es privado)
                        git url: 'https://github.com/braninostroza/angular-jv-app-firebase.git', branch: 'main', credentialsId: 'your-credential-id'  // Si es privado
                        echo '[SUCCESS] - El repositorio ha sido clonado correctamente'
                    } catch (Exception e) {
                        error "[ERROR] - Error durante la clonación del repositorio: ${e.message}"
                    }
                }
            }
        }
        
        stage('Install Dependencies') {
            steps {
                script {
                    try {
                        // Instala dependencias de Node.js y Angular (en Windows usa 'bat' en lugar de 'sh')
                        bat 'npm ci'  // Comando compatible con Windows
                        echo '[SUCCESS] - Las dependencias se han instalado correctamente'
                    } catch (Exception e) {
                        error "[ERROR] - Error durante la instalación de dependencias: ${e.message}"
                    }
                }
            }
        }
        
        stage('Build Angular') {
    steps {
        script {
            try {
                // Usando npx para ejecutar la CLI local de Angular
                bat 'npx ng build --configuration production'
                echo '[SUCCESS] - El proyecto Angular se ha compilado correctamente'
            } catch (Exception e) {
                error "[ERROR] - Error durante la compilación del proyecto Angular: ${e.message}"
            }
        }
    }
}
        
        stage('Deploy to Firebase') {
            steps {
                script {
                    try {
                        // Despliega a Firebase usando la variable de entorno (en Windows usa 'bat' en lugar de 'sh')
                        bat 'firebase deploy --token %FIREBASE_TOKEN%'
                        echo '[SUCCESS] - Despliegue a Firebase realizado con éxito'
                    } catch (Exception e) {
                        error "[ERROR] - Error durante el despliegue a Firebase: ${e.message}"
                    }
                }
            }
        }
    }
    
    post {
        always {
            script {
                if (currentBuild.currentResult == 'SUCCESS') {
                    echo '[SUCCESS] - El pipeline se completó correctamente'
                } else {
                    echo '[FAILURE] - El pipeline falló. Revisa los logs para más detalles.'
                }
            }
        }
    }
}
