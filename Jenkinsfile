pipeline {
    agent any
    parameters {
        string(name: 'tomcat_staging', defaultValue: '18.218.251.206', description: 'Staging Server')
        string(name: 'tomcat_production', defaultValue: '18.189.16.93', description: 'Prodution Server')
    }
    triggers {
        pollSCM('* * * * *')
    }
stages{
        stage('Build'){
            steps {
                    bat 'mvn clean package'
                  }
             post {
                success{
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage('Deployment'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        bat "scp -v -o StrictHostKeyChecking=no -i 'C:/Users/srikann/Downloads/tomcatdemo.pem' **/target/*.war ec2-user@${params.tomcat_staging}:/var/lib/tomcat7/webapps"
                    }
                }
                stage ('Deploy to Production'){
                    steps {
                        bat "scp -v -o StrictHostKeyChecking=no -i 'C:/Users/srikann/Downloads/tomcatdemo.pem' **/target/*.war ec2-user@${params.tomcat_production}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
        
    }    
}  
 