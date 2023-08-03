pipeline{
    agent any
    
    stages{
        stage("Code"){
            steps {
                echo "Clonning the code from git hub"
                git url:"https://github.com/nomansarwar84/django-notes-app.git", branch: "main"
            }
        }
        stage("Build"){
            steps {
                echo "Building the code image"
                sh "docker build . -t my-notes-app"
            }
        }
        stage("Push to Docker hub"){
            steps {
                echo "Pushing the image to docker Hub"
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker tag my-notes-app ${env.dockerHubUser}/my-notes-app:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/my-notes-app:latest"
                }
            }
        }
        stage("Deploy"){
            steps {
                echo "Deploying the code"
                sh "docker run -d -p 8000:8000 nomansarwar84/my-notes-app:latest"
            }
        }
    }     
}
