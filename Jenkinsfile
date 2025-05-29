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
                // Restore and build .NET backend
                dir('10-net9-remix-pg-env/Backend') {
                    sh 'dotnet restore' // Restores .NET dependencies
                    sh 'dotnet build --configuration Release' // Builds the .NET backend in Release mode
                }
                // Install and build Node.js/Remix frontend
                dir('10-net9-remix-pg-env/Frontend') {
                    sh 'npm install' // Installs Node.js dependencies
                    sh 'npm run build' // Builds the Remix frontend
                }
            }
        }
        stage('Test') {
            steps {
                // Run .NET backend tests
                dir('10-net9-remix-pg-env/Backend') {
                    sh 'dotnet test --no-build --verbosity normal' // Runs backend unit tests
                }
                // Run Node.js/Remix frontend tests
                dir('10-net9-remix-pg-env/Frontend') {
                    sh 'npm test' // Runs frontend tests
                }
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
