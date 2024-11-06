pipeline {
    agent any

    tools {
        maven 'Maven' // Ensure Maven is configured in Jenkins
        jdk 'JDK' // Ensure JDK is configured in Jenkins
    }

    environment {
        JAVA_HOME = '/opt/java/openjdk' // Set the path to your JDK installation
        GITHUB_REPO = 'https://github.com/ducanhptitb19cn028/bookingsystem.git'
        GIT_CREDENTIALS_ID = 'github-credentials' // Ensure this ID is correct
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: "${env.GITHUB_REPO}", credentialsId: "${env.GIT_CREDENTIALS_ID}"
            }
        }

        stage('Build') {
            steps {
                script {
                    // Run Maven build command
                    sh 'mvn clean install'
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    // Run Maven test command
                    sh 'mvn test'
                }
            }
        }

        stage('Push Changes to GitHub') {
            steps {
                script {
                    // Configure Git user
                    sh 'git config user.name "Jenkins"'
                    sh 'git config user.email "jenkins@example.com"'

                    // Add, commit, and push any changes made by the pipeline
                    sh 'git add .'
                    sh 'git commit -m "Automated commit by Jenkins pipeline" || echo "No changes to commit"'
                    sh 'git push origin main'
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

