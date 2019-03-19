pipeline {

  agent any
  tools {
      maven 'localMaven'
  }

  parameters {
    string(name: "tomcat_dev", defaultValue:"13.56.165.19", description: "for staging")
    string(name: "tomcat_prod", defaultValue:"18.144.71.147", description: "for prod")
  }

  triggers{
    pollSCM("* * * * *")
  }

stages{
  stage("Build"){
    steps{
      sh "mvn clean package"
    }
    post{
      success{
        echo "Now Archiving..."
        archiveArtifacts artifacts: "**/target/*.war"
      }
    }

  }

  stage("Deployment"){
    parallel{
      stage("Deploy-to-staging"){
        steps{
          sh "ssh -i /Users/jerry/Documents/awsPem/tomcatDemo.pem ec2-user@${params.tomcat_dev} \"sudo chmod 777 /var/lib/tomcat7\""
          sh "scp -i /Users/jerry/Documents/awsPem/tomcatDemo.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
        }
      }

      stage("Deploy-to-production"){
        steps{
          sh "scp -i /Users/jerry/Documents/awsPem/tomcatDemo.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
        }
      }
    }
  }
}


}
