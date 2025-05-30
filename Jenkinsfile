pipeline {
    agent any

    tools {
        dotnetsdk 'dotnet-9.0.203'
        nodejs 'node-20.19.2'
    }

    environment {
        // Add dotnet tools to PATH for future use
        PATH = "${env.PATH}:/home/server3/.dotnet/tools"
        SONARQUBE_URL = 'http://192.168.220.131:9000'
        SONARQUBE_TOKEN = 'sqa_b3a041656e7bb4f809ead486172be9cda3642e61'
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
        stage('Frontend - Test') {
            steps {
                dir('10-net9-remix-pg-env/Frontend') {
                    echo 'Running tests...'
                    sh 'npm test'
                }
            }
        }
        // ---
        // NOTE:
        // The original project (from the professor) does NOT have a separate test project for the backend.
        // Therefore, running 'dotnet test' or code coverage on the backend will fail in both this fork and the original.
        // These stages are commented out to avoid pipeline failures, but the structure is kept for future improvements.
        // ---
        // stage('Backend - Test') {
        //     steps {
        //         dir('10-net9-remix-pg-env/Backend') {
        //             echo 'Running tests...'
        //              sh 'dotnet test --no-build --verbosity normal' // Disabled: Tests should be in a separate test project, not in the main project.
        //         }
        //     }
        // }
        stage('Backend - Static Analysis (SonarQube)') {
            steps {
                dir('10-net9-remix-pg-env/Backend') {
                    echo 'Running static analysis with SonarQube...'
                    withSonarQubeEnv('SonarQube') {
                        withEnv(['PATH+DOTNET=/home/server3/.dotnet/tools']) {
                            sh 'dotnet sonarscanner begin /k:"backend-project" /d:sonar.host.url=$SONARQUBE_URL /d:sonar.login=$SONARQUBE_TOKEN'
                            sh 'dotnet build'
                            sh 'dotnet sonarscanner end /d:sonar.login=$SONARQUBE_TOKEN'
                        }
                    }
                }
            }
        }
        stage('Backend - Publish') {
            steps {
                dir('10-net9-remix-pg-env/Backend') {
                    echo 'Publishing the project...'
                    sh 'dotnet publish --configuration Release --no-build -o ./publish'
                }
            }
        }
        // stage('Backend - Code Coverage') {
        //     steps {
        //         dir('10-net9-remix-pg-env/Backend') {
        //             echo 'Running code coverage...'
        //             // sh 'dotnet test --collect:"XPlat Code Coverage" --no-build --verbosity normal' // Disabled: No separate test project available yet.
        //         }
        //     }
        // }
    }

    post {
        always {
            echo 'This will always run after the stages.'
        }
    }
}