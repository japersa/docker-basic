pipeline {
    agent any

    tools {
        dotnetsdk 'dotnet-9.0.203'
        nodejs 'node-20.19.2'
    }

    environment {
        // Add dotnet tools to PATH for future use
        PATH = "${env.PATH}:/home/server3/.dotnet/tools"
    }

    stages {
        stage('Check versions') {
            steps {
                script {
                    echo 'Node.js version:'
                    sh 'node -v'
                    echo 'NPM version:'
                    sh 'npm -v'
                    echo 'Dotnet version:'
                    sh 'dotnet --version'
                }
            }
        }
        stage('Checkout') {
            steps {
                // Clone the GitHub repository
                git url: 'https://github.com/toromartinezalvaro/docker-basic.git', branch: 'main'
            }
        }
        stage('Backend - Restore') {
            steps {
                dir('10-net9-remix-pg-env/Backend') {
                    echo 'Restoring dependencies...'
                    sh 'dotnet restore'
                }
            }
        }
        stage('Backend - Build') {
            steps {
                dir('10-net9-remix-pg-env/Backend') {
                    echo 'Building the project...'
                    sh 'dotnet build --configuration Release --no-restore'
                }
            }
        }
        stage('Frontend - Install') {
            steps {
                dir('10-net9-remix-pg-env/Frontend') {
                    echo 'Installing dependencies...'
                    sh 'npm install'
                }
            }
        }
        stage('Frontend - Build') {
            steps {
                dir('10-net9-remix-pg-env/Frontend') {
                    echo 'Building the project...'
                    sh 'npm run build'
                }
            }
        }
    }

    post {
        always {
            echo 'This will always run after the stages.'
        }
    }
}