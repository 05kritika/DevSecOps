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
                sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@54.164.167.157:/prod/apache-tomcat-8.5.40/webapps/webapp.war'
              }      
           }       
    }
    }
}
