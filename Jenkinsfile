pipeline {
    agent any
    tools { 
        maven 'mymaven' 
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
        
        
        stage ('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        
         stage ('Deploy-To-Tomcat') {
            steps {
               sshagent(['Tomcat']) {
                  sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@3.90.50.121:8080/home/ubuntu/prod/apache-tomcat-8.5.40/webapps/zap_test.war'
               }      
           }       
       }
        
        stage ('DAST') {
          steps {
            
               sh ' "docker run -t owasp/zap2docker-stable zap-baseline.py -t http://3.90.50.121:8080/webapp/" || true'
        
      }
    }
    
  }
}
