pipeline {
    agent any

    environment {
        SONARQUBE_URL = 'http://192.168.220.131:9000'
    }

    tools {
        dotnetsdk 'dotnet-9.0.203'
        nodejs 'node-20.19.2'
    }

    stages {
        stage('Build') {
            steps {
                echo 'esto sirve para construir'
            }
        }
        stage('Test') {
            steps {
                echo 'esto sirve para probar'
            }
        }
        stage('Deploy') {
            steps {
                echo 'esto sirve para desplegar'
            }
        }
    }

    post {
        always {
            echo 'Esto siempre se ejecutará después de las etapas.'
        }
    }
}
