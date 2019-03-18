pipeline {

    agent any
    tools {
        localMaven
    }

    stages{

        stage('Build'){
          steps{
              sh 'mvn clean package'
          }
          post{
              echo 'Now Archiving...'
              archiveArtifacts artifacts: '**/target/*.war'
          }
        }


    }




}
