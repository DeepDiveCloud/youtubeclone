pipeline{
  agent any
  //
  tools{
    jdk 'jdk17'
    nodejs 'nodejs16'
  }
  stages{
    stage('git checkout'){
      steps{
      git branch: 'main', url: 'https://github.com/DeepDiveCloud/youtubeclone.git'
       }
    }
    stage('OWASP FS SCAN') {
       steps { 
         dependencyCheck additionalArguments: '--scan --disableYarnAudit --disableNodeAudit', odcInstallation: 'Dependcy-check'
         dependencyCheckPublisher pattern: ''**/dependency-checkreport.xml'
       }
    }
  }
}
