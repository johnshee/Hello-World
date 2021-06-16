pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                  git branch: env.Github_Branch, changelog: false, credentialsId: env.Github_Credentials, poll: false, url: env.Github_URL
            }
        }        
        stage('Build') {
            steps {
                script 
                {
                    if ( env.Build_Tool == 'Maven')
                     {
                         bat 'mvn package -s settings.xml'
                     }
                     else if ( env.Build_Tool == 'Ant')
                     {
                         bat 'ant -buildfile build'
                     }
                }
            }
        }
        stage('Test') {
            steps {
                script
                    {
                        if (env.Perform_Sonar_Scan == 'true' )
                        {
                                bat 'mvn sonar:sonar -Dsonar.projectKey=sonarqubetest -Dsonar.host.url=http://localhost:9000 -Dsonar.login=d62607ff66391e8faeed90024ba205dc8cda4d2b'
                        }
                   }
                }
          }
        stage('Deploy') {
            steps {
                script {
                    if (env.Perform_EC2_deployment == 'true')
                    {
                        withCredentials([string(credentialsId: 'aws_key', variable: 'aws_key'), string(credentialsId: 'ec2_access_key', variable: 'ec2_access_key'), string(credentialsId: 'ec2_secret_key', variable: 'ec2_secret_key'), , string(credentialsId: 'aws_key', variable: 'aws_key')]) 
                        {
                             bat 'echo $aws_key > aws_key.pub'
                             bat 'ansible-playbook -vvvvv ansible/playbook.yml -e \"ec2_access_key=$ec2_access_key\" -e \"ec2_secret_key=$ec2_secret_key\"'
                        }
                    }
                }
            }
        }
    }
}
