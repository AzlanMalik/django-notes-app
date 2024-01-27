pipeline {
    agent any 
    
    stages{
        stage("Cloning the repo"){
            steps {
                echo "Cloning the repo"
                git url:"https://github.com/AzlanMalik/django-notes-app.git", branch: "main"
            }
        }
        stage("Build stage"){
            steps {
                echo "Building the image"
                sh "docker build -t notes-app ."
            }
        }
        stage("Push to Docker Hub"){
            steps {
                echo "Pushing the image to docker hub"
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker tag notes-app ${env.dockerHubUser}/notes-app:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/notes-app:latest"
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
