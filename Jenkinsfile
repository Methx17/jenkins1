pipeline {
    agent any

    environment {
        IMAGE_NAME = "methx18/jenkins"
        ID = "$BUILD_ID"
        
    }

    stages {
        stage('git checkout') {
            steps {
                git 'https://github.com/Methx17/jenkins1.git'
            }
        }
        stage('test and install') {
            steps {
                sh 'pip install -r requirements.txt'
                sh 'python3 -m pytest'
            }
        }
        stage('build image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$ID .'
                sh 'docker tag $IMAGE_NAME:$ID $IMAGE_NAME:latest'
                
            }
        }
        stage('docker login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker_cred', passwordVariable: 'pswd', usernameVariable: 'username')]) {
                sh  'echo "$pswd" | docker login -u $username --password-stdin'
                }
            }
        }
        
        stage('push image') {
            steps {
                sh 'docker push $IMAGE_NAME:$ID'
                sh 'docker push $IMAGE_NAME:latest'
            }
        }

        stage('deploy') {
            steps {
                sh 'docker compose up -d --build'
            }
        }
        
    }
}
