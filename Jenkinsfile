pipeline {
    agent any
    parameters {
        string(name: 'tomcat_staging', defaultValue: '3.19.218.240', description: 'Staging Server')
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
                        bat "scp -i C:/Users/srikann/Downloads/tomcatdemo.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
        
    }    
}  
 