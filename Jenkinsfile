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
                dependencyCheck additionalArguments: '--scan ./app/backend --disableYarnAudit --disableNodeAudit', odcInstallation: 'Dc'
                    dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        } 
        stage('TRIVY FS SCAN') {
            steps {
                sh "trivy fs ."
            }
        }                
    }
}
