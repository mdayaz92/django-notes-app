pipeline {
    agent any
    stages{
        stage ('git clone') {
            steps {
                echo "cloning the git"
                git url:"https://github.com/mdayaz92/django-notes-app.git",branch:"main"
            }
        }
        stage ('build') {
            steps {
                echo "building the image"
                sh "docker build -t myimage ."
            }
        }
        stage ('push') {
            steps {
                echo "pushing the image"
                 withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',
                    usernameVariable: 'DOCKER_HUB_USERNAME',
                    passwordVariable: 'DOCKER_HUB_PASSWORD'
                )]){
                sh "docker tag myimage ${env.DOCKER_HUB_USERNAME}/myimage:latest2"
                sh "docker login -u ${env.DOCKER_HUB_USERNAME} -p ${env.DOCKER_HUB_PASSWORD}"
                sh "docker push ${env.DOCKER_HUB_USERNAME}/myimage:latest2"
                } 
            }
        }
        stage ('deploy') {
            steps {
                echo "deploying to container"
                sh "docker-compose down && docker-compose up -d "
            }
        }
    }
}    
