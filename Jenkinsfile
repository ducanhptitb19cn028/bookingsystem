pipeline {
    agent any

    tools {
        maven 'Maven' // Make sure Maven is installed and configured in Jenkins (e.g., "Maven 3.6.3")
        jdk 'JDK11' // Make sure JDK 11 is installed and configured in Jenkins
    }

    environment {
        GITHUB_REPO = 'https://github.com/ducanhptitb19cn028/bookingsystem.git'
        GIT_CREDENTIALS_ID = 'github-credentials' // Set this to the ID of your GitHub credentials in Jenkins
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
