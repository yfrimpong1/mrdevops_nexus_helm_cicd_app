pipeline{
    agent any
    
    stages{

        stage('Sonar Quality Check'){

            agent{              
                docker{
                    image 'maven'
                }
            }
            
            steps{

                script{

                    withSonarQubeEnv(credentialsId: 'sonar-token') {
                       sh 'mvn clean package sonar:sonar'
                    }
                    
                }
            }
        }
       
        stage('Qyality Gate status'){
            steps{
                script{
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar-token'
                }
            }
        }

        
    }
}