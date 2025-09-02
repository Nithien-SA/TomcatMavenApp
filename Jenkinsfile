pipeline {
    agent any

    tools {
        maven 'M3'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/<your-username>/<repo-name>.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker stop C1 || true'
                sh 'docker rm C1 || true'
                sh 'cp target/*.war /tmp/'
                sh '''
                   docker run -d -p 8001:8080 \
                   -v /tmp/*.war:/usr/local/tomcat/webapps/app.war \
                   --name C1 tomcat:latest
                '''
            }
        }
    }
}
