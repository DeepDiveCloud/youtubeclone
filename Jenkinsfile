pipeline{
  agent any
  //
  tools{
    jdk 'jdk17'
    nodejs 'nodejs16'
  }
  stages{
    stage('git checkout'){
      git branch: 'main', url: 'https://github.com/DeepDiveCloud/youtubeclone.git'
    }
  }
  
}
