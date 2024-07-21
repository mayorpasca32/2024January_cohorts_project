pipeline {
    agent any

    stages {
        stage('Test') {
            steps {
                sh 'cd SampleWebApp mvn test'
            }
        }
        stage('Build & Complie') {
            steps {
                sh 'cd SampleWebApp && mvn clean package'
            }
        }
        
        stage('Deploy to Tomcat Web Server') {
            steps {
                // Assuming Cargo plugin or similar tool is used for deployment
                sh '''
                    mvn cargo:redeploy -Dcargo.verbose=true
                '''
            }
            steps {
                deploy adapters: [tomcat9(credentialsId: 'sunday-deploy.2', path: '', url: 'http://localhost:9090/')], contextPath: 'app', war: '**/*.war'
            }
        }
    }
}
