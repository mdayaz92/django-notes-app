pipeline {
    agent any 
    
    stages{
        stage("Clone Code"){
            steps {
                echo "Cloning the code"
                git url:"https://github.com/LondheShubham153/django-notes-app.git", branch: "main"
            }
        }
        stage("Build"){
            steps {
                echo "Building the image"
                sh "docker build -t my-note-app ."
            }
        }
        stage("Push to Docker Hub"){
            steps {
                echo "Pushing the image to docker hub"
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',
                    usernameVariable: 'DOCKER_HUB_USERNAME',
                    passwordVariable: 'DOCKER_HUB_PASSWORD'
                )]) {
                    
                sh "docker tag my-note-app ${env.DOCKER_HUB_USERNAME}/my-note-app:latest1"
                sh "docker login -u ${env.DOCKER_HUB_USERNAME} -p ${env.DOCKER_HUB_PASSWORD}"
                sh "docker push ${env.DOCKER_HUB_USERNAME}/my-note-app:latest1"
                }
            }
            
        }
        stage("Deploy"){
            steps {
                echo "Deploying the container"
                sh "docker-compose down && docker-compose up -d"
                
            }
        }
    }
}
