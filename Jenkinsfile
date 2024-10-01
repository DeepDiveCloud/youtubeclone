pipeline{
    agent any
    //
    tools{
        jdk 'jdk17'
        nodejs 'nodejs16'
    }
    environment {
           SCANNER_HOME=tool 'sonar-scanner'
    }
    stages {
        stage('Checkout from Git'){
            steps{
                git branch: 'main', url: 'https://github.com/DeepDiveCloud/youtubeclone.git'
            }
        }
        stage('OWASP FS SCAN') {
            steps {
                dependencyCheck additionalArguments: '--scan  --disableYarnAudit --disableNodeAudit', odcInstallation: 'Dependcy-check'
                    dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        } 
        stage('Install Dependencies') {
            steps {
                sh "npm install"
            }
        }
       stage('SONARQUBE ANALYSIS') {
            steps {
                withSonarQubeEnv('sonar-server') {
                    sh " $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=youtubeclone -Dsonar.projectKey=youtubeclone "
                }
            }
        }
        stage('QualityGate'){
          steps {
            script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar-api'
                }
              }
         }  
   }
}
        
