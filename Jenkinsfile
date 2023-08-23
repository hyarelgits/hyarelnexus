pipeline{
    
    agent any 
    
    stages {
        
        stage('Git Checkout'){
            
            steps{
                
                script{
                    
                    git branch: 'main', url: 'https://github.com/hyarelgits/hyarelnexus.git'
                }
            }
        }
        stage('UNIT testing'){
            
            steps{
                
                script{
                    
                    sh 'mvn test'
                }
            }
        }
        stage('Integration testing'){
            
            steps{
                
                script{
                    
                    sh 'mvn verify -DskipUnitTests'
                }
            }
        }
        stage('Maven build'){
            
            steps{
                
                script{
                    
                    sh 'mvn clean install'
                }
            }
        }
        stage('Static code analysis'){

            steps{

                script{

                    withSonarQubeEnv(credentialsId: 'p4') {

                        sh 'mvn clean package sonar:sonar'
                    }
                   }

                }
            }
            stage('Quality gat ststatus'){

                steps{

                    script{

                        waitForQualityGate abortPipeline: false, credentialsId: 'p4'
                    }
                }
            }
            stage('upload war file to nexus'){

                steps{

                    script{
          
                        nexusArtifactUploader artifacts: 
                        [
                            [
                             
                             artifactId: 'springboot',
                             classifier: '', file: 'target/Uber.jar', 
                             type: 'jar'
                             ]
                        ],
                        credentialsId: 'nexus-auth', 
                        groupId: 'com.example', 
                        nexusUrl: '192.168.11.128:8081', 
                        nexusVersion: 'nexus2', 
                        protocol: 'http', 
                        repository: 'p1-release', 
                        version: '1.0.0'
                    } 
               }
          }
     }

}
