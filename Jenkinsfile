pipeline {
    agent any

    environment {
        FIREBASE_TOKEN = credentials('FIREBASE_TOKEN')
        PATH = "$HOME/.firebase/bin:$PATH"
    }

    stages {
        stage('Install Firebase CLI') {
            steps {
                sh '''
                curl -sL https://firebase.tools | bash
                which firebase
                firebase --version
                '''
            }
        }

        stage('Deploy to Testing') {
            steps {
                sh 'firebase use devops-final-testing'
                sh 'firebase deploy --only hosting --token "$FIREBASE_TOKEN"'
            }
        }

        stage('Approval for Staging') {
            steps {
                input message: 'Deploy to Staging?'
            }
        }

        stage('Deploy to Staging') {
            steps {
                sh 'firebase use devops-final-staging'
                sh 'firebase deploy --only hosting --token "$FIREBASE_TOKEN"'
            }
        }

        stage('Approval for Production') {
            steps {
                input message: 'Deploy to Production?'
            }
        }

        stage('Deploy to Production') {
            steps {
                sh 'firebase use devops-final-production'
                sh 'firebase deploy --only hosting --token "$FIREBASE_TOKEN"'
            }
        }
    }
}
