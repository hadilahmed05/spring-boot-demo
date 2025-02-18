pipeline {
    
    agent any

    stages {

        stage('GetCode'){
            
            steps{


                echo "Getting Project from Git";

                git branch:"master", url : "https://github.com/revkov/spring-boot-java11.git"; 

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
                    
                    withSonarQubeEnv(credentialsId: 'sonar-api-key') {
                        
                        sh 'mvn clean package sonar:sonar'
                    }
                   
                    
                }
            }
        }
        stage("Deployment stage") {


            steps {

                script {
                    
                pom = readMavenPom file: 'pom.xml';
                   echo "${pom.artifactId}-${pom.version}.${pom.packaging}"
                   sh "mvn deploy:deploy-file  -DskipTests=true -DgroupId=${pom.groupId} -DartifactId=${pom.artifactId} -Dversion=${pom.version}  -DgeneratePom=true -Dpackaging=${pom.packaging}  -DrepositoryId=deploymentRepo -Durl=http://172.29.12.42:8089/repository/demoapp-release/  -Dfile=target/${pom.artifactId}-${pom.version}.${pom.packaging}"
                }
            }
        }
        
    }
}
