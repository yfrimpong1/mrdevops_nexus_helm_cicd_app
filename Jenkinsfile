pipeline{
    agent any
    environment{
        VERSION = "${env.BUILD_ID}"
    }

    stages{

        stage('sonar quality check'){

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

        stage('Docker build & docker push to Nexus Repository'){

            steps{
                script{
                    withCredentials([string(credentialsId: 'nexus_passwd', variable: 'nexus_creds')]) {
                    sh '''
                        docker build -t 192.168.100.6:8083/springapp:${VERSION} .
                        docker login -u admin -p $nexux_creds 192.168.100.6:8083
                        docker push  192.168.100.6:8083/springapp:${VERSION}
                        docker rmi 192.168.100.6:8083/springapp:${VERSION}
                        
                    '''
                    }

                }

            }
           
        }
    }
}