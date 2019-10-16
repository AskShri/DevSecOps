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
    
      stage ('Source Composition Analysis') {
      steps {
         sh 'rm owasp* || true'
         sh 'wget "https://raw.githubusercontent.com/cehkunal/webapp/master/owasp-dependency-check.sh" '
         sh 'chmod +x owasp-dependency-check.sh'
         sh 'bash owasp-dependency-check.sh'
         sh 'cat /var/lib/jenkins/OWASP-Dependency-Check/reports/dependency-check-report.xml'
        
      }
    } 
        stage ('SAST') {
      steps {
        withSonarQubeEnv('sonar') {
          sh 'mvn sonar:sonar'
          sh 'cat /var/lib/jenkins/workspace/dso/target/sonar/report-task.txt'
        }
      }
         }
    stage ('Build') {
      steps {
      sh 'mvn clean package'
       }
    }
    
    stage ('Deploy-To-Tomcat') {
            steps {
          
             
            sh 'sshpass -p "Ap@ch3adm1n@Syn0psys" scp ./target/WebApp.war dsoadmin@52.170.151.39:/apache/apache-tomcat-9.0.26/webapps/WebApp.war'    
            }
    }
          stage ('DAST') {
      steps {
    
         sh 'docker run -t owasp/zap2docker-stable zap-baseline.py -t http://52.170.151.39:8080/WebApp/ || true'
        }
    
    }   
            
         
  }
}

