pipeline {
    agent any

    environment {
        FIREBASE_TOKEN = credentials('FIREBASE_TOKEN')
        PATH = "$HOME/firebase-cli:$PATH"
    }

    stages {
        stage('Install Firebase CLI') {
            steps {
                sh '''
                    mkdir -p $HOME/firebase-cli
                    curl -L https://firebase.tools/bin/linux/latest -o $HOME/firebase-cli/firebase
                    chmod +x $HOME/firebase-cli/firebase
                    ./firebase --version
                '''
            }
        }

        stage('Deploy to Testing') {
            steps {
                sh '''
                    firebase use devops-final-testing
                    firebase deploy --only hosting --token "$FIREBASE_TOKEN"
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
                    firebase use devops-final-staging
                    firebase deploy --only hosting --token "$FIREBASE_TOKEN"
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
                    firebase use devops-final-production
                    firebase deploy --only hosting --token "$FIREBASE_TOKEN"
                '''
            }
        }
    }
}
