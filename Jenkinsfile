pipeline{
    agent any
    
    environment{
        PATH = "/opt/maven3/bin:$PATH"
    }
    stages{
        stage("Git Checkout"){
            steps{
                git credentialsId: 'javahome2', url: 'https://github.com/lingamakhil/repo1.git'
            }
        }
        stage("Maven Build"){
            steps{
                sh "mvn clean package"
                sh "mv target/*.war target/myweb.war"
            }
        }
        stage("deploy-dev"){
            steps{
                sshagent(['tomcat-new']) {
                sh """
                    scp -o StrictHostKeyChecking=no target/myweb.war  ec2-user@172.31.10.114:/home/ec2-user/apache-tomcat-11.0.0-M4 /webapps/
                    
                    ssh ec2-user@172.31.10.114 /home/ec2-user/apache-tomcat-9.0.73/bin/shutdown.sh
                    
                    ssh ec2-user@172.31.10.114 /home/ec2-user/apache-tomcat-9.0.73/bin/startup.sh
                
                """
            }
            
            }
        }
    }
}
