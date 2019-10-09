pipeline {
  agent any 
 
tools {
  maven 'maven'
}
  
  stages {
    stage ('Initialize') {
      steps {
        sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
            ''' 
      }
    }
    
      stage ('Check-Git-Secrets') {
      steps {
        sh 'rm trufflehog || true'
        sh 'docker run gesellix/trufflehog --json https://github.com/AskShri/DevSecOps.git > trufflehog'
        sh 'cat trufflehog'
      }
    }
    
     stage ('SAST') {
      steps {
        withSonarQubeEnv('sonar') {
          sh 'mvn sonar:sonar'
         
        }
      }
    }
    
            stage ('Source Composition Analysis') {
      steps {
         sh 'rm /var/lib/jenkins/workspace/dso/owasp* || true'
         sh 'bash /var/lib/jenkins/workspace/dso/owasp-dependency-check.sh'
        
      }
    }
    
    stage ('Build') {
      steps {
      sh 'mvn clean package'
       }
    }
    
    stage ('Deploy-To-Tomcat') {
            steps {
           sshagent(['tomcat']) {
                sh 'sudo scp -r /var/lib/jenkins/workspace/dso/target/*.war dsoadmin@52.170.151.39:/apache/apache-tomcat-9.0.26/webapps/WebApp.war'
              }      
           }   
          }
  }
}
