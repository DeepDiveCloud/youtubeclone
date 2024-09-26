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
        stage('Static code analysis: Sonarqube'){
            steps{
               script{
                   def SonarQubecredentialsId = 'sonarqube-api'
                   statiCodeAnalysis(SonarQubecredentialsId)
               }
            }
       }
       stage('Quality Gate Status Check : Sonarqube'){
            steps{
               script{ 
                   def SonarQubecredentialsId = 'sonarqube-api'
                   QualityGateStatus(SonarQubecredentialsId)
               }
            }
       }
        
    }
}
