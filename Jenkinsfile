pipeline {
    agent any

    stages {
        stage("check version")  {
            steps {
                sh "node --version"
                sh "npm --version"
            }
        }
        stage('Build') { 
            steps {
                sh 'npm install' 
            } 
        }
    }
}
