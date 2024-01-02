pipeline {
    agent any
    stages {
        stage('Verify') {
            steps {
                bat '''
                    docker version
                    docker-compose version
                '''
            }
        }
        stage('Build') {
            steps {
                bat 'docker-compose build'
            }
        }
        stage('Test') {
            steps {
                bat 'docker container run pi:bday7 -dp 7'
            }
        }
        stage('Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'HUB_USER', passwordVariable: 'HUB_TOKEN')]) {                      
                    bat '''
                        docker login -u $HUB_USER -p $HUB_TOKEN 
                        docker image tag pi:bday7 $HUB_USER/devop:test
                        docker image push $HUB_USER/devop:test
			docker logout
                    '''
                }
            }
        }
    }
}