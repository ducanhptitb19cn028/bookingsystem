pipeline {
    agent any

    tools {
        maven 'Maven'
        jdk 'JDK'
    }

    environment {
        JAVA_HOME = '/usr/lib/jvm/java-17-openjdk-arm64'
        GITHUB_REPO = 'https://github.com/ducanhptitb19cn028/bookingsystem.git'
        GIT_CREDENTIALS_ID = 'github-credentials'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: "${env.GITHUB_REPO}",
                    credentialsId: "${env.GIT_CREDENTIALS_ID}"
            }
        }

        stage('Build') {
            steps {
                script {
                    // First verify we're in the correct directory
                    sh 'pwd'
                    sh 'ls -la'

                    // Only cd if the directory exists
                    sh '''
                        if [ -d "bookingservice" ]; then
                            cd bookingservice
                        fi
                        mvn clean install -DskipTests
                    '''
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    sh '''
                        if [ -d "bookingservice" ]; then
                            cd bookingservice
                        fi
                        mvn test
                    '''
                }
            }
        }

        stage('Push Changes to GitHub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: "${env.GIT_CREDENTIALS_ID}",
                                                    usernameVariable: 'GIT_USERNAME',
                                                    passwordVariable: 'GIT_PASSWORD')]) {
                        sh '''
                            git config user.name "phucanhprono1"
                            git config user.email "anhnnp.b19cn029@stu.ptit.com.vn"
                            git add .
                            git diff --quiet && git diff --staged --quiet || git commit -m "Automated commit by Jenkins pipeline"
                            git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/ducanhptitb19cn028/bookingsystem.git main
                        '''
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}