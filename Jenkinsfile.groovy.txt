pipeline {

    agent any

    stages {

   stage('Clone') {
            steps {
                git url: 'https://github.com/Ganeshkiran123/ganeshkiranr.git',
                    branch: 'main'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t myapp:v1 .'
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                docker stop myapp || true
                docker rm myapp || true

                docker run -d \
                --name myapp \
                -p 80:80 \
                myapp:v1
                '''
            }
        }
    }
}
