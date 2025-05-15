pipeline {
    agent any

    environment {
        IMAGE = 'dockerram798/finance-me'
    }

    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Docker Build & Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                        docker build -t $IMAGE .
                        echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                        docker push $IMAGE
                    '''
                }
            }
        }

        stage('Deploy to EC2') {
            steps {
                sh 'ansible-playbook -i ansible/hosts ansible/deploy.yml'
            }
        }
    }
}
