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
         stage('Trivy fs scanning'){
          steps {
            sh "trivy fs ." 
          }
         }
         stage('Docker build and push '){
          steps {
            script {
              withDockerRegistry(credentialsId: 'docker', toolName: 'docker'){
                    sh "docker build --build-argREACT_APP_RAPID_API_KEY=f3f24d5e22msh7a205a012674785p1b131fjsn8b7288a78e45 . -t youtubeclone:latest"
                    sh "docker tag youtubeclone maxjith/youtubeclone:latest " 
                    sh "docker push maxjith/youtubeclone:latest " 
                      }
                  }
              }
          }
  }
}
        
