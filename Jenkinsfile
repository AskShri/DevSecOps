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
    
    
    stage ('Build') {
      steps {
      sh 'mvn clean package'
       }
    }
    
    stage ('Deploy-To-Tomcat') {
            steps {
          
             
            sh 'sshpass -p "Ap@ch3adm1n@Syn0psys" scp /target/WebApp.war dsoadmin@52.170.151.39:apache/apache-tomcat-9.0.26/webapps/WebApp.war'    
         
             
            
           }   
          }
  }
}
