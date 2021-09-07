pipeline{
    agent any
    
    environment{
        PATH = "/opt/maven3/bin:$PATH"
    }
    stages{
        stage("Git Checkout"){
            steps{
                git credentialsId: 'firstpipeline', url: 'https://github.com/VendraH/test-git-m2-jenkins.git'
            }
         }
         stage("Maven Build"){
             steps{
                 sh "mvn clean package"
                 
             }
         }
         stage("deploy-dev"){
             steps{
                 sshagent(['tomcat-new']){
                 sh """
                     scp -o StrictHostKeyChecking=no target/myweb.war ec2-user@172.31.10.32:/home/ec2-user/apache-tomcat-9.0.52/webapps/
                     
                     ssh ec2-user@172.31.10.32 /home/ec2-user/apache-tomcat-9.0.52/bin/shutdown.sh
                     
                     ssh ec2-user@172.31.10.32 /home/ec2-user/apache-tomcat-9.0.52/bin/startup.sh
                     
                 """
                 
                 }
             }
         }
    }
}
