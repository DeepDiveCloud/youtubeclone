pipeline {
    agent any
    tools {
      nodejs '20.7.0'
    }
    stages {
        stage("check out"){
            steps {
                checkout scm
            }
        }
        stage('print versions') {
          steps {
            sh 'npm version'
          }
        }
        stage('Install') { 
            steps {
              sh 'npm install'
            }
        }
        stage('Build') { 
            steps {
                sh 'npm run build' 
            }
        }
        
    }
}
