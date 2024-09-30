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
                dependencyCheck additionalArguments: '--scan  --disableYarnAudit --disableNodeAudit', odcInstallation: 'Dc'
                    dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        } 
        stage('TRIVY FS SCAN') {
            steps {
                sh "trivy fs ."
            }
        }
         stage('SONARQUBE ANALYSIS') {
            steps {
                withSonarQubeEnv('sonar-server') {
                    sh " $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=youtube -Dsonar.projectKey=youtube "
                }
            }
        }
        stage("quality gate"){
           steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'Sonar-api' 
                }
            } 
        }
         stage('Install Dependencies') {
            steps {
                sh "npm install"
            }
        }
        stage("Docker Build & Push"){
            steps{
                script{
                   withDockerRegistry(credentialsId: 'docker', toolName: 'docker'){
                       sh "docker build --build-arg REACT_APP_RAPID_API_KEY=f3f24d5e22msh7a205a012674785p1b131fjsn8b7288a78e45 . -t youtubev31:latest"
                       sh "docker tag youtubev31 maxjith/youtubev31:latest "
                       sh "docker push maxjith/youtubev31:latest "
                    } 
                }
            }
        }
        stage("TRIVY"){
            steps{
                sh "trivy image maxjith/youtubev31:latest > trivyimage.txt"
            }
        }
    }
}
