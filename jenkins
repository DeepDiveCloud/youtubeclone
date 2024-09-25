pipline {
    agent any
    stages{
        stage("checkout"){
            steps{
                checkout scm
            }
        }
        stage("npm"){
            staps{
                sh 'sudo npm install'
            }
        }
    }
}
