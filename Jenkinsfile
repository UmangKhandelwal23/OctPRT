
pipeline {
    agent { label 'Slave01' }

    environment {
        DOCKERHUB_CREDENTIALS = credentials('docker')
    }

    stages {

        stage('git') {
            steps {
                git branch: 'master', url: 'http://github.com/UmangKhandelwal23/OctPRT'
            }
        }

        stage('docker build') {
            steps {
                sh 'sudo docker build -t umangkhandelwal/octprt:v1 .'
            }
        }

        stage('Docker Push') {
            steps {
                sh '''
                echo "$DOCKERHUB_CREDENTIALS_PSW" | sudo docker login -u "$DOCKERHUB_CREDENTIALS_USR" --password-stdin
                sudo docker push umangkhandelwal/octprt:v1
                '''
            }
        }

        stage('Container Run') {
            steps {
                sh '''
                sudo docker stop octprt-container || true
                sudo docker rm octprt-container || true
                sudo docker run -d --name octprt-container -p 80:80 umangkhandelwal/octprt:v1
                '''
            }
        }
    }
}
