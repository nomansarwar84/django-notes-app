pipeline {
    agent any
    
    stages{
        stage("Code"){
            steps {
                echo "Clonning the code"
                git url:"https://github.com/nomansarwar84/django-notes-app.git", branch: "nomansarwar84-patch-1"
            }
        }
        stage("Build"){
            steps {
                echo "Building the Code"
                sh "docker build . -t myapp-notes"
            }
        }
        stage("Push to Docker Hub"){
            steps {
                echo "Pushing the image to docker hub"
                withCredentials([usernamePassword(credentialsId:"dockerhub",passwordVariable:"dockerhubPass",usernameVariable:"dockerhubuser")]){
                sh "docker tag myapp-notes ${env.dockerhubuser}/myapp-notes:latest"    
                sh "docker login -u ${env.dockerhubuser} -p ${env.dockerhubpass}"
                sh "docker push ${env.dockerhubuser}/myapp-notes:latest"
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
