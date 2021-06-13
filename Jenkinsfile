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
                     else if (env.Build_Tool == 'Ant')
                     {
                         bat 'ant -buildfile build.xml'
                     }
                }
            }
        }
        stage('Test') {
            steps {
                    script 
                    {
                        if ( env.Perform_Sonar_Scan == true)
                        {
                           withSonarQubeEnv('SonarQube') 
                           {
                            bat 'mvn verify'
                            }
                        }
                    }
            }
        }
        stage('Deploy') {
            steps {
                    script 
                    {
                        if ( env.Perform_EC2_deployment == true)
                        {
                           withSonarQubeEnv('SonarQube') 
                           {
                            bat 'mvn verify'
                            }
                        }
                    }
            }
        }
    }
}
