pipeline {
    agent any

    stages {

        stage('Clone') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/arsalan9997/axion-telemetry-query-service.git'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t axion-telemetry:latest .'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    docker rm -f axion-api || true

                    docker run -d \
                      --name axion-api \
                      --network axion-network \
                      -p 8000:8000 \
                      -e DATABASE_URL='postgresql://axion_user:P%40ssw01rd%40123@axion-db:5432/axion_db' \
                      axion-telemetry:latest
                '''
            }
        }

        stage('Verify') {
            steps {
                sh 'sleep 5'
                sh 'curl -f http://localhost:8000/docs'
            }
        }
    }

    post {
        success {
            echo 'Deployment Successful!'
        }

        failure {
            echo 'Deployment Failed!'
        }
    }
}
