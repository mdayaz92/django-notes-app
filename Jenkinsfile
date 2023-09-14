pipeline {
    agent any
    stages {
        stage('Git checkout') {
            steps {
        
            echo "clone git"
          
            git branch: 'main', url: 'https://github.com/mdayaz92/django-notes-app.git'
            }
        }
        stage("build") {
            steps {
                echo "building the image"
                sh "docker build -t my-note-app ."
            }
        }
        stage ("push to docker hub") {
            steps {
                echo "pushing the code"
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',
                    usernameVariable: 'DOCKER_HUB_USERNAME',
                    passwordVariable: 'DOCKER_HUB_PASSWORD'
                )]){
                sh "docker tag my-note-app ${env.DOCKER_HUB_USERNAME}/my-note-app:latest2"
                sh "docker login -u ${env.DOCKER_HUB_USERNAME} -p ${env.DOCKER_HUB_PASSWORD}"
                sh "docker push ${env.DOCKER_HUB_USERNAME}/my-note-app:latest2"
                } 
            } 
        }
        stage ('deploy to container') {
            steps {
                echo "deploying to container"
                sh "docker-compose down && docker-compose up -d"
            }
        }

    }
}    
