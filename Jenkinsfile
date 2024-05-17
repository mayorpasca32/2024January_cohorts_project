pipeline {
    agent any

    stages {   
        stage('Test') {
            steps {
                sh 'cd SampleWebApp && mvn test'
            }
        
        }

        stage('Build with maven') {
            steps {
                sh 'cd SampleWebApp && mvn clean install'
            }
        }
        
        
        stage('Quality Code Scan Analysis') {
           steps {
                  withSonarQubeEnv('sonar_server') {
             sh "mvn -f SampleWebApp/pom.xml sonar:sonar"     
               }
            }
       }

        
        stage('Push to Nexus Artifactory') {
            steps {
                nexusArtifactUploader artifacts: [[artifactId: 'SampleWebApp', classifier: '', file: 'SampleWebApp/target/SampleWebApp.war', type: 'war']], credentialsId: 'nexus_id', groupId: 'SampleWebApp', nexusUrl: '54.89.212.151:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven_snapshots', version: '1.0-SNAPSHOP'
            }   
            
        }
        
        stage('deploy to tomcat') {
          steps {
            deploy adapters: [tomcat9(credentialsId: 'tom_id', path: '', url: 'http://3.84.253.210:8080/')], contextPath: 'webapp', war: '**/*.war'
             
            }
            
        }
            
    }
} 
