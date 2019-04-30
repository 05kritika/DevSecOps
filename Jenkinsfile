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
        stage ('Check-Git-Secrets') {
           steps {
              sh 'rm trufflehog || true'
              sh 'docker run gesellix/trufflehog --json https://github.com/cehkunal/webapp.git > trufflehog'
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
               sshagent(['Tomcat']) {
                  sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@ec2-54-164-167-157.compute-1.amazonaws.com:/home/ubuntu/prod/apache-tomcat-8.5.40/webapps/webapp.war'
               }      
           }       
       }
    }
}
