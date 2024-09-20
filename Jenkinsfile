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
                    bat 'npm cache clean --force'  // Limpiar caché
                    bat 'npm ci'  // Instalación limpia de dependencias
                    bat 'npm install -g @angular/cli'
                    bat 'npm install --save-dev @angular-devkit/build-angular --legacy-peer-deps'
                    //bat 'ng update @angular/cli @angular/core --allow-dirty --force'
                    echo '[SUCCESS] - Las dependencias se han instalado correctamente'
                }
            }
        }
        
        stage('Check Angular CLI') {
            steps {
                script {
                    bat 'where ng'  // Comando para verificar si Angular CLI está en el PATH
                }
            }
        }

        stage('Check Global Angular CLI') {
            steps {
                script {
                    bat 'npm list -g @angular/cli'
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
