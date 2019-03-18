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
        build job: 'deploy-to-staging'
      }
    }


    }

    stage('Build to prod'){
      steps{
        timeout(time:5, unit:'DAYS'){
          input message: 'Approve production deployment?'
        }
        build job: 'deploy-to-prod'
      }
      post{
        success{
          echo "Code deploy to production"
        }
        failure{
          echo "Fail"
        }

      }
    }




}
