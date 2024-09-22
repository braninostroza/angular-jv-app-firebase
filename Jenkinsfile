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
        
        stage('Check PATH') {
            steps {
                script {
                    bat 'echo PATH: %PATH%'  // Verificar el PATH actual
                }
            }
        }
        
        stage('Install Dependencies') {
            steps {
                script {
                    bat 'npm cache clean --force'  // Limpiar caché
                    bat 'npm ci'  // Instalación limpia de dependencias
                    bat 'npm install @angular/cli'
                    bat 'npm install --save-dev @angular-devkit/build-angular --legacy-peer-deps'
                    echo '[SUCCESS] - Las dependencias se han instalado correctamente'
                }
            }
        }
        
        stage('Check Angular CLI') {
            steps {
                script {
                    bat 'where cmd'  // Verificar si cmd está disponible
                    bat 'where ng'   // Verificar si Angular CLI está en el PATH
                }
            }
        }

        stage('Build Angular') {
            steps {
                script {
                    bat 'npm run build --configuration production'
                    echo '[SUCCESS] - Construcción de Angular completada'
                }
            }
        }
        
        stage('Deploy to Firebase') {
            steps {
                script {
                    bat "firebase deploy --token ${FIREBASE_TOKEN}"
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
