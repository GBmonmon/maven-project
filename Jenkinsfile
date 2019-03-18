pipeline {

  agent any
  tools {
      maven 'localMaven'
  }

  stages{

    stage('Build'){
      steps{
        sh 'mvn clean package'
      }
      post{
        success{
          echo 'Now Archiving...'
          archiveArtifacts artifacts: '**/target/*.war'
        }
      }

    }

    stage('Build to staging'){
      steps{
        build job: 'Deploy-to-staging'
      }
    }


    }




}
