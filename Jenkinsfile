pipeline{
    agent any
    tools{
        jdk 'jdk17'
        nodejs 'node16'
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
        stage('Install Dependencies') {
            steps {
                sh "npm install"
            }
        }
        stage("Sonarqube Analysis "){
            steps{
                withSonarQubeEnv('sonar-server') {
                    sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=youtube \
                    -Dsonar.projectKey=youtube '''
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
        stage('OWASP FS SCAN') {
            steps {
                dependencyCheck additionalArguments: '--scan ./ --disableYarnAudit --disableNodeAudit', odcInstallation: 'DC'
                dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }
        stage('TRIVY FS SCAN') {
            steps {
                sh "trivy fs . &gt; trivyfs.json"
            }
        }
        stage("Docker Build &amp; Push"){
            steps{
                script{
                   withDockerRegistry(credentialsId: 'docker', toolName: 'docker'){   
                       sh "docker build -t youtube ."
                       sh "docker tag youtube maxjith/youtube:latest "
                       sh "docker push maxjith/youtube:latest "
                    }
                }
            }
        }
        stage("TRIVY"){
            steps{
                sh "trivy image /youtube:latest &gt; trivy.json" 
            }
        }
        stage ("Remove container") {
            steps{
                sh "docker stop youtube | true"
                sh "docker rm youtube | true"
             }
        }
        stage('Deploy to container'){
            steps{
                sh 'docker run -d --name youtube -p 3000:3000 maxjith/youtube:latest'
            }
        }
    }
}
