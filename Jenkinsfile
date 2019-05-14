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
                  sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@ec2-54-152-25-39.compute-1.amazonaws.com:/home/ubuntu/prod/apache-tomcat-8.5.40/webapps/webapp.war'
               }      
           }       
       }
        
        stage ('DAST') {
          steps {
            
               sh ' "docker run -t owasp/zap2docker-stable zap-baseline.py -t http://ec2-54-152-25-39.compute-1.amazonaws.com:8080/target/" '
        
      }
    }
    
  }
}
