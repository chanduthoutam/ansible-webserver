pipeline {
    agent any
    stages {
        stage('Delete the workspace') {
            steps {
                cleanWs()
            }
        }
        stage('Second stage') {
            steps {
                echo "Second stage"
            }
        }
        stage('Third stage') {
            steps {
                echo "Third stage"
            }
        }
    }
}