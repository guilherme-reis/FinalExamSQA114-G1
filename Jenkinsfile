pipeline {
    agent any

    environment {
        FIREBASE_TOKEN = credentials('FIREBASE_TOKEN')
    }

    stages {
        stage('Install Firebase CLI') {
            steps {
                sh '''
                    mkdir -p $HOME/firebase-cli
                    curl -L https://firebase.tools/bin/linux/latest -o $HOME/firebase-cli/firebase
                    chmod +x $HOME/firebase-cli/firebase
                    $HOME/firebase-cli/firebase --version
                '''
            }
        }

        stage('Deploy to Testing') {
            steps {
                sh '''
                    $HOME/firebase-cli/firebase use devops-final-testing-grm
                    $HOME/firebase-cli/firebase deploy --only hosting --token "$FIREBASE_TOKEN"
                '''
            }
        }

        stage('Approval for Staging') {
            steps {
                input message: 'Deploy to Staging?'
            }
        }

        stage('Deploy to Staging') {
            steps {
                sh '''
                    $HOME/firebase-cli/firebase use devops-final-staging-grm
                    $HOME/firebase-cli/firebase deploy --only hosting --token "$FIREBASE_TOKEN"
                '''
            }
        }

        stage('Approval for Production') {
            steps {
                input message: 'Deploy to Production?'
            }
        }

        stage('Deploy to Production') {
            steps {
                sh '''
                    $HOME/firebase-cli/firebase use devops-final-production-grm
                    $HOME/firebase-cli/firebase deploy --only hosting --token "$FIREBASE_TOKEN"
                '''
            }
        }
    }
}
