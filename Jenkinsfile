pipeline {
    agent any

    stages {   
        stage('Build with maven') {
            steps {
                sh 'cd SampleWebApp && mvn clean install'
            }
        }
        
             stage('Test') {
            steps {
                sh 'cd SampleWebApp && mvn test'
            }
        
            }
      
        stage('Code Qualty Scan') {

           steps {
                  withSonarQubeEnv('sona') {
             sh "mvn -f SampleWebApp/pom.xml sonar:sonar"      
               }
            }
       }
       
       stage('Quality Gate') {
          steps {
                 waitForQualityGate abortPipeline: true
              }
        }
       stage('push to nexus') {
            steps {
                nexusArtifactUploader artifacts: [[artifactId: 'SampleWebApp', classifier: '', file: 'SampleWebApp/target/SampleWebApp.war', type: 'war']], credentialsId: 'nes_id_nexus', groupId: 'SampleWebApp', nexusUrl: 'ec2-3-133-110-110.us-east-2.compute.amazonaws.com:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-snapshots', version: '1.0-SNAPSHOT'
            }   
            
        }
        
        stage('deploy to tomcat') {
          steps {
              deploy adapters: [tomcat9(credentialsId: 'tomcat_id_tomcat', path: '', url: 'http://18.118.10.195:8080/')], contextPath: 'myapp', war: '**/*.war'
             
              
              
          }
            
        }
            
        }
} 
