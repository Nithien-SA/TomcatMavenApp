pipeline {
    agent any

    tools {
        maven 'M3'   // matches the name you configured under Global Tool Config
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
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
                // Stop and remove existing container
                sh 'docker stop C1 || true'
                sh 'docker rm C1 || true'

                // Copy the WAR file (name from pom.xml build output)
                sh 'cp target/TomcatMavenApp-2.0.war /tmp/'

                // Run Tomcat with WAR
                sh '''
                   docker run -d -p 8001:8080 \
                   -v /tmp/TomcatMavenApp-2.0.war:/usr/local/tomcat/webapps/TomcatMavenApp-2.0.war \
                   --name C1 tomcat:latest
                '''
            }
        }
    }
}
