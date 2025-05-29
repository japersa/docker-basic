pipeline {
    agent any

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
