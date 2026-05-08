pipeline {
    agent any

    environment {
        IMAGE_NAME = "dayanandpy/pyapp"
        ID = "$BUILD_ID"
        
    }

    stages {
        stage('git checkout') {
            steps {
                git credentialsId: 'github-cred', url: 'https://github.com/dayanandagowda222/pyapp.git'
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
                withCredentials([usernamePassword(credentialsId: 'docker-cred', passwordVariable: 'pswd', usernameVariable: 'username')]) {
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

