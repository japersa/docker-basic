pipeline {
    agent any

    environment {
        SONARQUBE_URL = 'http://192.168.220.131:9000'
        SONARQUBE_TOKEN = 'sqa_b3a041656e7bb4f809ead486172be9cda3642e61'
    }

    tools {
        dotnetsdk 'dotnet-9.0.203'
        nodejs 'node-20.19.2'
    }

    stages {
        stage('Checkout') {
            steps {
                // Clone the GitHub repository
                git url: 'https://github.com/toromartinezalvaro/docker-basic.git', branch: 'main'
            }
        }
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
        // This stage performs static code analysis on the .NET backend using SonarQube.
        // It uses the SonarScanner for .NET to send code quality and security data to the SonarQube server.
        // The analysis is started with 'dotnet sonarscanner begin', then the project is built, and finally 'dotnet sonarscanner end' sends the results.
        // The SonarQube environment and authentication token are injected from Jenkins global configuration and environment variables.
        // 'withSonarQubeEnv' is a block provided by the SonarQube Scanner plugin for Jenkins.
        stage('SonarQube Analysis') {
            steps {
                // Run SonarQube analysis for .NET backend
                withSonarQubeEnv('SonarQube') {
                    dir('10-net9-remix-pg-env/Backend') {
                        sh 'dotnet sonarscanner begin /k:"backend-project" /d:sonar.host.url=$SONARQUBE_URL /d:sonar.login=$SONARQUBE_TOKEN' // Start SonarQube analysis
                        sh 'dotnet build' // Build the backend for analysis
                        sh 'dotnet sonarscanner end /d:sonar.login=$SONARQUBE_TOKEN' // End SonarQube analysis and send results
                    }
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